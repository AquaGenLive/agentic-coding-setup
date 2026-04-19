---
name: tech_security_check
description: >
  Automated security review of a codebase section. Spawns parallel agents to
  audit authentication, input validation, data access control, configuration,
  dependencies, error handling, and rate limiting. Produces a prioritized
  findings report with checkable todo items for tracking remediation progress.
  Use this skill whenever the user asks to "security review", "audit", "check
  for vulnerabilities", "security check", or "pentest" any part of the codebase.
  Also triggers when users mention OWASP, CVEs, hardcoded secrets, injection
  risks, or ask "is this secure?". Accepts a section parameter specifying what
  to review (e.g., "backend", "frontend", "api", "database", or a directory path).
argument-hint: <section> (e.g., "backend", "frontend", "api", "database", or a path)
---

# Security Review Workflow

You are running an automated security review of a codebase section. You will resolve the scope, spawn parallel review agents covering different security domains, aggregate their findings, and produce a structured report with prioritized, actionable items.

The core principle: **breadth and accuracy over speed**. Every finding must include the exact file path and line number so developers can locate and fix the issue. False negatives (missed vulnerabilities) are worse than false positives (flagging something safe), so when in doubt, flag it and note the uncertainty.

## Input

The section to review is: **$ARGUMENTS**

Parse the arguments:
- The section name (required): identifies which part of the codebase to review

If no section argument is provided, read `CLAUDE.md` and list the available sections of the project (e.g., backend, frontend, database, etc.) based on the project structure described there. Ask the user to pick one before proceeding.

## Phase 1: Resolve Scope and Context

1. **Read project context.** Read `CLAUDE.md` to understand the tech stack, project structure, conventions, and architecture. Also check `package.json`, `pom.xml`, `requirements.txt`, `go.mod`, or equivalent for dependency information.

2. **Map the section argument to directories.** Use CLAUDE.md's project structure to resolve the section:

   | Section argument | Typical resolution | Notes |
   |---|---|---|
   | `backend` | Backend source directory + config files | Include root config that affects backend (docker-compose, .env.example) |
   | `frontend` | Frontend source directory | Include public/ and config files |
   | `api` | Controllers/routes + DTOs + security config | Focus on the API surface |
   | `database` | Migrations + repositories/DAOs + entity models | Focus on schema and queries |
   | `e2e` | End-to-end test directory | Focus on test credential handling |
   | `config` | All configuration files across the project | YAML, .env, Docker, CI/CD |
   | A file path | That exact path | Verify it exists |
   | `all` | Entire project | Split across agents by directory |

   Adapt this mapping to the actual project structure from CLAUDE.md. Not every project will have all sections.

3. **Capture metadata** for the report header:
   - Current date
   - Current git branch (`git rev-parse --abbrev-ref HEAD`)
   - Scope description (what directories and file types are covered)

4. **Identify the tech stack** so agents can apply the right checklists. Examples:
   - Java + Spring Boot: check Bean Validation, Spring Security, JPA queries, Liquibase migrations
   - React + TypeScript: check unsafe HTML rendering, token storage, client-side validation
   - Python + Django: check ORM queries, CSRF middleware, template escaping
   - Node + Express: check middleware ordering, helmet headers, parameterized queries
   - The skill adapts to whatever stack CLAUDE.md describes

## Phase 2: Spawn Parallel Review Agents

Launch **4 Explore agents in a single message** (all in parallel). Each agent covers a cluster of related security domains. Provide each agent with:
- The resolved directory paths for this section
- The tech stack summary from Phase 1
- The domain-specific checklist below (adapted to the tech stack)
- Clear instructions to report findings in this intermediate format:

```
FINDING:
- Title: {descriptive title}
- Severity: {Critical|High|Medium|Low}
- Description: {what the issue is and why it matters}
- File: {exact file path}
- Line: {line number(s)}
- Recommendation: {concrete fix}

POSITIVE:
- {Description of well-implemented security measure} ({file path}:{line})
```

### Agent 1: Authentication & Authorization

Review all authentication and authorization mechanisms in the resolved scope.

