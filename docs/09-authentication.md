# 09 — Authentication Module (Phase 5 — First Coding Module)

## Scope
This is the FIRST module you code. Everything else depends on it.

## What's Included
- JWT-based authentication
- Spring Security configuration
- Role-Based Access Control (RBAC): STUDENT / FACULTY / HOD / ADMIN
- PostgreSQL-backed user store

## Entities
- `User` — id, email, password (hashed), role_id (FK), is_active, created_at
- `Role` — id, name
- (Faculty/Student entities link to User via `user_id` FK — built in later
  modules, but the relationship should be planned now)

## Flow
1. **Register** (Admin-driven for Faculty/Student, or self-signup for
   Student with email domain check) → password hashed with BCrypt → user
   saved.
2. **Login** → credentials validated → JWT issued (contains `userId`,
   `role`, `exp`).
3. **Every subsequent request** → `JwtAuthenticationFilter` validates
   token → populates `SecurityContext`.
4. **Authorization** → `@PreAuthorize("hasRole('ADMIN')")` (or similar) on
   controller methods per endpoint.

## Checklist (mark off as you build)
- [ ] `User`, `Role` entities + Flyway migration
- [ ] `UserRepository`
- [ ] `PasswordEncoder` bean (BCrypt)
- [ ] `JwtService` (generate/validate/parse token)
- [ ] `JwtAuthenticationFilter`
- [ ] `SecurityConfig` (permit `/api/auth/**`, secure everything else)
- [ ] `AuthController` — `/register`, `/login`, `/refresh`
- [ ] Global exception handling for `BadCredentialsException`,
      `ExpiredJwtException`
- [ ] Unit tests: login success/failure, expired token rejected
- [ ] Postman/curl collection to manually verify all 4 roles

## Design Decisions to Note
- Where does `role` live — on `User` directly, or is `User` generic and
  `Faculty`/`Student`/`HOD`/`Admin` are separate profile tables linked by
  `user_id`? → **Recommended: generic `User` + `Role`, with
  Faculty/Student as separate profile entities** (cleaner, avoids one
  giant polymorphic User table).
- Refresh token strategy: short-lived access token (e.g. 15 min) + longer
  refresh token (e.g. 7 days), refresh token stored hashed in DB or Redis.

## Done Criteria
A user of each role can register (or be created), log in, receive a valid
JWT, and hitting a protected endpoint with the wrong role returns `403`.
