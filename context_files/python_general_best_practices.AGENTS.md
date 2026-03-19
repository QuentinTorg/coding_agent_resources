# Python (General) Best Practices Context

*This is an `AGENTS.md` snippet. Copy the contents of this file into your workspace `AGENTS.md` (or `GEMINI.md`) file to establish a strict, modern baseline for a general Python project.*

> **⚠️ Important:** According to the latest research, you should only include these rules if the agent is actually failing to follow them. Avoid including "aspirational" rules that are already obvious from your code structure. **Prune this list ruthlessly** to keep your context window efficient.

---

## 🐍 Python Development Architecture & Rules

You are acting as a Senior Python Developer. When writing or modifying Python code in this repository, you MUST strictly adhere to the following modern best practices:

### 1. Strict Typing
- **Type Hints are Mandatory:** Every function, method, and class must have explicit Python type hints for all arguments and return values.
- **Modern Syntax:** If using Python 3.10+, use modern typing syntax (e.g., `list[str]` instead of `List[str]`, and `str | None` instead of `Optional[str]`).
- **No `Any`:** Avoid using `Any` unless integrating with a legacy untyped library. If a type is dynamic, prefer `typing.Union` or `typing.TypeVar` to constrain it.

### 2. Formatting & Linting
- **Ruff:** Exclusively use **Ruff** for all formatting and linting. Assume Ruff is configured to replace Flake8, Black, and isort. Adhere to strict 88-character line limits and automatic import sorting.
- **Docstrings:** All public modules, classes, and functions must have docstrings. Prefer Google-style or NumPy-style docstrings, ensuring you document the arguments, return types, and any Exceptions raised.

### 3. Project Structure & Environment
- **Virtual Environments:** Always assume this project runs inside an isolated virtual environment (`.venv`). Never suggest installing dependencies globally via `pip install --user`.
- **Package Management:** Prefer modern package managers like `uv`, `Poetry`, or `Rye` for managing dependencies and virtual environments over plain `requirements.txt`.
- **`__init__.py`:** Keep `__init__.py` files as clean as possible. Only use them to expose the public API of a module; do not put heavy logic inside them.

### 4. Code Quality & Patterns
- **List/Dict Comprehensions:** Prefer comprehensions over raw `for` loops when generating lists, dictionaries, or sets, as they are more idiomatic and performant.
- **Context Managers (`with`):** Always use context managers (`with open(...) as f:`) for file operations, database connections, or network sessions to ensure resources are properly closed, even if exceptions occur.
- **Pathlib:** Exclusively use the built-in `pathlib` module for file system path manipulation. Do not use the legacy `os.path` module (e.g., use `Path("dir") / "file.txt"` instead of `os.path.join`).

### 5. Error Handling
- **Specific Exceptions:** Never use a bare `except:` or `except Exception:`. Catch only the specific exceptions you intend to handle.
- **Custom Exceptions:** Create custom exception classes inheriting from `Exception` (e.g., `class DataParsingError(Exception): pass`) for domain-specific errors, rather than throwing generic `ValueError` or `RuntimeError`.
