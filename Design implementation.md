# Implementation Requirements

본 장에서는 '웹 기반 루프스테이션 시스템(Web-based Loop Station System)'을 실제 동작하는 프로토타입 및 상용 웹 소프트웨어로 구현하기 위해 충족해야 하는 상세 기술 명세 및 제약사항을 정의한다.

5.1 Software Environment Requirements (소프트웨어 환경 요구사항)
5.1.1 Client Runtime & Web Standard Browser
사용자가 별도의 네이티브 플러그인(ActiveX, 확장 프로그램 등) 설치 없이 브라우저 기본 내장 자원만으로 전문 음악 장비 수준의 컴퓨팅을 처리할 수 있도록, W3C 웹 표준 명세를 완벽히 지원하는 런타임 환경을 요구한다.

표준 API 준수 사양: HTML5 MediaDevices 명세 및 Web Audio API 레벨 2 스펙 표준을 완벽히 지원해야 한다.

대상 브라우저: Chromium 기반 브라우저(Google Chrome v100 이상, Microsoft Edge v100 이상) 및 WebKit 기반 브라우저(Apple Safari v15 이상), Mozilla Firefox 환경에서 크로스 브라우징이 보장되어야 한다.

5.1.2 Development Tech Stack & Libraries
Frontend Framework: 다중 트랙의 오디오 재생 상태, 실시간 녹음 상태(LED 온/오프), 이펙터 파라미터 믹싱 등 빈번한 UI 리렌더링과 단방향 데이터 흐름을 효율적으로 관리하기 위해 컴포넌트 기반 프론트엔드 라이브러리/프레임워크(React.js 또는 Vue.js 등)를 활용한다.

Audio Core: 원시 웹 오디오 제어를 위해 브라우저 내장 AudioContext 객체를 싱글톤(Singleton) 형태로 래핑하여 오디오 소스 노드와 물리 출력단 사이의 오디오 가공 파이프라인(Audio Graph)을 구축한다.

Backend API Server: RESTful 아키텍처 규칙을 준수하는 백엔드 프레임워크(Node.js Express, Spring Boot, 또는 Python FastAPI 등)를 통해 비동기 상태 전환 요청을 처리한다.

Database & 영속성 스토리지:

RDBMS/NoSQL: 사용자 계정(User Schema) 및 프로젝트의 메타데이터(BPM, FX ID, 트랙 배치 구조) 정보 관리를 위해 관계형 데이터베이스(MySQL) 또는 문서 기반 데이터베이스(MongoDB)를 구축한다.

File Storage: 녹음된 트랙별 원시 바이너리 오디오 파일(Blob 형태의 .wav 혹은 .webm 데이터)을 저장하기 위해 AWS S3 혹은 로컬 파일 시스템 아키텍처 구조의 전용 스토리지 인프라를 연결해야 한다.

5.2 Non-Functional Requirements (비기능적 요구사항)
5.2.1 Real-time Audio Processing & Latency Optimization
웹 브라우저의 메인 스레드와 오디오 연산 스레드 간의 병목 현상을 방지하고 입출력 지연 시간을 최소화하기 위한 요구사항이다.

오디오 지연 시간 제어: 오디오 신호가 마이크 입력단을 거쳐 버퍼링된 후 스피커로 출력되기까지의 왕복 지연 시간(Round-trip Latency)을 20ms 이하로 유지해야 한다.

스레드 분리 제약: 실시간 음성 가공 및 오디오 믹싱 시 UI 렌더링 스레드가 멈추는 오디오 글리치(Audio Glitch, 소리 끊김) 현상을 방지하기 위해, 오디오 스트림 연산은 메인 스레드와 분리된 전용 오디오 워커 스레드(AudioWorkletProcessor) 환경에서 비동기 처리되도록 구조화해야 한다.

5.2.2 High-Precision Track Synchronization (정밀 트랙 동기화)
루프스테이션의 생명인 다중 트랙의 박자 일치성을 확보하기 위한 타이밍 요구사항이다.

타이머 제약 사양: JavaScript의 기본 타이머(setTimeout, setInterval)는 웹 브라우저의 백그라운드 탭 비활성화 시 주기가 늘어지는 하드웨어 제약이 있으므로 절대 사용을 금지한다.

오디오 타임스탬프 동기화: 마이크 녹음이 완료되어 Track 객체가 추가되거나 AudioPlayer.play()가 실행될 때, 시스템 가상 메트로놈 가속 타이머는 브라우저 오디오 하드웨어 시계의 절대적인 시간축인 AudioContext.currentTime 타임스탬프 값에 엄격하게 정렬되어 다중 오디오 버퍼가 동일 샘플 레벨에서 동기 재생되도록 코딩되어야 한다.

