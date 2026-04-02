# Logic-Trace (v0.1-alpha)
> **The Execution-Aware Index for Agentic Coding Assistants**

---

## 🚩 The Problem: The "Static Context" Tax
AI coding assistants (Claude Code, Cursor, GitHub Copilot) operate on **Static Codebases**. They read files, but they are blind to the **Runtime Execution Flow**. 

*   **The Result:** AI explores 50K+ tokens of files just to understand a simple login flow.
*   **The Risk:** Hallucinated fixes that break downstream dependencies because the AI doesn't see the "live" side effects.

## 💡 The Solution: Logic-Trace
`Logic-Trace` generates a compact, high-density **Behavioral Index** (`.logic-trace/`) that maps the **"Live Soul"** of your application. Instead of reading code, the AI reads **Trajectories**.

### **Core Specification:**
*   **The Happy Path:** A compressed trace of function calls for core features.
*   **The Error Surface:** Mapping where the app historically fails based on log signatures.
*   **The Side-Effect Map:** A dependency graph of what happens *after* a database write.

## 📊 The "Context Arbitrage"

| Activity | Manual AI Exploration | **Logic-Trace Handshake** |
| :--- | :--- | :--- |
| **Context Window Cost** | 50,000+ Tokens | **<500 Tokens** |
| **Onboarding Time** | Minutes (Scanning) | **Milliseconds (Reading Index)** |
| **Logic Accuracy** | Probabilistic Guessing | **Deterministic Mapping** |

## 🛠 Usage (The Future Handshake)
Add this to your `CLAUDE.md` or `.cursorrules`:
> "Before exploring files, read `.logic-trace/trajectories.md` to understand the execution flow of the Login and Checkout features."

---
*Mapping the 'Inside' of the machine for the 'Brain' of the Assistant.*

### 🗺️ The Output: Trajectory Mapping

## [Flow] User Login
1. Client: `POST /api/login` [body: email, password]
2. AuthMiddleware: `verifyCredentials(email, password)`
3. Database: `users.findFirst({ where: { email } })`
4. Bcrypt: `compare(password, user.hash)`
5. TokenService: `generateJWT(userId)`
6. Response: `200 OK` [set-cookie: session_token]

## [Side-Effects] 
- `logs.create({ action: "LOGIN_SUCCESS", userId })`
- `users.update({ lastLogin: Date.now() })`

## [Known Error Surfaces]
- `401 Unauthorized`: Invalid password (Bcrypt mismatch)
- `404 Not Found`: User does not exist in DB
- `500 Internal`: JWT Secret key missing in `.env`

