# Logic-Trace (v0.1-alpha)
> An execution-aware index for agentic coding assistants.

## The Problem
AI coding assistants read files but are blind to runtime 
execution flow. The result: 50K+ tokens spent scanning 
code to understand a single login flow, with hallucinated 
fixes that break downstream dependencies.

## What This Is
A lightweight behavioral index that maps how your application 
actually executes. Instead of reading code, the AI reads 
trajectories.

### Core Specification
- **Happy Path:** Compressed trace of function calls for core features
- **Error Surface:** Where the app historically fails based on log signatures
- **Side-Effect Map:** What happens after a database write

## Example Trajectory: User Login
```
[Flow] User Login
1. Client: POST /api/login [body: email, password]
2. AuthMiddleware: verifyCredentials(email, password)
3. Database: users.findFirst({ where: { email } })
4. Bcrypt: compare(password, user.hash)
5. TokenService: generateJWT(userId)
6. Response: 200 OK [set-cookie: session_token]

[Side-Effects]
- logs.create({ action: "LOGIN_SUCCESS", userId })
- users.update({ lastLogin: Date.now() })

[Known Error Surfaces]
- 401: Invalid password (Bcrypt mismatch)
- 404: User does not exist in DB
- 500: JWT Secret key missing in .env
```

## Industry Bricks
Reference implementations applied to real domains:

| Brick | Domain | Files |
|-------|--------|-------|
| medical-admin | Prior Authorization | [schema](industry-bricks/medical-admin/prior-auth-schema.md) · [server](industry-bricks/medical-admin/server.py) |

## Status
- [x] Spec defined
- [x] Login flow reference trajectory
- [x] Medical-admin industry brick
- [ ] Trace generator script
- [ ] Additional industry bricks

## Stack Context
Logic-Trace is the context layer of the Sovereign Intelligence Stack:
- [Elastic Inference Protocol](https://github.com/uameer/Elastic-Inference-Protocol-0.12) — compute layer
- **Logic-Trace** — context layer ← you are here
- [Truth-Trace Protocol](https://github.com/uameer/Truth-Trace-Protocol) — verification layer
