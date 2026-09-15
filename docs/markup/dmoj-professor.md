# DMOJ 교수 사용 가이드 (C Programming 수업용)

서버: `http://61.81.98.89` | SSH 접속 후 작업 디렉토리: `/opt/dmoj-docker/dmoj`

---

## 목차

1. 문제(Problem) 등록 방법
2. 테스트케이스 등록 방법 — 웹 화면 / 체커 / YAML
3. ⚠️ 필수: 새 문제는 반드시 `fix_problem.sh` 실행
4. 대회(Contest) 등록 방법
5. 대회 종료 후 문제 공개 (Problems 탭에 나오게 하기)
6. 학생 계정 관리
7. 조직(Organization) 관리
8. 채점 서버(judge) 문제 진단
9. 자주 겪는 에러 체크리스트

---

## 1. 문제(Problem) 등록 방법

1. `http://61.81.98.89/admin/judge/problem/add/` 접속
2. 주요 입력 항목:
   - **Code**: 문제 고유 코드 (예: `p3fixbugs`). 한 번 정하면 바꾸기 번거로우니 신중하게
   - **Name**: 문제 제목
   - **Description**: 문제 지문 (마크다운 지원 — `problem.md` 파일 내용을 그대로 붙여넣기)
   - **Points**: 배점 (partial credit 쓰려면 **Partial** 체크박스도 켜기)
   - **Allowed languages**: **C**만 체크 (수업 특성상)
   - **Authors**: 본인 계정 지정
3. 저장 후, 바로 이어서 **테스트케이스 등록**(2번 항목)으로 진행

---

## 2. 테스트케이스 등록 방법 — 웹 화면 / 체커 / YAML

### 2-1. 웹 화면 (표 기반 에디터) — 기본 방법

1. 문제 편집 화면 → **"Edit Testcases"** 진입 (URL: `/problem/<코드>/data`)
2. **"Data zip file"**에 `.in`/`.out` 파일 + `init.yml`을 담은 zip 업로드
   - zip 안 구조는 평평하게(폴더 없이) `01.in`, `01.out`, `init.yml` 등이 바로 있어야 함
3. 표 아래쪽 **각 케이스 행의 "Points" 칸에 배점을 직접 입력** — zip에 `init.yml`로 배점을 넣어놔도 **자동으로 안 채워짐**, 반드시 수동 입력 필요
4. 다 채운 후 **Submit!**

### 2-2. `[View YAML]` 링크는 이 서버에서 작동 안 함

화면 우측 상단 `[View YAML]`을 눌러도 `404 error`가 뜹니다 (이 DMOJ 버전의 알려진 제약). YAML을 직접 붙여넣는 방식은 포기하고, 항상 **2-1의 표 방식**을 쓰세요.

### 2-3. 체커(Checker) — 정답 판정 규칙 바꾸기

기본은 "출력이 100% 완전 일치"해야 정답입니다. 특정 케이스만 다른 기준으로 채점하고 싶으면 표의 **"Checker" 열**(안 보이면 상단 `Show columns: Checker` 체크)에서 드롭다운으로 선택:

| 체커 | 용도 |
|---|---|
| Standard | 기본값, 완전 일치 |
| Floats / Floats (absolute) / Floats (relative) | 실수 오차 허용 |
| Non-trailing spaces | 줄 끝 공백 무시 |
| Sorted | 순서 무관, 정렬 비교 |
| Byte identical | 완전 바이트 일치 (공백도 안 봐줌) |
| Line-by-line | 줄 단위 비교 |

이 목록에 없는 규칙(예: 문장부호 무시, 컴파일만 되면 정답 처리)이 필요하면, 커스텀 체커 Python 코드를 만들어서 서버에 등록해야 합니다 — 필요하면 요청해주세요, 코드부터 새로 짜드립니다.

---

## 3. ⚠️ 필수: 새 문제는 반드시 `fix_problem.sh` 실행

### 왜 필요한가

이 DMOJ 채점 서버는 `/problems/<문제코드>/<문제코드>/init.yml`처럼 **폴더 이름을 두 번 중첩**해야 문제를 인식하는 구조적 결함이 있습니다. 웹에서 zip을 업로드하면 `/problems/<문제코드>/`에만 풀리고, 중첩 폴더가 자동으로 안 만들어집니다.

