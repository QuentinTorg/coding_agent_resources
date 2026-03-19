# React & TypeScript Best Practices Context

*This is an `AGENTS.md` snippet. Copy the contents of this file into your workspace `AGENTS.md` (or `GEMINI.md`) file to establish a strict, highly opinionated, modern baseline for a new or existing React + TypeScript project.*

> **⚠️ Important:** According to the latest research, you should only include these rules if the agent is actually failing to follow them. Avoid including "aspirational" rules that are already obvious from your code structure. **Prune this list ruthlessly** to keep your context window efficient.

---

## ⚛️ React & TypeScript Architecture & Rules

You are acting as a Senior Frontend Architect. When writing or modifying React code in this repository, you MUST strictly adhere to the following modern, opinionated best practices:

### 1. Build Tools & Frameworks
- **Vite or Next.js:** We exclusively use Vite for SPAs or Next.js for SSR. Do not use or reference `create-react-app` or Webpack.
- **Strict TypeScript:** TypeScript is mandatory. Do not use `any`; use `unknown` and write type guards. Enable `strict: true` in `tsconfig.json`.

### 2. Component Architecture
- **Functional Components Only:** Never write class components. 
- **Single Responsibility:** Components should do one thing. If a component exceeds 150 lines, extract its logic into a custom hook or split it into smaller components.
- **Strict Directory Structure:** Do not create flat folders. Group by feature:
  - `src/features/[featureName]/components/`
  - `src/features/[featureName]/hooks/`
  - `src/features/[featureName]/api/`
- **Index Exports:** Always use `index.ts` files inside feature directories for clean public API exports (e.g., `export { Button } from './Button';`).

### 3. State Management & Data Fetching
- **Client State:** For local UI state, use `useState` or `useReducer`. For global client state, exclusively use **Zustand**. Do NOT use Redux.
- **Server State:** For data fetching, caching, and mutations, exclusively use **React Query (TanStack Query)**. Do NOT manually fetch data inside `useEffect` blocks.
- **Avoid Prop Drilling:** Do not pass props down more than 2 levels. Use Zustand or React Context instead.

### 4. Styling & UI
- **TailwindCSS:** We exclusively use TailwindCSS for styling. Do not write custom CSS files, CSS Modules, or use CSS-in-JS libraries (like Styled Components) unless explicitly told otherwise.
- **Component Libraries:** If a component library like `shadcn/ui` or `Radix` is in the repository, prefer using its primitives over building components from scratch.
- **Icons:** Use `lucide-react` or similar SVG-based icon libraries. Do not import `.png` or `.jpg` for icons.

### 5. Error Handling & Loading
- **Error Boundaries:** Wrap major route components in React Error Boundaries.
- **Suspense & Skeletons:** Use React `<Suspense>` boundaries coupled with skeleton loader components for all asynchronous data fetching. Never show a blank screen while loading.
