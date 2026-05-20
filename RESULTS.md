# Results

## 1. 실험 요약
- 저장소: exp-webllm-browser-chat
- 커밋 해시: c332e2e
- 실험 일시: 2026-05-20T15:46:43.530Z -> 2026-05-20T15:46:57.850Z
- 담당자: ai-webgpu-lab
- 실험 유형: `llm`
- 상태: `success`

## 2. 질문
- WebLLM-style 브라우저 채팅 baseline이 단일 결과 문서로 고정되는가
- 같은 prompt budget에서 fallback 메타데이터와 worker mode가 함께 기록되는가
- 후속 local chat demo로 승격하기 전에 readiness harness로 재사용 가능한가

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
- adapter: synthetic-webgpu-profile
- backend: `webgpu`
- fallback triggered: `false`
- worker mode: `worker`
- cache state: `warm`
- required features: ["shader-f16"]
- limits snapshot: {}

## 4. 워크로드 정의
- 시나리오 이름: WebLLM Browser Chat
- 입력 프로필: prompt-20-output-56
- 데이터 크기: promptTokens=20; outputTokens=56; executionMode=webgpu; backend=webgpu; automation=playwright-chromium, promptTokens=20; outputTokens=56; executionMode=webgpu; backend=webgpu; realAdapter=fallback(adapter.loadModel is not a function); automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: webllm-browser-chat-baseline
- 양자화/정밀도: -
- resolution: -
- context_tokens: 20
- output_tokens: 56

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 564.2 ~ 1839.4 ms
- init_ms: 84.1 ~ 84.2 ms
- success_rate: 1
- peak_memory_note: 32 GB reported by browser
- error_type: -

### LLM / Benchmark
- ttft_ms: 19.1 ~ 19.2 ms
- prefill_tok_per_sec: 819.67 ~ 826.45 tok/s
- decode_tok_per_sec: 208.1 ~ 208.18 tok/s
- turn_latency_ms: 377.4 ~ 377.6 ms
- backends: webgpu
- fallback states: false

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | WebLLM Browser Chat | webgpu | warm | 208.1 | 19.1 | prefill=819.67 tok/s, metric=decode tok/s / TTFT ms |
| 2 | WebLLM Browser Chat | webgpu | warm | 208.18 | 19.2 | prefill=826.45 tok/s, metric=decode tok/s / TTFT ms |

## 7. 관찰
- WebLLM browser chat baseline은 backend=webgpu, worker_mode=worker로 기록됐다.
- readiness summary는 TTFT=19.1 ms, decode=208.1 tok/s였다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. Real Adapter vs Deterministic
- adapter: real=webllm-llama-3-2-1b-instruct-q4f16-1-mlc-0278, deterministic=deterministic-mock
- adapter_run: real=connected, deterministic=deterministic
- success_rate: real=1, deterministic=1

## 9. 결론
- WebLLM browser chat readiness harness가 첫 raw result와 summary 문서를 갖게 됐다.
- 다음 단계는 synthetic single-runtime path를 실제 WebLLM integration으로 교체하는 것이다.
- 이후 `app-local-chat-arena`와 shared prompt budget을 유지하면서 app 승격 기준으로 재사용할 수 있다.

## 10. 첨부
- 스크린샷: ./reports/screenshots/01-webllm-browser-chat.png, ./reports/screenshots/10-webllm-browser-chat-real-webllm.png
- 로그 파일: ./reports/logs/01-webllm-browser-chat.log, ./reports/logs/10-webllm-browser-chat-real-webllm.log
- raw json: ./reports/raw/01-webllm-browser-chat.json, ./reports/raw/10-webllm-browser-chat-real-webllm.json
- 배포 URL: https://ai-webgpu-lab.github.io/exp-webllm-browser-chat/
- 관련 이슈/PR: -
