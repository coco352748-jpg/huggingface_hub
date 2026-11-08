# huggingface_hub 최상위 아키텍처 정리

> 기준: 저장소에 커밋된 문서 (`AGENTS.md`, `src/huggingface_hub/README.md`, `docs/source/en/concepts/git_vs_http.md`, `docs/source/en/guides/*.md`).
> 정렬 기준: 핵심도(중요도). 문서에는 👍 같은 추천수가 없습니다.

## 1. HTTP 클라이언트 중심 설계 (`HfApi`)

- Hub의 모든 저장소(모델, 데이터셋, Space)는 git 기반이지만, 라이브러리는 로컬 git 클론 대신 HTTP API로 접근합니다.
- `HfApi` 클래스가 저장소 생성/삭제, 파일 업로드/삭제, 커밋, 브랜치/태그, 디스커션/PR, 검색을 제공합니다.
- 주요 파일: `src/huggingface_hub/hf_api.py` (약 11k 라인)
- 근거 문서: `docs/source/en/concepts/git_vs_http.md`

## 2. 다운로드 및 캐시 시스템

- `hf_hub_download()`: 단일 파일 다운로드, 버전 인식 로컬 캐시 사용
- `snapshot_download()`: 저장소 전체를 특정 revision으로 다운로드
- `hf_hub_url()`: 파일 URL 생성. 작은 파일은 직접 반환, 큰 파일(git-lfs)은 CDN으로 리다이렉트
- 주요 파일: `file_download.py`, `_snapshot_download.py`, `utils/_cache_manager.py`
- 근거 문서: `src/huggingface_hub/README.md`, `docs/source/en/guides/manage-cache.md`

## 3. 업로드 및 커밋 계층

- `upload_file`, `upload_folder`, `create_commit` 등이 하위 커밋 API를 호출합니다.
- 대용량 파일은 Git LFS 배치 업로드를 사용합니다.
- 주요 파일: `_commit_api.py` (`CommitOperationAdd/Delete/Copy`), `lfs.py`, `_commit_scheduler.py`
- 근거 문서: `docs/source/en/guides/upload.md`, `docs/source/en/guides/repository.md`

## 4. Inference 계층

- `InferenceClient`가 40개 이상의 작업(task) 메서드를 제공합니다.
- 프로바이더 어댑터(`inference/_providers/`, 24개 이상: hf_inference, openai, cohere, together, fal 등)가 통일된 API를 각 프로바이더의 HTTP API로 변환합니다.
- 타입 정의는 `inference/_generated/`에 자동 생성되며 수동 수정하지 않습니다.
- 비동기 클라이언트도 자동 생성됩니다.
- 근거 문서: `docs/source/en/guides/inference.md`, `AGENTS.md`

## 5. CLI 계층 (`hf`)

- 진입점: `cli/hf.py` (Typer 앱)
- 하위 명령 그룹: `auth`, `repos`, `spaces`, `models`, `datasets`, `buckets`, `jobs`, `collections`, `webhooks` 등
- 출력 형식(auto/json/human/quiet/agent)과 확인 프롬프트(`out.confirm`)를 공통 헬퍼로 처리합니다.
- 근거 문서: `docs/source/en/guides/cli.md`, `docs/source/en/guides/cli-extensions.md`, `AGENTS.md`

## 6. Jobs, Spaces, Webhooks

- Jobs: 컴퓨트 작업 실행 및 하드웨어 선택 (`_jobs_api.py`, `guides/jobs.md`)
- Spaces: 런타임, 하드웨어, 시크릿, 스토리지 관리 (`_space_api.py`, `guides/manage-spaces.md`)
- Webhooks: 수신 서버를 Space로 배포 (`_webhooks_server.py`, `_webhooks_payload.py`, `guides/webhooks.md`)

## 7. 프레임워크 연동 및 모델 카드

- `ModelHubMixin`: ML 프레임워크 모델의 Hub 저장/로드 기반 클래스 (`hub_mixin.py`)
- `RepoCard`, `ModelCard`, `DatasetCard`: 메타데이터 관리 (`repocard.py`, `repocard_data.py`)
- 근거 문서: `docs/source/en/guides/integrations.md`, `docs/source/en/guides/model-cards.md`

## 8. 파일시스템 인터페이스

- `HfFileSystem`: fsspec 기반으로 Hub 저장소를 파일 객체처럼 다룹니다 (`hf_file_system.py`).
- 근거 문서: `docs/source/en/guides/hf_file_system.md`

## 9. 공통 유틸리티

- HTTP 재시도/백오프: `utils/_http.py` (`http_backoff`, `http_stream_backoff`)
- 인증 토큰 조회: `utils/_auth.py`, `_login.py`
- 입력 검증: `utils/_validators.py` (`@validate_hf_hub_args`)
- 설정 상수: `constants.py` (환경변수 기반 타임아웃, 캐시 경로, 엔드포인트)
- 예외 계층: `errors.py` (`HfHubHTTPError`, `RepositoryNotFoundError` 등)
