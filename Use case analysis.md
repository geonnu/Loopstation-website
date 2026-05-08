Use case analysis

2.1 Use case diagram
![alt text](<제목 없음.png>)

Conceptualization Document에서 정의했던 Use Case List를 바탕으로 위와 같은 Use Case Diagram을 도출하였다. 모든 Use Case는 사용자의 행위를 기준으로 동사 형태로 Naming하였다.

본 시스템의 Actor는 Guest와 User로 구성하였다. Guest는 비회원 사용자를 의미하며, User는 로그인한 회원 사용자를 의미한다. User는 Guest의 기능을 포함하는 형태로 generalization 관계를 사용하였다. 이를 통해 회원 사용자는 비회원이 사용할 수 있는 기본 기능 또한 사용할 수 있도록 구성하였다.

Guest 사용자는 로그인하지 않아도 기본적인 루프 기능을 체험할 수 있도록 하였다. 따라서 Guest는 회원가입, 로그인, 마이크 입력 활성화 기능에 접근할 수 있으며, 마이크 입력 활성화를 기반으로 루프 생성 기능을 사용할 수 있다. 또한 생성된 루프를 기반으로 BPM 설정, 효과 적용, 루프 재생 제어, 루프 삭제 기능을 사용할 수 있도록 구성하였다.

반면 User는 로그인된 사용자를 의미하며, 루프 프로젝트 저장, 이전 루프 조회, 루프 프로젝트 불러오기, 설정 파일 업로드 및 내보내기와 같은 계정 기반 기능을 사용할 수 있도록 구성하였다.

Use Case 간의 관계는 기능 간 의존성을 기준으로 표현하였다. 루프 생성 기능은 마이크 입력 권한이 반드시 필요하므로 Activate Microphone Input과 include 관계로 표현하였다. 또한 루프 재생 제어와 루프 삭제 기능은 생성된 루프가 존재해야 사용할 수 있으므로 Create Loop와의 관계를 기반으로 구성하였다.


## 2.3 Use Case Description

### Use Case #1 : Register

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 비회원 사용자가 회원 계정을 생성한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update | |
| Status | Analysis |
| Primary Actor | Guest |
| Preconditions | 사용자가 회원 계정을 가지고 있지 않은 상태이다. |
| Trigger | 사용자가 회원가입 버튼을 클릭한다. |
| Success Post Condition | 사용자 계정이 생성되어 로그인할 수 있다. |
| Failed Post Condition | 회원가입에 실패하여 계정이 생성되지 않는다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 회원가입 버튼을 클릭한다. |
| 2 | 시스템이 회원가입 화면을 보여준다. |
| 3 | 사용자가 ID, Password 등 필요한 정보를 입력한다. |
| 4 | 시스템이 입력된 정보를 확인한다. |
| 5 | 시스템이 사용자 계정을 생성한다. |
| 6 | 회원가입 완료 메시지를 보여준다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 3a | 필수 정보가 입력되지 않은 경우 |
| 3a.1 | 시스템은 필수 정보를 입력하라는 메시지를 보여준다. |
| 4a | 이미 존재하는 ID인 경우 |
| 4a.1 | 시스템은 중복된 ID라는 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | None |
| Due Date |  Kim geonwoo |
| Etc | None |


### Use Case #2 : Login

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 회원 사용자가 자신의 계정으로 로그인한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest |
| Secondary Actors | Server |
| Preconditions | 사용자가 회원가입을 완료한 상태이다. |
| Trigger | 사용자가 ID와 Password를 입력하고 로그인 버튼을 클릭한다. |
| Success Post Condition | 사용자가 로그인되어 회원 전용 기능을 사용할 수 있다. |
| Failed Post Condition | 로그인이 실패하여 회원 전용 기능을 사용할 수 없다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 로그인 버튼을 클릭한다. |
| 2 | 시스템이 로그인 화면을 보여준다. |
| 3 | 사용자가 ID와 Password를 입력한다. |
| 4 | 사용자가 로그인 버튼을 클릭한다. |
| 5 | 시스템이 입력된 정보를 서버의 회원 정보와 비교한다. |
| 6 | 로그인 성공 메시지를 보여주고 회원 상태로 전환한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 3a | ID 또는 Password가 입력되지 않은 경우 |
| 3a.1 | 시스템은 입력값이 필요하다는 메시지를 보여준다. |
| 5a | ID 또는 Password가 일치하지 않는 경우 |
| 5a.1 | 시스템은 로그인 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | None |
| Due Date |    |
| Etc | None |


