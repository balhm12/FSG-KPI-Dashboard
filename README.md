# FSG KPI 대시보드 (개인·팀 KPI)

CRM 서비스 활동 Raw 엑셀을 암호화해 올리면 GitHub Actions가 집계·암호화해
GitHub Pages(비밀번호 잠금)로 배포합니다.

- 관리자: FSS Team Jun (hmson@sysmex.co.kr)
- 주소: https://balhm12.github.io/FSG-KPI-Dashboard/
- 비밀번호는 이 저장소 어디에도 적지 않습니다. 관리자에게 받으세요.

> 공개 저장소입니다. 평문 엑셀(`raw.xlsx`, `master.xlsx`), `dashboard_data*.json`,
> 비밀번호를 절대 올리지 마세요. `.gitignore`가 막아 주지만 커밋 전에 한 번 더 확인합니다.

## 구조

| 경로 | 역할 |
|---|---|
| `data/raw.xlsx.enc` | 서비스 활동 Raw (암호화). 매주 교체 |
| `reference/master.xlsx.enc` | 팀 명단·장비 그룹·거래처-팀 매핑 (암호화). 조직 개편 시 교체 |
| `scripts/extract.py` | 집계 규칙 본체 (가중치 `TECH_WEIGHT`·`ACAD_WEIGHT`, 제외 인원 `EXCLUDED_PEOPLE`, 추가 인원 `EXTRA_MEMBERS`) |
| `scripts/finalize.py` | 화면용 JSON 가공 (목표값 `benchmarks`) |
| `scripts/app.js`, `scripts/template.html` | 화면 |
| `scripts/build.py` | 복호화 → 집계 → 암호화 → `docs/index.html` |
| `tools/encrypt_source.html` | 엑셀을 암호화하는 오프라인 도구 |
| `.github/workflows/deploy.yml` | push·매주 월 07:00(KST)·수동 실행 시 자동 빌드·배포 |

## 처음 설정 (한 번만)

1. **Settings → Secrets and variables → Actions → New repository secret**
   - Name: `DASHBOARD_PASSWORD` / Secret: 대시보드 비밀번호
   - `data/raw.xlsx.enc`, `reference/master.xlsx.enc`를 암호화할 때 쓴 비밀번호와 같아야 합니다.
2. **Settings → Pages → Build and deployment → Source: GitHub Actions**
3. **Actions** 탭 → "Build and deploy KPI dashboard" → **Run workflow**. 녹색 체크가 뜨면 위 주소로 접속합니다.

## 매주 데이터 갱신

1. CRM에서 **누적 전체 기간**(약 40,000행) 엑셀을 내려받습니다. 부분 월 파일을 올리면 이전 데이터가 사라집니다.
2. `tools/encrypt_source.html`을 브라우저로 열어 엑셀 선택 → 비밀번호 입력 → `raw.xlsx.enc` 다운로드.
3. 저장소 `data/` 폴더 → **Add file → Upload files**로 같은 이름으로 덮어쓰고 Commit.
4. Actions가 녹색이 되면 대시보드를 새로고침해 데이터 기준일을 확인합니다.

## 비밀번호 변경

`raw.xlsx.enc`, `master.xlsx.enc`를 새 비밀번호로 다시 암호화해 올리고, Secret `DASHBOARD_PASSWORD`도 같은 값으로 바꿉니다.

## 변경 이력

- 2026-10-06: PM 가중치 0.4 → 0.2, 온라인 정기점검 0.2 → 0(KPI 점수 제외),
  인원 FSG 로스터 44명 기준(백원욱·심구 FS WEST 추가), 빌드 로그의 비밀번호 출력 제거,
  README의 평문 비밀번호 삭제. 원작자: 김성민(ksm1339/FSG-KPI-Dashboard)
