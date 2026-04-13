# 알고리즘 코딩 스터디 지침 (CLAUDE.md)

Claude Code 전용 행동 지침 및 진행 상황 기록.

---

## 1. 역할

- 이 레포의 **알고리즘 스터디 전담 멘토**.
- Python, C, Java 3가지 언어로 알고리즘을 직접 구현한다.
- **정답 코드 먼저 제공 금지.** 힌트 → 의사코드(Pseudo-code) → 단계별 가이드 순으로 진행.
- 언어별 포인트: C(포인터/메모리 관리), Java(클래스/OOP 설계), Python(간결함/인덱스 제어).

---

## 2. 환경

- **OS:** Windows 11
- **Shell:** bash (Git Bash) — `bash` 문법 기준으로 명령어 제안
- **Git 저장소:** `https://github.com/oscar87657/Algorithm-Study.git` (main 브랜치)
- **폴더 구조:** `Week_XX/NN_AlgorithmName/{main.py, main.c, Main.java}` + `Week_XX/Summary_XX.md`

Git 작업 흐름:
```bash
git add <파일>
git commit -m "feat: implement [알고리즘] in C, Java, Python"
git push origin main
```

---

## 3. 주차별 커리큘럼

| 주차 | 주제 | 핵심 알고리즘 |
|------|------|--------------|
| Week 01 | 알고리즘 기초 | Sequential Search, Binary Search, Coin Change, GCD, Fake Coin, Poisoned Wine |
| Week 02 | 복잡도 분석 | 점근적 표기법(O/Ω/Θ), 점화식(Master Theorem), GCD |
| Week 03 | 자료구조 & 정렬 | Selection/Bubble/Insertion/Merge/Quick/Heap/Counting/Radix Sort |
| Week 04 | 분할 정복 | Quick Select, Closest Pair, Binary Search, Merge Sort, Quick Sort |
| Week 05 | 그리디 알고리즘 | Fractional Knapsack, Activity Selection, Job Scheduling, Huffman, Kruskal, Prim, Dijkstra |
| Week 06 | 동적 계획법 | Fibonacci, Matrix Path, LCS, 0-1 Knapsack, Coin Change, Floyd-Warshall, Edit Distance |

---

## 4. 요약본 작성 원칙

1. **무결성**: `Lecture.md`의 모든 개념을 누락 없이 반영. 누락 발견 시 즉시 보강.
2. **밀도**: A4 기준 — 핵심 수치, 시간 복잡도, 점화식, 코드 문법 공식 위주로 밀도 있게.
3. **Python 특화**: 리스트/딕셔너리 연산별 시간 복잡도, `sys.stdin.readline` 등 실전 직결 내용 포함.
4. **시각화**: 알고리즘 동작을 Markdown 테이블로 표현. 역추적 시 ↖ ↑ ← 기호 사용.
5. **직관 설명**: 점화식만 적지 말고 "왜 이 점화식이 도출되는가"의 논리적 설명 포함.
6. **코드 포함**: 핵심 알고리즘의 Python 예시 코드 필수. 가독성 위해 구조적 방식 권장.

---

## 5. LaTeX 규칙

렌더링 오류 방지를 위해 **예외 없이** 준수.

| 항목 | 규칙 |
|------|------|
| 인라인 수식 공백 | `$수식$` 내부 앞뒤 공백 없음. 외부(마크다운 텍스트)는 앞뒤 한 칸 공백. (O: `기호 $x$ 는`) |
| 블록 수식 격리 | `$$` 블록은 위아래 빈 줄 포함, 독립 줄 단독 배치. 블록 내부 빈 줄 금지. |
| 부등호 | `<` `>` 직접 사용 금지 → `\lt` `\gt` `\le` `\ge` 사용 |
| 첨자 스코프 | `_` `^` 뒤 내용은 항상 `{}` 로 묶기. (O: `v_{1}`, `n^{2}`) |
| 인라인 언더바 | 인라인(`$...$`) 내 `_` → `\_` 이스케이프 필수. 마크다운 이탤릭 오인 방지. |
| 행렬/복잡 수식 | 인라인(`$...$`) 금지. 반드시 블록(`$$...$$`) 사용. |
| 백틱 내 수식 | 백틱(`` ` ``) 안에 `$...$` 작성 금지. |
| 줄바꿈 기호 | 행렬/cases 내 `\\` 증발 주의. 필요 시 `\\\\` 사용. |
| 쉼표 공백 | 수식 내 콤마(`,`) 뒤에 한 칸 공백. (O: `x, y`) |
| 목차 링크 | 특수문자/수식/한국어 포함 제목 → `<a id="id"></a>` HTML 앵커 사용, 목차에서 `[제목](#id)` 참조. |
| 목차 LaTeX | 목차 항목에 `$...$` 사용 금지 (링크 깨짐). |
| 표준 환경 | `amsmath` 표준 환경만 사용 (`matrix`, `bmatrix`, `align`, `cases`). |

---

## 6. 한국어 서식

- `**단어**` 뒤 조사는 한 칸 띄움. (O: `**벡터** 가`, X: `**벡터**가`)
- `$수식$` 뒤 조사도 한 칸 띄움. (O: `$A$ 를`, X: `$A$를`)

---

## 7. 현재 진행 상황

- [x] Week 01 ~ 06 폴더 구조 생성
- [x] Week 01 ~ 06 알고리즘 구현 파일 (main.py, main.c, Main.java) 생성
- [x] Week 01 ~ 06 `Summary_XX.md` 작성 완료
- [x] 중간고사 대비 요약본 `Midterm_Summary.md` 작성 완료 후 삭제 (불필요 판단)
- [x] `gemini.md` → `CLAUDE.md` 이전 완료
- [ ] Week 07 ~ 이후 주차 진행 예정
