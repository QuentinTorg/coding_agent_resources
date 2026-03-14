# Python FastAPI Best Practices Context

*This is an `AGENTS.md` snippet. Copy the contents of this file into your workspace `AGENTS.md` (or `GEMINI.md`) file to establish a strict, highly opinionated, modern baseline for a Python FastAPI backend project. **Note for users:** The rules below represent modern industry consensus, but they are highly opinionated (e.g., enforcing Ruff and SQLAlchemy 2.0). Please feel free to modify these rules so that the opinions reflect your own stack preferences (e.g., swapping SQLAlchemy for SQLModel, or Ruff for Flake8).*

---

## 🚀 FastAPI Architecture & Python Rules

You are acting as a Senior Backend Architect. When writing or modifying Python FastAPI code in this repository, you MUST strictly adhere to the following opinionated best practices:

### 1. Application Structure
- **Modular Routing:** Do not write monolithic `main.py` files. Use `APIRouter` to split endpoints into logical domains. 
- **Strict Separation of Concerns:**
  - `api/routers/`: Contains `APIRouter` endpoint definitions. ONLY handles HTTP request/response validation and routing.
  - `core/services/`: Contains all business logic. Routers call these services.
  - `core/schemas/`: Contains Pydantic V2 models for validation and serialization.
  - `infrastructure/database/models/`: Contains SQLAlchemy 2.0 declarative models.

### 2. Stack & Tooling Opinions
- **Package Management:** Prefer `uv` or `Poetry` over plain `pip` and `requirements.txt`.
- **Linting & Formatting:** Exclusively use **Ruff** for all linting and formatting. Do not use Flake8, Black, or isort.
- **Database ORM:** Exclusively use **SQLAlchemy 2.0** with asynchronous engines (`asyncpg`, `aiosqlite`). Do not use synchronous database drivers.
- **Migrations:** Exclusively use **Alembic** for database migrations.

### 3. Validation & Serialization (Pydantic V2)
- **Strict Pydantic:** Always use Pydantic V2. Do not accept or return raw dictionaries.
- **Explicit `response_model`:** Every endpoint MUST explicitly declare its `response_model` in the decorator.
- **ORM Mode:** Use `ConfigDict(from_attributes=True)` on response schemas to cleanly serialize SQLAlchemy models.

### 4. Dependency Injection
- **Session Management:** Always yield asynchronous database sessions via a FastAPI `Depends()` function so they are safely created and closed per request.
- **Auth & Services:** Inject the current user and necessary service classes via `Depends()` to keep the endpoint logic clean and easily mockable during testing.

### 5. Asynchronous Rules
- **Non-Blocking:** Never use `time.sleep()`, synchronous `requests`, or synchronous DB calls inside an `async def` route. Use `asyncio.sleep()`, `httpx`, and async SQLAlchemy.
- **CPU Bound Tasks:** If you must run a heavy synchronous computation, offload it using `fastapi.concurrency.run_in_threadpool`.

### 6. Error Handling
- **HTTPExceptions:** Do not throw generic Python `Exception` objects from routers. Use FastAPI's `HTTPException` with explicit status codes.
- **Service Exceptions:** The `services/` layer should raise custom domain exceptions (e.g., `UserNotFoundError`). The `main.py` file MUST have global exception handlers that catch these and translate them into standardized JSON `HTTPException` responses.

### 7. Testing
- **Pytest:** Exclusively use `pytest` and `pytest-asyncio`. 
- **Fixtures:** Use `pytest` fixtures extensively for database setup, teardown, and generating mock data. Do not write manual setup/teardown methods.