**이걸 빠뜨리면 학생이 제출할 때 "No judge is available for this problem" 에러가 뜹니다.**

### 실행 방법

```bash
cd /home/adminai
sudo ./fix_problem.sh <문제코드>
```

예시:
```bash
sudo ./fix_problem.sh p3fixbugs
```

이 스크립트가 자동으로:
1. 중첩 폴더 생성 및 파일 복사
2. `judge` 컨테이너 재시작
3. 채점 서버가 인식했는지 확인 (`✅ 성공!` 메시지)

**순서 요약: 문제 등록 → zip 업로드 (2번) → `fix_problem.sh` 실행 (3번) → 학생 테스트 제출로 확인**

(참고: `Edit Testcases` 화면 상단에도 이 체크리스트가 노란 배너로 항상 표시됩니다.)

---

## 4. 대회(Contest) 등록 방법

1. `http://61.81.98.89/admin/judge/contest/add/` 접속
2. 주요 항목:
   - **Key**: 대회 코드 (예: `week2c1`)
   - **Name**: 대회 이름
   - **Organizations**: **해당 분반만** 지정 (예: C1만 지정하면 C1 학생만 이 대회를 볼 수 있음) — `is_organization_private` 자동 적용됨
   - **Start time / End time**: 전체 대회 접속 가능 창 (모두에게 동일)
   - **Time limit**: **개인별 타이머**. 비워두면 전체 창 끝까지 무제한, 채우면 `H:MM:SS` 형식(예: `0:30:00` = 30분)으로 입력 — 학생이 Join한 시점부터 개인 카운트다운 시작
   - **Scoreboard visibility**: 보통 `Visible`
3. 저장 후 **"Problems"** 탭에서 문제 추가 (순서/배점 지정 가능)

### Join / Spectate / Virtual join 차이

| 상태 | 의미 |
|---|---|
| **Join contest** | 정식 참가 가능, 점수 반영됨 |
| **Spectate** | 대회가 끝났거나 개인 타이머를 이미 소진 — 구경/연습 제출만 가능, 점수 미반영 |
| **Virtual join** | 대회 종료 후에도 원래 시간 제한 그대로 재도전 가능 (연습용, 실제 순위표엔 영향 없음) |

---

## 5. 대회 종료 후 문제 공개 (Problems 탭에 나오게 하기)

대회가 끝나도 문제는 자동으로 `Problems` 탭에 안 나타납니다(보안/공정성을 위한 기본 설정). 수동으로 공개 전환해야 합니다.

```bash
docker exec -it dmoj-site-1 python3 manage.py publish_ended_contests
```

- **종료된 모든 대회**를 한 번에 스캔해서, 그 대회를 봤던 분반 학생만 볼 수 있게 자동으로 공개 전환합니다
- 진행 중이거나 시작 전인 대회는 **자동으로 건드리지 않음** (실수로 대회 도중 돌려도 안전)
- 이미 처리된 문제는 건너뜀 (여러 번 실행해도 안전)
- **매 대회 종료 후 직접 실행해야 합니다** (자동화 안 해두기로 결정한 상태)

---

## 6. 학생 계정 관리

모두 아래 명령으로 시작합니다:
```bash
docker exec -it dmoj-site-1 python3 manage.py shell
```

### 6-1. 비밀번호 강제 재설정

```python
from django.contrib.auth.models import User

username = '학번'
new_password = '새비밀번호'

u = User.objects.get(username=username)
u.set_password(new_password)
u.is_active = True
u.save()
print(f'{username}: 초기화 완료 -> {new_password}')
```

(참고: "Forgot your password?" 기능은 이메일 서버가 없어서 작동하지 않으므로, 학생 비밀번호 분실 시 이 방법이 유일한 해결책입니다.)

### 6-2. 계정 활성화만 (비밀번호는 그대로 유지)

```python
u = User.objects.get(username='학번')
u.is_active = True
u.save()
```

### 6-3. username / email 변경

```python
u = User.objects.get(username='기존학번')
u.username = '새학번'
u.email = 'new_email@example.com'
u.save()
```

