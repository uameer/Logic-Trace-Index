# Logic-Trace Trajectories

> Reference trajectory index for agentic coding assistants.
> Add path to your `CLAUDE.md` or `.cursorrules`.

---

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
