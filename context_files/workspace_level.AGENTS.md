# Workspace-Level Agent Context

*This is an example `AGENTS.md` file intended for the root directory of your project repository. It provides the agent with project-specific context that isn't typically found in a `README.md` (which is geared towards human onboarding). Providing this context helps the agent navigate, build, and test your code without needing to ask basic questions or guess your project structure.*

---

## 🏗️ Project Structure & Architecture
*Briefly explain where different parts of the application live so the agent knows where to start looking.*
- **`/src/components/`**: Contains all reusable React components.
- **`/src/api/`**: Contains all external API wrappers and network requests.
- **`/tests/unit/`**: Contains unit tests. Tests must mirror the structure of the `/src/` directory.
- **State Management**: We use Redux Toolkit. State slices should be placed in `/src/store/`.

## 🛠️ Build & Run Commands
*Provide the exact commands the agent should use to build, lint, and run the project.*
- **Install dependencies**: `npm install --silent`
- **Run local dev server**: `npm run dev` (run in background if needed)
- **Linting**: `npm run lint` (Always run this and fix errors before committing)
- **Formatting**: `npm run format` (We use Prettier)

## 🧪 Testing Guidelines
*Explain how to test the code and any project-specific testing rules.*
- **Test Runner**: We use Jest and React Testing Library.
- **Running Tests**: Run tests via `npm run test`. To run a specific file, use `npm run test -- <filename>`.
- **Coverage**: All new components must have at least 80% test coverage. Do not write snapshot tests; prefer querying by accessibility roles.
- **Mocks**: External API calls must always be mocked in tests using MSW (Mock Service Worker).

## 📝 Project-Specific Coding Conventions
*Detail any strict rules that apply uniquely to this codebase.*
- **Typing**: This is a strict TypeScript project. Avoid using `any`; define precise interfaces for all props and state.
- **Imports**: Always use absolute imports for internal modules (e.g., `import { Button } from '@/components/Button'` instead of `../../components/Button`).
- **Error Handling**: Use the custom `AppError` class located in `/src/utils/errors.ts` instead of throwing generic `Error` objects.

## 📦 Commit & PR Guidelines
*Specify how the agent should format its commits for this specific project.*
- **Format**: We strictly follow Conventional Commits (e.g., `feat: add user login`, `fix: resolve crash on logout`).
- **Scope**: Always include the scope of the change in the commit message (e.g., `feat(auth): ...`).
- **PRs**: When drafting a PR description, ensure you fill out the standard project template, including linking the relevant Jira ticket.
