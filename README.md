# Nurse Scheduler Prototype

병동 간호사 근무표를 GitHub Pages에서 바로 열어볼 수 있게 만든 정적 HTML 프로토타입입니다.

## 목표
- 별도 서버 없이 GitHub Pages 배포
- 엑셀처럼 한 달 근무표 확인
- 사람별 월 목표 근무수(D/S/E/N/OFF) 직접 수정
- 사람별 리퀘스트(쉬고 싶은 날, 선호/회피 쉬프트) 입력
- 날짜별 부족/초과 배정 시 연한 경고색으로 즉시 표시

## 현재 기능
- 3교대 + S 포지션
- 요일별 필요 인원 자동 반영
  - 월/금: D3 E3 N2
  - 화/수/목: D2 + S1 / E2 / N2
  - 토: D3 E2 N2
  - 일/공휴일: D2 E2 N2
- N 2연속 후 OFF 2연속 구조
- E 다음날 D/S 금지
- S는 연차 하위 5명만 가능
- 주간 3인조에는 상위 4명 중 1명 포함
- 수간호사(1번) D만, 목/일 OFF
- 표에서 셀 직접 수정 가능
- 표에서 목표치 직접 수정 가능
- 리퀘스트 직접 수정 가능
- 커버리지 불일치 시 시각 경고
- 현재 상태 JSON 저장 가능

## 입력 형식
왼쪽의 인원 기본값 textarea는 아래 형식입니다.

```text
번호,이름,role,maxN,preferred,blocked,request
```

예시:

```text
1,1,HN,0,D,Thu|Sun,prefer:D:1|2|4
7,7,RN,6,N,,off:10|11;avoid:E:12|13
```

### 필드 설명
- `번호`: 연차순 번호
- `이름`: 화면 표시 이름
- `role`: `HN` 또는 `RN`
- `maxN`: 월 N 목표/상한 기본값
- `preferred`: 기본 선호 쉬프트 (`D`,`S`,`E`,`N`)
- `blocked`: 요일 고정 OFF (`Thu|Sun` 같은 형식)
- `request`: 개별 요청

## request 문법
세미콜론(`;`)으로 여러 요청을 나눕니다.

### 1) 특정 날짜 OFF 요청
```text
off:5|12|26
```

### 2) 특정 쉬프트 선호
```text
prefer:D:1|2|3
prefer:N:14|15
```

### 3) 특정 쉬프트 회피
```text
avoid:E:20|21
avoid:N:28|29
```

조합 예시:

```text
off:5|12;prefer:D:1|2|3;avoid:N:20|21
```

## GitHub Pages 배포 방법

### 방법 1: 새 저장소에 바로 올리기
1. GitHub에서 새 저장소 생성
2. 이 폴더의 `index.html`과 `README.md` 업로드
3. 저장소 설정(Settings) → Pages
4. Source를 `Deploy from a branch`로 선택
5. Branch를 `main` / root 로 선택
6. 저장하면 Pages URL 생성

### 방법 2: 로컬에서 git으로 올리기
```bash
git init
git add .
git commit -m "init nurse scheduler prototype"
git branch -M main
git remote add origin <YOUR_REPO_URL>
git push -u origin main
```
그 뒤 GitHub Pages를 켭니다.

## 사용 흐름
1. 연도/월/공휴일 입력
2. 인원 기본값 입력 또는 샘플 불러오기
3. 표 생성 / 다시 계산 클릭
4. 생성된 표에서
   - 셀 직접 수정
   - 월 목표치 수정
   - 리퀘스트 수정
5. 부족/초과가 생기면 하이라이트 확인
6. JSON 저장으로 현재 설정 백업

## 한계
- 아직 완전한 최적화 엔진은 아님
- 목표치 수정 후 자동 재최적화는 부분적
- 법정 근로시간/주당 시간/연속근무 상한 등은 추가 구현 필요
- 공휴일에 따른 기본 OFF 수량 자동 계산은 다음 단계 권장

## 다음 추천 단계
- 사람별 전담 쉬프트 비율 반영
- 공휴일 포함 월 기본 OFF 자동 계산
- 자동 재배치 엔진 강화
- CSV/엑셀 내보내기
- 로컬 저장(localStorage)