**Core checklist:**
- JWT/session/token implementation: algorithm strength, secret handling, expiry, refresh mechanism
- Token storage: where tokens are stored, httpOnly flags, secure flags
- Password handling: hashing algorithm and strength, complexity requirements, change flow
- Login/registration: rate limiting, user enumeration risk (timing attacks), account lockout
- Role-based access control: endpoint protection, role checks in controllers and services
- CORS configuration: allowed origins, credentials handling, wildcard risks
- Session management: stateless vs stateful, session fixation, concurrent sessions
- OAuth/SSO integration: redirect URI validation, state parameter, token exchange
- Logout: token revocation, session invalidation

**Backend-specific extras:**
- Spring Security filter chain ordering, SecurityConfig rules
- `@PreAuthorize`, `@Secured`, `@RolesAllowed` usage
- Custom authentication filters

**Frontend-specific extras:**
- Token storage mechanism (localStorage vs cookies)
- Auth state management (Zustand, Context, Redux)
- Protected route implementation
- Token refresh flow on the client side

### Agent 2: Input Validation & Injection Prevention

Review all input handling for injection and validation weaknesses.

**Core checklist:**
- SQL injection: parameterized queries vs string concatenation, ORM query safety
- XSS: output encoding, unsafe HTML rendering, template injection, user content display
- Command injection: shell command construction, subprocess calls with user input
- Path traversal: file path construction from user input, directory escape
- Request validation: DTO/schema validation completeness, missing constraints, type coercion
- Parameter validation: query params, path variables, headers without validation
- Size/bounds validation: missing max lengths on strings, missing decimal max on amounts, unbounded pagination
- Regex safety: ReDoS risk from user-supplied or complex patterns
- Deserialization: unsafe deserialization of user-controlled data
- File upload: type validation, size limits, storage path safety

**Backend-specific extras:**
- Bean Validation annotations (`@Valid`, `@NotBlank`, `@Size`, `@Pattern`, `@Email`, `@DecimalMin`/`@DecimalMax`)
- Spring `@RequestParam` and `@PathVariable` without validation constraints
- JPA `@Query` with string concatenation vs `@Param`

**Frontend-specific extras:**
- Client-side-only validation without server-side backup
- Unsafe rendering of user content (innerHTML, v-html, [innerHTML])
- URL construction from user input

### Agent 3: Data Access Control & User Isolation

Review all data access patterns for authorization bypass and data leakage.

**Core checklist:**
- IDOR (Insecure Direct Object Reference): can user A access user B's data by guessing IDs?
- User isolation: do all queries filter by user_id/owner_id? Look for `findById()` without user filter
- Authorization in service layer: ownership checks before update/delete operations
- Repository/DAO query patterns: consistent use of user-scoped queries
- Transaction isolation: `@Transactional` correctness, race conditions in read-modify-write
- Sensitive data in responses: password hashes, internal IDs, other users' data in API responses
- Mass assignment: can users set fields they shouldn't (admin flags, IDs, internal state)?
- Cascade behavior: does deleting a parent leak or orphan child records?
- Soft delete: can "deleted" records still be accessed?
- Audit trail: are sensitive operations logged?

**Backend-specific extras:**
- Spring Data JPA: `findByIdAndUserId()` pattern consistency
- `@EntityGraph` and fetch joins for N+1 prevention (DoS vector via slow queries)
- Atomic balance/counter updates (read-modify-write race conditions)

**Frontend-specific extras:**
- Sensitive data cached in browser state (Zustand, Redux, localStorage)
- User data visible in URLs or query parameters
- API responses containing more data than the UI displays

### Agent 4: Configuration, Dependencies & Infrastructure

Review all configuration, dependency, and infrastructure security.

**Core checklist:**
- Hardcoded secrets: API keys, JWT secrets, database passwords in source code or config files
- Default credentials: well-known default usernames/passwords in configs
- Environment variable handling: fallback values that expose secrets, missing validation at startup
- Dependency versions: snapshot/unstable dependencies (supply chain risk), non-LTS runtime versions, known CVEs
- Security headers: HSTS, X-Frame-Options, Content-Type-Options, CSP, Referrer-Policy, Permissions-Policy
- Error handling: stack trace leakage in responses, verbose error messages to clients, information disclosure
- Debug/documentation endpoints: Swagger/OpenAPI, actuator, debug routes exposed in production
- Rate limiting: presence and configuration, per-endpoint vs global, bypass vectors
- HTTPS/TLS: enforcement, certificate validation, mixed content
- Logging: sensitive data (passwords, tokens, PII) in log output
- Docker/container: running as root, exposed ports, secret mounting, base image currency
- CI/CD: secrets in pipeline configs, artifact integrity