5.2.3 Security & Actor Role Guard
인증 메커니즘: 사용자 인증 관리는 상태 비저장(Stateless) 방식의 JWT(JSON Web Token) 아키텍처 또는 세션 쿠키 메커니즘을 사용한다.

라우터 및 API 가드: Guest 권한 접근 시에는 메모리 기반의 일시적 루프 생성/재생만 허용하고, 데이터베이스 및 물리 저장소 인터페이스가 동작하는 핵심 비즈니스 로직 메소드(saveProject(), loadProject(), exportSetting(), importSetting()) 호출 시에는 반드시 프론트엔드 라우터 가드(Router Guard)와 백엔드 미들웨어 인증 필터를 거쳐 클라이언트 토큰의 유효성과 userRole == User 세션 권한을 검증하도록 차단 처리해야 한다.

5.2.Performance & Serialization Spec
응답 성능 제약: 프로젝트 목록을 서버로부터 가져오거나(loadProject), 대용량 멀티 트랙 데이터를 압축하여 데이터베이스에 트랜잭션을 전송 및 커밋(saveProject)하는 전체 네트워크 I/O 병목 시간은 사용자의 UX 유지를 위해 최대 3초 이내에 완결되어야 한다.

데이터 직렬화: 설정 파일 공유 기능을 위해 현재 프로젝트의 정보(BPM 설정값, 개별 트랙 식별자, 트랙별 볼륨 및 믹서 패닝 값, 슬롯별 적용된 AudioEffect 파라미터 구조체)를 텍스트 기반의 표준 형식인 JSON(JavaScript Object Notation) 형태로 완벽하게 직렬화(Serialize) 및 역직렬화할 수 있는 데이터 포맷팅 규칙을 정의해야 한다.

5.3 Hardware Interface Requirements (하드웨어 인터페이스 요구사항)
5.3.1 Audio Input Interface & Browser User Gesture Policy
마이크 스트림 획득: 사용자의 물리적 입력 장치와 결합하기 위해 HTML5 navigator.mediaDevices.getUserMedia({ audio: true, video: false }) 인터페이스 파이프라인을 호출하여 오디오 미디어 스트림 트랙(MediaStreamAudioSourceNode)을 확보해야 한다.

브라우저 오디오 정책(Autoplay Policy) 대응 예외 처리: 최신 모던 브라우저 보안 규정상 사용자의 직접적인 마우스 클릭이나 키보드 입력 같은 '사용자 제스처(User Gesture)'가 발생하기 전에는 AudioContext가 강제로 suspended(정지) 상태로 잠겨 소리가 나지 않는다. 따라서 시스템은 사용자가 최초 화면 진입 후 '시작하기' 혹은 '로그인' 버튼을 클릭하는 최초의 UI 상호작용 이벤트를 캡처하여 AudioContext.resume() 메소드를 명시적으로 호출해 하드웨어 락을 해제해 주는 초기화 핸들러 로직을 반드시 포함해야 한다.

5.3.2 Microphone Permission Exception Handling
권한 거부 상태 예외 처리: 클라이언트가 시스템 실행 혹은 녹음 진입 시 브라우저 권한 팝업창에서 마이크 접근 권한을 거부(NotAllowedError, Permission Denied)한 경우, 시스템 내부 catch 블록은 이를 즉시 감지해야 한다.

UI 컴포넌트 폴백 제어: 마이크 권한 거부 예외가 발생하면 즉시 사용자에게 "루프 녹음을 위해서는 마이크 설정 권한이 필요합니다"라는 안내 모달창을 띄우고, 메인 화면 UI 상의 startRecording()을 호출하는 녹음 시작 컴포넌트 버튼 객체의 HTML 속성을 disabled=true 상태로 변경하여 하드웨어 접근 불가로 인한 자바스크립트 런타임 크래시 오류를 원천 차단해야 한다.

5.3.3 Audio Output Routing Interface
물리 장치 라우팅: AudioPlayer 클래스가 로드한 다중 Track의 가공된 오디오 샘플 데이터스트림과 메인 가상 이펙터 노드(GainNode, BiquadFilterNode, ConvolverNode 등)의 최종 결합 출력단은 브라우저 커널이 시스템 사운드 드라이버와 통신하는 최상위 출력 인터페이스 노드인 AudioContext.destination으로 하드웨어 라우팅 파이프라인을 최종 연결시켜 스피커 또는 이어폰 장치로 실제 물리 파형 사운드가 흐르도록 구현해야 한다.