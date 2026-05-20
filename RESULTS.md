# Results

## 1. 실험 요약
- 저장소: bench-diffusion-browser-shootout
- 커밋 해시: 78ad779
- 실험 일시: 2026-05-20T15:37:00.012Z -> 2026-05-20T15:37:05.549Z
- 담당자: ai-webgpu-lab
- 실험 유형: `benchmark`
- 상태: `success`

## 2. 질문
- 고정 prompt-to-image fixture에서 diffusion profile별 sec per image와 steps per sec 차이를 같이 비교할 수 있는가
- WebGPU와 fallback 실행 모드가 같은 diffusion benchmark protocol과 결과 스키마로 기록되는가
- 실제 browser diffusion runtime 교체 전 resolution_success_rate와 oom_or_fail_rate 기준을 고정할 수 있는가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 32 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: synthetic-webgpu-diffusion-profile, cpu-diffusion-fallback
- backend: `webgpu, cpu`
- fallback triggered: `false, true`
- worker mode: `worker, main`
- cache state: `warm`
- required features: ["shader-f16"], []
- limits snapshot: {"maxStorageBuffersPerShaderStage":8,"maxTextureDimension2D":8192}, {}

## 4. 워크로드 정의
- 시나리오 이름: Diffusion Shootout / WebGPU, Diffusion Shootout / Fallback
- 입력 프로필: 768x512-24-steps-3-profiles
- 데이터 크기: winner=balanced-karras; promptTag=observatory-aurora-bench-v1; scheduler=dpmpp-2m-karras; resolution=768x512; seed=60; steps=21; previews=4; backend=webgpu; fallback=false; automation=playwright-chromium, winner=balanced-karras; promptTag=observatory-aurora-bench-v1; scheduler=dpmpp-2m-karras; resolution=768x512; seed=60; steps=21; previews=4; backend=cpu; fallback=true; automation=playwright-chromium, winner=balanced-karras; promptTag=observatory-aurora-bench-v1; scheduler=dpmpp-2m-karras; resolution=768x512; seed=60; steps=21; previews=4; backend=webgpu; fallback=false; realAdapter=fallback(Benchmark.Suite not available); automation=playwright-chromium
- dataset: diffusion-benchmark-fixture-v1
- model_id 또는 renderer: balanced-karras
- 양자화/정밀도: -
- resolution: -
- context_tokens: -
- output_tokens: -

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 1117.5 ~ 2099.4 ms
- init_ms: 306.56 ~ 655.74 ms
- success_rate: 1
- peak_memory_note: 32 GB reported by browser
- error_type: -

### Diffusion
- sec_per_image: 0.31 ~ 0.66 s
- steps_per_sec: 54.51 ~ 106.84
- resolution_success_rate: 0.82 ~ 1
- oom_or_fail_rate: 0.02 ~ 0.18
- worker modes: worker, main
- backends: webgpu, cpu
- fallback states: false, true

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | Diffusion Shootout / WebGPU | webgpu | warm | 106.84 | 0.31 | resolution_success=1, oom_or_fail=0.02 |
| 2 | Diffusion Shootout / Fallback | cpu | warm | 54.51 | 0.66 | resolution_success=0.82, oom_or_fail=0.18 |
| 3 | Diffusion Shootout / WebGPU | webgpu | warm | 106.84 | 0.31 | resolution_success=1, oom_or_fail=0.02 |

## 7. 관찰
- diffusion browser shootout는 backend=webgpu, fallback_triggered=false로 기록됐다.
- 가장 낮은 sec_per_image는 Diffusion Shootout / WebGPU의 0.31 s였고 resolution_success_rate=1였다.
- diffusion benchmark metadata는 winner=balanced-karras; promptTag=observatory-aurora-bench-v1; scheduler=dpmpp-2m-karras; resolution=768x512; seed=60; steps=21; previews=4; backend=webgpu; fallback=false; automation=playwright-chromium로 남았다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. WebGPU vs Fallback
- Diffusion shootout: webgpu winner=balanced-karras, fallback winner=balanced-karras
- sec_per_image: webgpu=0.31 s, fallback=0.66 s, delta=-0.349 s
- steps_per_sec: webgpu=106.84, fallback=54.51, delta=+52.33
- resolution_success_rate: webgpu=1, fallback=0.82, delta=+0.18
- oom_or_fail_rate: webgpu=0.02, fallback=0.18, delta=-0.16

## 9. Real Adapter vs Deterministic
- adapter: real=bench-diffusion-diffusion-browser-shootout-214-300, deterministic=deterministic-renderer-shootout
- adapter_run: real=connected, deterministic=deterministic
- success_rate: real=1, deterministic=1

## 10. 결론
- diffusion browser shootout가 WebGPU와 fallback pair를 raw JSON과 RESULTS.md 양쪽에 남기게 됐다.
- 다음 단계는 deterministic diffusion profiles를 실제 browser diffusion runtime 후보로 교체하되 sec_per_image/steps_per_sec/resolution_success_rate/oom_or_fail_rate result fields를 유지하는 것이다.
- 이후 `exp-diffusion-webgpu-browser`와 `app-browser-image-lab`의 diffusion regression 기준으로 재사용할 수 있다.

## 11. 첨부
- 스크린샷: ./reports/screenshots/01-diffusion-browser-shootout-webgpu.png, ./reports/screenshots/02-diffusion-browser-shootout-fallback.png, ./reports/screenshots/10-diffusion-browser-shootout-real-diffusion-bench.png
- 로그 파일: ./reports/logs/01-diffusion-browser-shootout-webgpu.log, ./reports/logs/02-diffusion-browser-shootout-fallback.log, ./reports/logs/10-diffusion-browser-shootout-real-diffusion-bench.log
- raw json: ./reports/raw/01-diffusion-browser-shootout-webgpu.json, ./reports/raw/02-diffusion-browser-shootout-fallback.json, ./reports/raw/10-diffusion-browser-shootout-real-diffusion-bench.json
- 배포 URL: https://ai-webgpu-lab.github.io/bench-diffusion-browser-shootout/
- 관련 이슈/PR: -