**Backend-specific extras:**
- `application.yml` / `application.properties` security settings
- Spring profiles: dev settings leaking into production
- Liquibase/Flyway migrations: missing database constraints, insecure defaults
- Database indexes on security-critical columns (user_id, email)

**Frontend-specific extras:**
- `.env` files with secrets committed to repo
- API URLs and keys in client-side bundles
- Source maps enabled in production
- CSP meta tags or header configuration
- Third-party script integrity (SRI hashes)

## Phase 3: Aggregate and Deduplicate

Once all 4 agents report back, process their findings:

1. **Collect** all findings from all agents into a unified list.

2. **Deduplicate** by comparing findings:
   - Same file path + overlapping line range (within 5 lines) = likely the same issue
   - Same root cause described differently by two agents = merge into one finding
   - Same recommendation targeting the same code = merge
   - When merging, keep the more detailed description and combine any unique context from both agents

3. **Apply the priority rubric** to validate or adjust agent-assigned severities:

   | Priority | Criteria | Examples |
   |---|---|---|
   | **Critical** | Directly exploitable with high impact. No special conditions needed. | Secrets in source code, authentication bypass, SQL injection, RCE vectors, supply chain compromise (snapshot deps) |
   | **High** | Exploitable with specific conditions. Significant impact if exploited. | IDOR / missing user_id filters, user enumeration, no token revocation, unprotected admin endpoints, unvalidated inputs on sensitive operations |
   | **Medium** | Defense-in-depth gap. Requires chaining with other issues or specific misconfiguration. | Missing input bounds (@DecimalMax), CORS misconfiguration risk, unbounded pagination, no account lockout, missing database indexes |
   | **Low** | Best-practice violation. Theoretical risk, low impact. | Broad exception catching, dead configuration, limited password entropy, missing Swagger annotation, cosmetic security docs |

4. **Assign sequential IDs**: SEC-01, SEC-02, etc. Order by priority (all Critical first, then High, Medium, Low). Within a priority level, order by impact.

5. **Collect positive findings** from all agents into a consolidated list. These are security measures that are well-implemented and worth acknowledging.

## Phase 4: Write the Report

Create the output file at `spec/security-check/{section}.md`. If the directory does not exist, create it.

Use this exact format:

```markdown
# {Section Title Case} Security Review

**Date:** {YYYY-MM-DD}
**Scope:** {Description of what was reviewed}
**Branch:** `{branch-name}`

---

## Critical

- [ ] **SEC-{NN}: {Finding Title}**
  - **Priority:** Critical
  - **Description:** {What the issue is and why it matters}
  - **Location:**
    - `{path/to/file}` -- line {XX} ({brief context})
  - **Recommendation:** {Concrete steps to fix the issue}

## High

{Same structure as Critical}

## Medium

{Same structure}

## Low

{Same structure}

---

## Positive Findings

These security measures are well-implemented and worth noting:

- **{Measure name}:** {Brief description} (`{file path}:{line}`)
```

Rules for the report:
- Every finding has a checkbox (`- [ ]`) so it works as a progress tracker
- Every finding has a unique ID (SEC-01, SEC-02, ...) for reference in commits and PRs
- Every finding includes at least one specific file path and line number
- Recommendations are concrete and actionable (not vague "consider improving")
- If a severity section has no findings, include the heading with "No {level} findings identified."
- The Positive Findings section should have at least 5 items if the codebase has reasonable security

## Phase 5: Summary

After writing the report, present a brief summary to the user:

1. **Findings count by severity** in a table:
   ```
   | Severity | Count |
   |----------|-------|
   | Critical | X     |
   | High     | X     |
   | Medium   | X     |
   | Low      | X     |
   ```

2. **Top 3 most critical items** with their SEC-ID and one-line description

3. **Output file path** so the user knows where to find the full report

Do not repeat the full report content in the summary — the user can read the file.
