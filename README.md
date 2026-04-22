# bench-diffusion-browser-shootout

`브라우저 diffusion 비교`를 비교 가능한 형태로 측정하는 benchmark 저장소입니다. 동일한 입력과 보고 형식을 유지하면서 구현체 간 차이를 수치와 산출물로 남기는 것이 목적입니다.

## 저장소 역할
- 둘 이상의 런타임, 모델, 렌더러, 구현 방식을 같은 조건으로 비교합니다.
- 결과 수집과 표준화된 보고 형식을 우선하며, 단일 기능 구현 자체보다 비교 가능성이 더 중요합니다.
- 개별 실험 저장소의 가설을 수치로 검증하는 공통 벤치마크 기준점을 제공합니다.

## 핵심 질문
- 같은 시나리오에서 어떤 구현이 더 빠르고 안정적인가
- 성능 차이가 초기 로드, warm run, cache 상태, worker 분리에 따라 어떻게 달라지는가
- 이후 제품/앱 선택에 필요한 최소 비교 지표는 무엇인가

## 포함 범위
- 비교 대상과 공통 입력 시나리오 정의
- raw JSON, 로그, 스크린샷 등 재현 가능한 결과 저장
- `RESULTS.md`에 핵심 비교표와 해석 기록

## 비범위
- 단일 구현의 세부 기능 확장
- 프로덕션 배포용 UI 완성도 추구
- 비교 조건이 제각각인 임의 측정

## 기본 구조
- `src/` - 구현 코드 또는 baseline 프로토타입
- `public/` - GitHub Pages baseline probe 또는 실제 정적 데모 산출물
- `reports/raw/` - 원시 측정 결과 JSON/CSV/로그
- `reports/screenshots/` - 시각 결과 스크린샷
- `reports/logs/` - 실행 로그와 디버깅 산출물
- `schemas/ai-webgpu-lab-result.schema.json` - 공통 결과 스키마
- `RESULTS.md` - 핵심 결과 요약과 해석

## 메타데이터
- Track: Benchmark
- Kind: benchmark
- Priority: P2

## 현재 상태
- Repository scaffold initialized
- Shared result schema copied to `schemas/ai-webgpu-lab-result.schema.json`
- Shared reporting template copied to `RESULTS.md`
- GitHub Pages baseline probe copied to `public/index.html`
- Browser probe logic copied to `public/app.js`
- GitHub Pages workflow copied to `.github/workflows/deploy-pages.yml`

## GitHub Pages 운영 메모
- Pages URL: https://ai-webgpu-lab.github.io/bench-diffusion-browser-shootout/
- 기본 bootstrap workflow는 `public/` baseline probe 정적 artifact를 배포합니다.
- 실제 빌드가 필요한 저장소는 install/build 단계와 artifact 경로를 저장소 사양에 맞게 교체해야 합니다.

## 측정 및 검증 포인트
- latency, throughput, load time, cache hit 여부
- 브라우저/OS/디바이스별 편차
- 오류율, fallback 발생, UI jank 또는 frame budget 영향

## 산출물
- 공통 입력 기준이 명시된 벤치마크 harness
- raw 결과 파일과 요약 보고서
- GitHub Pages 또는 README에서 확인 가능한 결과 surface

## 작업 및 갱신 절차
- `src/` 아래에 첫 runnable baseline 또는 비교 harness를 구현합니다.
- 실제 사용 스택이 정해지면 이 README에 install/dev/build/test 명령을 추가합니다.
- 측정 결과는 `reports/raw/`와 `RESULTS.md`에 함께 반영합니다.
- 브라우저, OS, 디바이스, cache, worker 여부 등 재현 조건을 결과와 같이 기록합니다.
- Pages를 유지하는 경우 baseline probe 또는 workflow를 실제 저장소 동작에 맞게 교체합니다.

## 완료 기준
- 비교 대상별 결과가 같은 형식으로 수집됩니다.
- `RESULTS.md`가 raw JSON과 일치합니다.
- 의사결정에 필요한 최소 비교표와 해석이 문서화되어 있습니다.

## 관련 저장소
- `shared-bench-schema` - 벤치 결과 공통 스키마
- `docs-lab-roadmap` - 벤치 우선순위와 실행 계획
- 관련 `exp-*` 저장소 - 벤치 입력과 baseline 출처

## 라이선스
MIT