### Use Case #3 : Activate Microphone Input

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 루프를 생성하기 위해 마이크 입력을 활성화한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest, User |
| Secondary Actors | Browser |
| Preconditions | 사용자가 웹 브라우저를 통해 시스템에 접속한 상태이다. |
| Trigger | 사용자가 마이크 입력 활성화 버튼을 클릭한다. |
| Success Post Condition | 마이크 입력이 활성화되어 루프 생성 기능을 사용할 수 있다. |
| Failed Post Condition | 마이크 권한이 허용되지 않아 루프 생성 기능을 사용할 수 없다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 시스템에 접속한다. |
| 2 | 사용자가 마이크 입력 활성화 버튼을 클릭한다. |
| 3 | 브라우저가 마이크 권한 요청 창을 보여준다. |
| 4 | 사용자가 마이크 사용을 허용한다. |
| 5 | 시스템은 마이크 입력을 받을 수 있는 상태로 전환된다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 3a | 사용자가 마이크 권한을 거부한 경우 |
| 3a.1 | 시스템은 마이크 권한이 필요하다는 메시지를 보여준다. |
| 3a.2 | 사용자는 루프 생성 기능을 사용할 수 없다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | None |
| Due Date |    |
| Etc | None |


### Use Case #4 : Create Loop

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 마이크 입력을 이용하여 루프를 생성한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest, User |
| Preconditions | 마이크 입력이 활성화된 상태이다. |
| Trigger | 사용자가 루프 생성 버튼을 클릭한다. |
| Success Post Condition | 새로운 루프가 생성되어 재생 가능한 상태가 된다. |
| Failed Post Condition | 루프 생성에 실패한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 마이크 입력을 활성화한다. |
| 2 | 사용자가 루프 생성 버튼을 클릭한다. |
| 3 | 시스템이 마이크 입력을 녹음한다. |
| 4 | 사용자가 녹음을 종료한다. |
| 5 | 시스템이 녹음된 데이터를 루프로 생성한다. |
| 6 | 생성된 루프를 재생 가능한 상태로 표시한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 3a | 마이크 입력이 정상적으로 감지되지 않는 경우 |
| 3a.1 | 시스템이 마이크 입력 오류 메시지를 출력한다. |
| 3a.2 | 루프 생성이 취소된다. |
| 5a | 루프 데이터 생성에 실패한 경우 |
| 5a.1 | 시스템이 루프 생성 실패 메시지를 출력한다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | Multiple Track Support |
| Due Date |    |
| Etc | None |


### Use Case #5 : Set BPM

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 루프의 BPM을 설정하거나 변경한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest, User |
| Preconditions | 시스템이 루프 생성 가능한 상태이다. |
| Trigger | 사용자가 BPM 값을 입력하거나 조절한다. |
| Success Post Condition | 설정한 BPM 값이 루프 재생 기준으로 적용된다. |
| Failed Post Condition | BPM 값이 적용되지 않는다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 BPM 설정 영역을 선택한다. |
| 2 | 사용자가 원하는 BPM 값을 입력하거나 조절한다. |
| 3 | 시스템이 입력된 BPM 값을 확인한다. |
| 4 | 시스템이 BPM 값을 루프 재생 기준으로 적용한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 2a | 사용자가 유효하지 않은 BPM 값을 입력한 경우 |
| 2a.1 | 시스템은 올바른 BPM 값을 입력하라는 메시지를 보여준다. |
| 4a | BPM 적용 중 오류가 발생한 경우 |
| 4a.1 | 시스템은 BPM 적용 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 1 Second |
| Frequency | Variable |
| Concurrency | Multiple Track Synchronization |
| Due Date |    |
| Etc | None |


### Use Case #6 : Apply Audio Effects

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 루프에 Reverb, EQ 등의 오디오 효과를 적용한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest, User |
| Preconditions | 효과를 적용할 루프 또는 트랙이 존재해야 한다. |
| Trigger | 사용자가 적용할 오디오 효과를 선택한다. |
| Success Post Condition | 선택한 오디오 효과가 루프에 적용된다. |
| Failed Post Condition | 오디오 효과가 적용되지 않는다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 효과를 적용할 트랙을 선택한다. |
| 2 | 사용자가 Reverb, EQ 등의 오디오 효과를 선택한다. |
| 3 | 시스템이 선택한 효과 설정을 해당 트랙에 적용한다. |
| 4 | 사용자는 효과가 적용된 루프를 재생하여 확인한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 1a | 효과를 적용할 루프가 없는 경우 |
| 1a.1 | 시스템은 루프가 필요하다는 메시지를 보여준다. |
| 3a | 효과 적용 중 오류가 발생한 경우 |
| 3a.1 | 시스템은 효과 적용 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 2 Seconds |
| Frequency | Variable |
| Concurrency | Multiple Track Support |
| Due Date |    |
| Etc | Basic audio effects only |


