# LlamaIndex + Ollama 기반 FastAPI 챗봇

이 프로젝트는 **로컬 LLM (Ollama)** 을 활용하여, 사용자의 문서를 기반으로 질의응답할 수 있는 **FastAPI 기반 챗봇 서버**입니다.  
내부적으로 [LlamaIndex](https://github.com/jerryjliu/llama_index)를 사용하여 문서를 인덱싱하고, [Ollama](https://ollama.com)를 통해 모델 추론을 수행합니다.

---

## 기술 스택

- **LLM 서버**: [Ollama](https://ollama.com)
- **벡터 인덱싱**: LlamaIndex (`VectorStoreIndex`)
- **백엔드 API**: FastAPI
- **컨테이너 구성**: Docker + Docker Compose
- **임베딩/쿼리**: 로컬 문서를 기반으로 LLM 추론

---

## 프로젝트 구조

```
.
├── app/
│   ├── main.py              # FastAPI 진입점
│   ├── indexing.py          # 문서 로딩 및 인덱싱
│   └── llm_config.py        # LLM 설정
├── documents/               # 질의응답에 사용할 문서들
├── storage/                 # llama_index 인덱스 저장 폴더
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 실행 방법

### 1. 사전 준비

Ollama가 사용할 LLM 모델을 다운로드합니다. 예:

```bash
ollama pull mistral
```

### 2. Docker Compose로 실행

```bash
docker compose up --build
```

- `ollama_dev`: Ollama LLM 서버
- `backend_dev`: FastAPI 챗봇 서버

### 3. API 테스트

FastAPI가 실행되면 `http://localhost:8000`에서 사용 가능하며, 예시는 다음과 같습니다:

```bash
curl -X POST http://localhost:8000/chat/ \
  -H "Content-Type: application/json" \
  -d '{"query": "이 프로젝트는 무엇을 하나요?"}'
```

응답 예시:

```json
{
  "response": "이 프로젝트는 Ollama와 llama_index를 사용해 문서를 기반으로 답변하는 챗봇입니다."
}
```

---

## LLM 모델 설정

- 사용 모델은 기본적으로 `mistral` (또는 `llama3`, `phi`, `gemma` 등)
- `app/llm_config.py` 또는 `.env`에서 변경 가능
- `OLLAMA_HOST=http://ollama_dev:11434`로 설정됨 (도커 내 연결)

---

## 문서 인덱싱

- `documents/` 폴더에 `.txt`, `.md` 등 문서를 넣으면 자동 인덱싱됩니다.
- llama_index가 처음 실행 시 `storage/` 폴더에 벡터 저장

---

## 향후 개선 계획 (TODO)

- [ ] 사용자별 세션 대화 저장
- [ ] OpenAPI 문서 UI 개선

---

