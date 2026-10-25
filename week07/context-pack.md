# CONTEXT_PACK — 운동 기록 앱 다음 작업

## 다음 작업 (하나)
**벤치프레스 한 종목의 기록 화면을 만든다.**
무게·횟수를 입력해 세트를 추가하고, 잘못된 입력은 막고, 기록이 없으면 안내 문구를, 기록이 있으면 직전 기록을 보여 준다.

- 대상 AC: proposal.md의 AC1(정상 저장), AC2(잘못된 입력), AC3(빈 상태), AC4(직전 기록)
- 이번 작업에서 제외: AC5(종목 추가), 화면 간 이동, 기기 저장

## 작업 범위와 제약
- 수정 대상 파일: `lib/main.dart` 하나 (새 Flutter 프로젝트)
- 상태 관리: `StatefulWidget` + `setState` (외부 상태관리 패키지 사용 안 함)
- 저장: 메모리 리스트만 사용 → 앱 종료 시 기록이 사라짐 (proposal.md 남은 질문 참고)
- 서버·비동기 기능 사용 안 함
- 계산할 수 있는 값(직전 기록)은 따로 저장하지 않고 getter로 계산한다

## 상태 설계 (초안)
| 구분 | 이름 | 내용 |
|---|---|---|
| 데이터 | `WorkoutSet` | `double weight`, `int reps`, `DateTime date` |
| 원본 상태 | `sets` | `List<WorkoutSet>`, 최신 기록이 맨 앞 |
| 계산값 | `latestSet` | `sets.isEmpty ? null : sets.first` (getter) |
| 이벤트 | `_addSet()` | 입력 검증 → 통과하면 `setState` 안에서 `sets.insert(0, ...)` |
| 오류 상태 | `_errorMessage` | 검증 실패 시 문구, 성공 시 `null` |

## 자료표

| # | 자료 | 위치 | 이번 작업에서 쓰는 부분 | 확인 상태 |
|---|---|---|---|---|
| 1 | 앱 제안서 | `week07/proposal.md` 8~9절 | 데이터 규칙(무게 0 초과 500 이하, 횟수 1~100), AC1~AC4 | 직접 작성·확인 |
| 2 | 3주차 목표 추가 코드 | [week03_goal_lab/lib/main.dart](https://github.com/chuns3hun/week03_goal_lab/blob/d318d04/lib/main.dart) 47~100행 | `TextEditingController`, `trim()` 후 빈 값 차단, `errorText`로 오류 표시 → AC2 | 저장소에서 확인 (커밋 `d318d04`) |
| 3 | 3주차 기능 컨텍스트 | [week03_goal_lab/FEATURE_CONTEXT.md](https://github.com/chuns3hun/week03_goal_lab/blob/d318d04/FEATURE_CONTEXT.md) | Given-When-Then AC 작성 방식, 빈 입력 예외 처리 | 저장소에서 확인 |
| 4 | 4주차 위젯 화면 코드 | [week04_widget_lab/lib/main.dart](https://github.com/chuns3hun/week04_widget_lab/blob/138f2c6/lib/main.dart) 98~101행 | `Expanded` + `isEmpty`면 안내 문구, 아니면 `ListView.builder` → AC1, AC3 | 저장소에서 확인 (커밋 `138f2c6`) |
| 5 | 4주차 위젯 선택표 | [week04_widget_lab/README.md](https://github.com/chuns3hun/week04_widget_lab/blob/138f2c6/README.md) | Card·ListTile로 기록 한 줄 표시, Flutter 공식 문서 링크 | 저장소에서 확인 |
| 6 | 5주차 상태 계약 | [week05_state_lab/STATE_CONTRACT.md](https://github.com/chuns3hun/week05_state_lab/blob/22ccef8/STATE_CONTRACT.md) | 원본 상태와 계산값 구분 방식 → `latestSet` getter 설계 | 저장소에서 확인 (커밋 `22ccef8`) |
| 7 | 5주차 상태 코드 | [week05_state_lab/lib/main.dart](https://github.com/chuns3hun/week05_state_lab/blob/22ccef8/lib/main.dart) 37~41행 | getter로 계산값 만들기, `setState` 안에서 원본 상태 변경 → AC4 | 저장소에서 확인 |
| 8 | Dart 숫자 변환 | [double.tryParse 공식 문서](https://api.dart.dev/stable/dart-core/double/tryParse.html), [int.tryParse 공식 문서](https://api.dart.dev/stable/dart-core/int/tryParse.html) | 문자 입력 시 `null` 반환 → AC2의 `abc` 입력 차단 | **미확인** — 이전 실습 코드에 없음. 링크를 아직 직접 열어 보지 않았으므로 구현 전 문서 확인 필요 |

## AI에게 이 작업을 요청할 때 함께 줄 자료
- 이 파일(`context-pack.md`)과 `proposal.md`
- 자료 2, 4, 7의 코드 일부 (전체 파일이 아니라 위 행 범위만)
- 이유: 5주차 PROMPT_COMPARISON에서 완성된 코드 전체를 주면 응답이 입력을 거의 그대로 반복해 비교가 어려웠음. 이번에는 패턴만 주고 새 코드를 요청한다.

## 완료 기준
- AC1~AC4를 실제 실행 화면으로 확인하고 캡처를 `evidence/`에 저장
- `flutter analyze` 결과 `No issues found`
- 실행하지 않은 항목은 "미확인"으로 기록

## 남은 질문
- 6~7주차 실습 저장소가 공개 목록에 없음 → 이번 작업과 관련된 자료가 있으면 자료표에 추가 필요
- 기기 저장 방식은 수업 진도 확인 후 결정 (이번 작업 범위 밖)