### 6-4. 계정 삭제

```python
u = User.objects.get(username='학번')
u.delete()
```
⚠️ 되돌릴 수 없고, 그 학생의 제출 기록도 같이 삭제됩니다. 이미 성적이 매겨진 학생이면 삭제 대신 `is_active = False`로 비활성화만 추천합니다.

### 6-5. 조직(분반)별 학생 목록 조회

**웹 화면**: `/admin/judge/profile/` → 우측 필터 패널의 **"By organizations"**에서 분반 선택

**셸에서**:
```python
from judge.models import Organization
org = Organization.objects.get(name='C 2026 Fall - C1')
for profile in org.member.all().order_by('user__username'):
    print(profile.user.username, '|', profile.user.email)
```

---

## 7. 조직(Organization) 관리

- 학생은 **자기 조직(분반)을 스스로 변경할 수 없습니다** (프로필 화면에서 비활성화되어 있음, 실수로 다른 분반 가입 방지)
- 조직 변경이 필요하면 교수가 직접 처리:

```python
from judge.models import Organization

u = User.objects.get(username='학번')
new_org = Organization.objects.get(name='C 2026 Fall - C2')
u.profile.organizations.set([new_org])
```

---

## 8. 채점 서버(judge) 문제 진단

### "No judge is available for this problem" 뜰 때

**99% `fix_problem.sh`를 안 돌려서 생기는 문제입니다.** 3번 항목대로 실행하세요.

### 그래도 안 되면 — 서버 상태 점검

```bash
docker compose ps                        # 모든 컨테이너가 Up 상태인지 확인
docker compose logs judge --tail 20      # judge가 정상 연결됐는지
docker compose logs bridged --tail 20    # 중계 서버 상태
```

`judge` 또는 `bridged`가 응답이 없어 보이면:
```bash
docker compose restart judge
# 그래도 안 되면
docker compose restart bridged
```

### "We are waiting for a suitable judge..."에서 멈춰있을 때

채점 서버가 연결은 됐는데 대기열만 쌓이는 상태입니다. 위와 같이 `judge`/`bridged` 재시작으로 대부분 해결됩니다.

---

## 9. 자주 겪는 에러 체크리스트

| 증상 | 원인 | 조치 |
|---|---|---|
| 학생 로그인 안 됨, "Invalid username or password" | 이메일 인증 미완료로 계정 비활성 상태 | `is_active=True`로 활성화 (6-2번) |
| 비밀번호 분실 | 이메일 인증 기능 없음, 셀프 재설정 불가 | 6-1번 강제 재설정 |
| "No judge is available" | 새 문제에 `fix_problem.sh` 안 돌림 | 3번 항목 |
| 대회 끝났는데 Problems 탭에 문제 안 보임 | 자동 공개 전환 안 됨 | 5번 항목 |
| 학생이 대회 도중 "Spectate"만 뜸 | 개인 Time limit 이미 소진 | 정상 동작, Virtual join으로 재도전 안내 |
| 조직 변경 요청 | 학생 셀프 변경 불가 (의도된 제한) | 7번 항목으로 교수가 직접 변경 |
| 테스트케이스 저장 시 "Points must be defined" 에러 | zip의 배점이 표에 자동 반영 안 됨 | 표에 Points 직접 입력 (2-1번) |

---

## 참고 — 서버 관리 시 안전 수칙

- **템플릿/설정 파일을 수정하기 전엔 항상 백업**: `sudo cp 원본파일 원본파일.bak.$(date +%Y%m%d_%H%M%S)`
- **CSS/JS를 건드렸다면** `make_style.sh` + `collectstatic` + `compilejsi18n`을 순서대로 실행 후 재시작 (안 그러면 사이트 전체가 다운될 수 있음 — 실제로 겪었던 사고입니다)
- **`docker compose` 명령은 반드시 `/opt/dmoj-docker/dmoj` 디렉토리에서 실행** (다른 위치에서 실행하면 "no configuration file provided" 에러)
- 확실하지 않은 서버 작업은 먼저 여기 있는 절차를 따르고, 애매하면 진행 전에 꼭 재확인하는 습관 권장