### Use Case #7 : Control Loop Playback

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 생성된 루프의 재생 및 정지를 제어한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest, User |
| Preconditions | 생성된 루프가 존재해야 한다. |
| Trigger | 사용자가 재생 또는 정지 버튼을 클릭한다. |
| Success Post Condition | 루프가 정상적으로 재생 또는 정지된다. |
| Failed Post Condition | 루프 재생 제어에 실패한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 생성된 루프를 확인한다. |
| 2 | 사용자가 재생 버튼을 클릭한다. |
| 3 | 시스템이 루프를 BPM 기준에 맞게 재생한다. |
| 4 | 사용자가 정지 버튼을 클릭한다. |
| 5 | 시스템이 루프 재생을 종료한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 2a | 생성된 루프가 존재하지 않는 경우 |
| 2a.1 | 시스템이 루프가 존재하지 않는다는 메시지를 출력한다. |
| 3a | 루프 동기화에 실패한 경우 |
| 3a.1 | 시스템이 동기화 오류 메시지를 출력한다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 1 Second |
| Frequency | Frequent |
| Concurrency | Multiple Track Playback |
| Due Date |    |
| Etc | None |


### Use Case #8 : Delete Loop

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 생성된 루프를 삭제한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | Guest, User |
| Preconditions | 삭제할 루프가 존재해야 한다. |
| Trigger | 사용자가 정지 버튼을 길게 누르거나 연속으로 입력한다. |
| Success Post Condition | 선택한 루프가 삭제된다. |
| Failed Post Condition | 루프 삭제에 실패한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 삭제할 루프가 있는 트랙을 확인한다. |
| 2 | 사용자가 정지 버튼을 길게 누르거나 연속으로 입력한다. |
| 3 | 시스템이 삭제 요청을 인식한다. |
| 4 | 시스템이 해당 트랙의 루프 데이터를 삭제한다. |
| 5 | 시스템이 삭제 완료 상태를 표시한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 2a | 삭제 입력이 정상적으로 인식되지 않은 경우 |
| 2a.1 | 시스템은 루프를 유지한다. |
| 4a | 삭제 중 오류가 발생한 경우 |
| 4a.1 | 시스템이 삭제 실패 메시지를 출력한다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 1 Second |
| Frequency | Variable |
| Concurrency | None |
| Due Date |    |
| Etc | Stop button long press or repeated input |


### Use Case #9 : Save Loop Project

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 생성한 루프 프로젝트를 저장한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | User |
| Secondary Actors | Server |
| Preconditions | 사용자가 로그인한 상태이며 저장할 루프 프로젝트가 존재해야 한다. |
| Trigger | 사용자가 프로젝트 저장 버튼을 클릭한다. |
| Success Post Condition | 루프 프로젝트가 서버에 저장된다. |
| Failed Post Condition | 프로젝트 저장에 실패한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 로그인된 상태로 루프 프로젝트를 생성한다. |
| 2 | 사용자가 프로젝트 저장 버튼을 클릭한다. |
| 3 | 시스템이 루프 데이터, BPM, 트랙 구성, 효과 설정값을 수집한다. |
| 4 | 시스템이 프로젝트 정보를 서버에 저장한다. |
| 5 | 시스템이 저장 완료 메시지를 출력한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 1a | 사용자가 로그인하지 않은 상태인 경우 |
| 1a.1 | 시스템이 로그인 화면으로 이동시킨다. |
| 4a | 서버 저장 중 오류가 발생한 경우 |
| 4a.1 | 시스템이 저장 실패 메시지를 출력한다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | Multiple User Support |
| Due Date |    |
| Etc | None |


### Use Case #10 : View Previous Projects

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 이전에 저장한 루프 프로젝트 목록을 조회한다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | User |
| Secondary Actors | Server |
| Preconditions | 사용자가 로그인한 상태이며 저장된 프로젝트가 존재할 수 있다. |
| Trigger | 사용자가 이전 루프 조회 메뉴를 선택한다. |
| Success Post Condition | 저장된 루프 프로젝트 목록이 화면에 표시된다. |
| Failed Post Condition | 프로젝트 목록을 조회하지 못한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 이전 루프 조회 메뉴를 클릭한다. |
| 2 | 시스템이 사용자 계정 정보를 확인한다. |
| 3 | 시스템이 서버에서 해당 사용자의 저장 프로젝트 목록을 가져온다. |
| 4 | 시스템이 프로젝트 목록을 화면에 표시한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 3a | 저장된 프로젝트가 없는 경우 |
| 3a.1 | 시스템은 저장된 프로젝트가 없다는 메시지를 보여준다. |
| 3b | 서버 조회에 실패한 경우 |
| 3b.1 | 시스템은 조회 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | Multiple User Support |
| Due Date |    |
| Etc | None |


