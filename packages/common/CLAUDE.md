# common — 계약 패키지

이 패키지는 모든 모듈이 공유하는 **계약**이다. 인터페이스(Protocol), 데이터 타입(pydantic), 오류, API 상수만 둔다.

## 하지 않는 일

- 로직, 외부 서비스 호출, 모듈 구현 import를 넣지 않는다. 허용 의존성은 `pydantic`뿐이다.
- 사람의 명시적 지시 없이 이 폴더를 수정하지 않는다. 모든 변경은 **계약 변경 PR**이다.

## 변경하면

1. `types.py`·`interfaces.py`·`api.py` 수정 (필드 추가는 선택으로만, 삭제·이름 변경·의미 변경은 모든 소비자를 같은 PR에서 고친다)
2. `uv run python scripts/export_openapi.py`
3. `uv run python scripts/check_examples.py`와 `scripts/check_dataset_mapping.py` 통과
4. `uv run python scripts/build_contract_viewer.py`, 필요하면 `CONTRACTS.md` 갱신
5. 각 모듈의 목업 수정, 영향받는 담당자 승인, 병합 즉시 각자 `dev`를 자기 브랜치에 반영

## 파일

- `types.py`: 모든 데이터 타입. validator가 항상 성립해야 하는 규칙을 강제한다.
- `interfaces.py`: Protocol과 그 계약(독스트링의 '계약' 목록이 구현자가 지킬 규칙이다).
- `entrypoints.py`: `app`이 호출하는 `create_*` 진입점 시그니처.
- `errors.py`, `api.py`: 오류 클래스, 경로·HTTP 상태 매핑.
