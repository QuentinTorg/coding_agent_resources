# Workspace-Level Agent Context (The "Landmines-Only" Template)

*This is a template for your `AGENTS.md` (or `CLAUDE.md`, `.cursorrules`, etc.) file. According to the latest research, the most effective context files are **human-curated and hyper-minimal**. Use this file to document only the non-obvious "gotchas" and strict constraints that an AI agent cannot infer from reading your code.*

---

# Agent Context: [Project Name]

> **The Golden Rule:** If it's obvious from the code or the README, **delete it**. Do not include standard CLI commands (like `npm install`) or general programming rules. Focus only on the "invisible" quirks of this specific codebase.

## 🛑 Strict Architectural Constraints
*List hard rules that the AI frequently breaks or cannot infer.*
* **State Management:** [e.g., "Always use Zustand for global state. Do NOT use React Context under any circumstances."]
* **Styling:** [e.g., "We strictly use Tailwind CSS. Do not generate custom inline styles or new .css files."]

## ⚠️ Known Pitfalls & Institutional Memory
*The "Trap Detector". List specific debugging rabbit holes the AI has fallen into before.*
* **Database Migrations:** [e.g., "When updating the Prisma schema, do NOT use `npx prisma db push`. You must generate a migration using `npx prisma migrate dev`."]
* **API Routing:** [e.g., "We are using Next.js App Router, but legacy auth still relies on Pages Router. Do not attempt to move auth to the app directory."]

## 🛠️ Custom/Quirky CLI Commands
*Only include commands that deviate from the standard.*
* **Running Tests:** [e.g., "To test the billing webhook locally, you MUST use `npm run stripe:listen` before running `npm run test`."]

## 📁 Critical File Boundaries
*Tell the AI what NOT to touch or where to be extremely careful.*
* [e.g., "Never modify `lib/core-engine.ts` without explicitly asking for permission first."]

---

### 💡 How to maintain this file:
1. **Iterate after failures:** The next time the agent hallucinates a fix or wastes 20 minutes, add a single bullet point to "Known Pitfalls" so it never happens again.
2. **Prune ruthlessly:** If a rule is no longer applicable or the agent has "learned" it from the code structure, delete it.
3. **Avoid Aspirational Rules:** Only document what is *actually* enforced in the code today.