### Use Case #11 : Load Loop Project

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 저장된 루프 프로젝트를 불러온다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | User |
| Secondary Actors | Server |
| Preconditions | 사용자가 로그인한 상태이며 불러올 프로젝트가 존재해야 한다. |
| Trigger | 사용자가 저장된 프로젝트 중 하나를 선택한다. |
| Success Post Condition | 선택한 루프 프로젝트가 작업 화면에 불러와진다. |
| Failed Post Condition | 프로젝트를 불러오지 못한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 이전 루프 프로젝트 목록을 조회한다. |
| 2 | 사용자가 불러올 프로젝트를 선택한다. |
| 3 | 시스템이 서버에서 해당 프로젝트 데이터를 가져온다. |
| 4 | 시스템이 트랙 구성, BPM, 효과 설정값을 작업 화면에 반영한다. |
| 5 | 사용자는 불러온 프로젝트를 이어서 작업할 수 있다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 2a | 사용자가 프로젝트 선택을 취소한 경우 |
| 2a.1 | 시스템은 기존 화면을 유지한다. |
| 3a | 프로젝트 데이터 불러오기에 실패한 경우 |
| 3a.1 | 시스템은 불러오기 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | Multiple User Support |
| Due Date |    |
| Etc | None |


### Use Case #12 : Export Setting File

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 루프 프로젝트의 설정값을 파일로 내보낸다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | User |
| Preconditions | 사용자가 로그인한 상태이며 내보낼 루프 프로젝트가 존재해야 한다. |
| Trigger | 사용자가 설정 파일 내보내기 버튼을 클릭한다. |
| Success Post Condition | 루프 설정 파일이 생성된다. |
| Failed Post Condition | 설정 파일 내보내기에 실패한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 설정 파일 내보내기 버튼을 클릭한다. |
| 2 | 시스템이 현재 프로젝트의 BPM, 트랙 구성, 효과 설정값을 수집한다. |
| 3 | 시스템이 설정값을 파일 형태로 변환한다. |
| 4 | 사용자는 생성된 설정 파일을 저장한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 2a | 내보낼 프로젝트 설정값이 없는 경우 |
| 2a.1 | 시스템은 내보낼 설정값이 없다는 메시지를 보여준다. |
| 3a | 파일 변환에 실패한 경우 |
| 3a.1 | 시스템은 내보내기 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | None |
| Due Date |    |
| Etc | Export project settings only |


### Use Case #13 : Upload Setting File

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 설정 파일을 업로드하여 루프 구성을 불러온다. |
| Scope | Web-based Loop Station System |
| Level | User Level |
| Author | Kim geonwoo |
| Last Update |    |
| Status | Analysis |
| Primary Actor | User |
| Preconditions | 사용자가 로그인한 상태이며 업로드할 설정 파일이 존재해야 한다. |
| Trigger | 사용자가 설정 파일 업로드 버튼을 클릭하고 파일을 선택한다. |
| Success Post Condition | 설정 파일의 루프 구성이 시스템에 반영된다. |
| Failed Post Condition | 설정 파일 업로드 또는 적용에 실패한다. |

| MAIN SUCCESS SCENARIO |  |
|---|---|
| Step | Action |
| 1 | 사용자가 설정 파일 업로드 버튼을 클릭한다. |
| 2 | 사용자가 업로드할 설정 파일을 선택한다. |
| 3 | 시스템이 파일 형식과 내용을 확인한다. |
| 4 | 시스템이 BPM, 트랙 구성, 효과 설정값을 작업 화면에 반영한다. |
| 5 | 사용자는 업로드된 설정을 기반으로 루프 작업을 진행한다. |

| EXTENSION SCENARIOS |  |
|---|---|
| Step | Branching Action |
| 2a | 사용자가 파일 선택을 취소한 경우 |
| 2a.1 | 시스템은 기존 화면을 유지한다. |
| 3a | 파일 형식이 올바르지 않은 경우 |
| 3a.1 | 시스템은 지원하지 않는 파일이라는 메시지를 보여준다. |
| 4a | 설정값 적용 중 오류가 발생한 경우 |
| 4a.1 | 시스템은 설정 적용 실패 메시지를 보여준다. |

| RELATED INFORMATION |  |
|---|---|
| Performance | ≤ 3 Seconds |
| Frequency | Variable |
| Concurrency | None |
| Due Date |    |
| Etc | Upload project settings only |