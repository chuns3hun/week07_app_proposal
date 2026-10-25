# CONTEXT_PACK — 운동 기록 앱 다음 작업

## 1. 목표 (다음 작업 하나)
**벤치프레스 한 종목의 기록 화면을 만든다.**
무게·횟수를 입력해 세트를 추가하고, 잘못된 입력은 막고, 기록이 없으면 안내 문구를, 기록이 있으면 직전 기록을 보여 준다.

- 대상 앱 AC: proposal.md의 AC1(정상 저장), AC2(잘못된 입력), AC3(빈 상태), AC4(직전 기록)
- 이번 작업에서 제외: AC5(종목 추가), 화면 간 이동, 기기 저장

## 2. 근거
- 이 작업을 먼저 하는 이유: 앱의 핵심 가치(직전 기록 확인)를 가장 작은 범위에서 확인할 수 있다.
- 필요한 구현 패턴은 3~5주차 실습에서 이미 써 봤다 (아래 자료표 2~7행).
  - 입력 검증·오류 문구 → 3주차, 빈 상태와 목록 → 4주차, 원본 상태와 계산값 구분 → 5주차

## 3. 제약
- 수정 대상 파일: 새 Flutter 프로젝트의 `lib/main.dart` 하나
- 상태 관리: `StatefulWidget` + `setState` (외부 상태관리 패키지 사용 안 함)
- 저장: 메모리 리스트만 사용 → 앱 종료 시 기록이 사라짐
- 로그인·서버·결제·유료 API·새 패키지를 추가하지 않음
- 계산할 수 있는 값(직전 기록)은 따로 저장하지 않고 getter로 계산

## 4. 현재 상태
- 앱 코드: **없음** (이번 주는 기획 문서만 작성)
- 확정: 앱 주제, MVP 범위, 화면 3개, 데이터 규칙, 앱 AC 5개
- 미정: 기기 저장 방법, 실행 대상 기기(아래 자료 충돌 참고)
- 상태 설계 초안:

| 구분 | 이름 | 내용 |
|---|---|---|
| 데이터 | `WorkoutSet` | `double weight`, `int reps`, `DateTime date` |
| 원본 상태 | `sets` | `List<WorkoutSet>`, 최신 기록이 맨 앞 |
| 계산값 | `latestSet` | `sets.isEmpty ? null : sets.first` (getter) |
| 이벤트 | `_addSet()` | 입력 검증 → 통과하면 `setState` 안에서 `sets.insert(0, ...)` |
| 오류 상태 | `_errorMessage` | 검증 실패 시 문구, 성공 시 `null` |

## 5. 확인 기준
- 앱 AC1~AC4를 실제 실행 화면으로 확인하고 캡처를 `evidence/`에 저장
- `dart format lib`, `flutter analyze` 결과 `No issues found`
- 실행하지 않은 항목은 `미실행`으로 기록

## 6. 자료표

| # | 자료 | 위치 | 역할 | 최신성 | 미정 항목 |
|---|---|---|---|---|---|
| 1 | 앱 제안서 | `week07/proposal.md` 8~9절 | 데이터 규칙(무게 0 초과 500 이하, 횟수 1~100), AC1~AC4 기준 | 이번 주 작성, 최신 | 저장 방법 |
| 2 | 3주차 목표 추가 코드 | [week03_goal_lab/lib/main.dart](https://github.com/chuns3hun/week03_goal_lab/blob/d318d04/lib/main.dart) 47~100행 | `TextEditingController`, `trim()` 후 빈 값 차단, `errorText` → AC2 | 커밋 `d318d04` 기준 | 숫자 범위 검증은 없음 |
| 3 | 3주차 기능 컨텍스트 | [week03_goal_lab/FEATURE_CONTEXT.md](https://github.com/chuns3hun/week03_goal_lab/blob/d318d04/FEATURE_CONTEXT.md) | Given-When-Then AC, 빈 입력 예외 처리 방식 | 커밋 `d318d04` 기준 | — |
| 4 | 4주차 위젯 화면 코드 | [week04_widget_lab/lib/main.dart](https://github.com/chuns3hun/week04_widget_lab/blob/138f2c6/lib/main.dart) 98~101행 | `Expanded` + `isEmpty`면 안내 문구, 아니면 `ListView.builder` → AC1, AC3 | 커밋 `138f2c6` 기준 | — |
| 5 | 4주차 위젯 선택표 | [week04_widget_lab/README.md](https://github.com/chuns3hun/week04_widget_lab/blob/138f2c6/README.md) | Card·ListTile로 기록 한 줄 표시, Flutter 공식 문서 링크 | 커밋 `138f2c6` 기준 | — |
| 6 | 5주차 상태 계약 | [week05_state_lab/STATE_CONTRACT.md](https://github.com/chuns3hun/week05_state_lab/blob/22ccef8/STATE_CONTRACT.md) | 원본 상태와 계산값 구분 → `latestSet` getter 설계 | 커밋 `22ccef8` 기준 | — |
| 7 | 5주차 상태 코드 | [week05_state_lab/lib/main.dart](https://github.com/chuns3hun/week05_state_lab/blob/22ccef8/lib/main.dart) 37~41행 | getter 계산값, `setState` 안에서 원본 상태 변경 → AC4 | 커밋 `22ccef8` 기준 | — |
| 8 | Dart 숫자 변환 문서 | [double.tryParse](https://api.dart.dev/stable/dart-core/double/tryParse.html), [int.tryParse](https://api.dart.dev/stable/dart-core/int/tryParse.html) | 문자 입력 시 `null` 반환 → AC2의 `abc` 차단 | Dart 공식 문서 최신판 | **링크 직접 열어 보지 않음**, 구현 전 확인 필요 |

## 7. 자료 충돌
- 실행 대상: proposal.md는 "Android 에뮬레이터 또는 실기기", 5주차 실습과 7주차 실습 안내는 `flutter run -d chrome` → 이번 작업은 chrome으로 먼저 확인하고, 실행 대상은 미정으로 둔다.

## 8. AI에게 이 작업을 요청할 때
- 제공할 자료: 이 파일과 proposal.md, 자료 2·4·7의 해당 행 범위만 (전체 파일·교재 본문·이전 대화 전체는 넣지 않음)
- 이유: 5주차 PROMPT_COMPARISON에서 완성 코드 전체를 주면 응답이 입력을 반복해 검토가 어려웠음
