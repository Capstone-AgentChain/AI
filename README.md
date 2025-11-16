# AI Backend 개발 프로젝트

## 프로젝트 개요

**목적**: AI 플레이그라운드 백엔드 개발 (OpenAI, Anthropic Claude, Google Gemini API 통합)

**환경**: 
- Windows 11
- Python 3.13
- VS Code
- FastAPI + ngrok

**목표**: 
- 프론트엔드에서 여러 AI 모델을 하나의 API로 사용
- 토큰 사용량 실시간 추적 (세션별/일일/월별/모델별)
- 비용 자동 계산
- API 키는 백엔드에서 관리 (프론트엔드 노출 X)

**MVP 개발**: 빠른 프로토타입

---

## 프로젝트 구조

```
ai_openapi_BE/
│
├── main.py                      # FastAPI 앱 + 라우터 등록
├── requirements.txt             # 패키지 목록
├── .env.example                 # 실행을 위한 환경 변수 템플릿
├── .env                         # API 키 (Git 제외)
├── .gitignore                   # Git 제외 파일
│
├── config.py                    # 설정 관리
├── database.py                  # SQLite 연결 및 모델
│
├── routers/
│   ├── __init__.py
│   ├── chat.py                  # POST /api/chat/completions
│   └── usage.py                 # GET /api/usage/*
│
├── services/
│   ├── __init__.py
│   ├── openai_service.py        # OpenAI 통합
│   ├── anthropic_service.py     # Claude 통합
│   ├── gemini_service.py        # Gemini 통합
│   ├── usage_tracker.py         # 사용량 추적
│   └── cost_calculator.py       # 비용 계산
│
├── models/
│   ├── __init__.py
│   └── schemas.py               # Pydantic 모델
│
├── utils/
│   ├── __init__.py
│   └── token_counter.py         # 토큰 카운팅
│
└── data/
    ├── usage.db                 # SQLite DB (자동 생성)
    └── models_config.json       # 모델 정보/가격
```

---

## 요구사항 및 구현 상세 (예시 변경 가능)

### 1. `requirements.txt`

```txt
fastapi==0.104.1
uvicorn[standard]==0.24.0
python-dotenv==1.0.0
sqlalchemy==2.0.23
aiosqlite==0.19.0
pydantic==2.5.0

openai==1.3.0
anthropic==0.7.0
google-generativeai==0.3.0

tiktoken==0.5.1
httpx==0.25.0
```

---

### 2. `.env` (예시)

```env
OPENAI_API_KEY=sk-proj-...
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_API_KEY=AIza...

DATABASE_URL=sqlite:///./data/usage.db
ALLOWED_ORIGINS=https://ai-modelhub-platform.vercel.app,http://localhost:5173

PORT=8000
DEBUG=True
```

> ℹ️ **TIP**: 저장소에는 비밀값이 제거된 `.env.example` 파일이 포함되어 있습니다. 이 파일을 복사해 `.env`로 이름을 변경한 뒤 각 API 키와 설정 값을 채워 넣으면 됩니다.

---

### 3. `.gitignore`

```
.env
__pycache__/
*.pyc
*.db
data/usage.db
venv/
.vscode/
```

---
