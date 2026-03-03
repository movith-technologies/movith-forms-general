# BACKEND EXCEPTION HANDLING — MANDATORY RULES

**Project:** Movith Forms SaaS  
**Audience:** All AI assistants (Cursor, Antigravity, Claude, etc.) and developers  

**CRITICAL:** Read and apply this document before adding, changing, or refactoring any exception or error-handling code in the backend. Do not introduce scenario-specific exception types or move handling logic into endpoints.

---

## 1. PRINCIPLE: ONE EXCEPTION TYPE PER HTTP STATUS

- Each **HTTP status** (409, 404, 400, …) is represented by **one** exception type in the Application layer.
- The **message** varies per use case; the **type** does not.
- **Do not** create new exception types for each business scenario (e.g. no `ActiveWorkflowAlreadyExistsException`, `DuplicateCodeException`, etc.). Use the generic type and pass a specific message.

**Correct:** `throw new ConflictException("Bu FormDefinition için zaten aktif bir WorkflowDefinition mevcut: ...");`  
**Wrong:** Creating `ActiveWorkflowAlreadyExistsException` or any other scenario-specific exception that maps to the same status.

---

## 2. APPLICATION LAYER (Where to throw)

- **Location:** `Backend/src/Application/Common/Exceptions/`
- **Allowed generic types (use only these for API-facing errors):**
  - `ConflictException` → HTTP 409 Conflict
  - `NotFoundException` → HTTP 404 Not Found
  - `BadRequestException` → HTTP 400 Bad Request
  - `ValidationException` → HTTP 400 (validation errors)
  - `ForbiddenAccessException` → HTTP 403 Forbidden
  - `UnauthorizedAccessException` → HTTP 401 Unauthorized
- **Usage:** In command/query handlers, throw the appropriate type with a clear, user-facing message:  
  `throw new ConflictException("Senaryoya özel Türkçe mesaj.");`
- **Do not:** Add new exception classes for new “reasons” that still return the same status. Reuse the existing type and change the message.

---

## 3. WEB LAYER (Where to map to HTTP)

- **Single place:** `Backend/src/Web/Infrastructure/CustomExceptionHandler.cs`
- **Mechanism:** `CustomExceptionHandler` implements `IExceptionHandler`. It registers **one handler per exception type** (e.g. `ConflictException` → `HandleConflictException` → 409 + ProblemDetails).
- **Pipeline:** `Program.cs` must call `builder.Services.AddExceptionHandler<CustomExceptionHandler>();` and `app.UseExceptionHandler();`. Do not remove or bypass these.
- **Do not:**
  - Put try-catch in individual endpoints (e.g. `WorkflowDefinitions.Update`) to convert exceptions to HTTP. The global handler does this.
  - Add inline `UseExceptionHandler(appError => { ... })` logic in `Program.cs`. All mapping stays in `CustomExceptionHandler`.
  - Create new exception types in Application and then add new handler entries for scenario-specific names; use the generic type so one handler covers all 409s (or 404s, 400s, etc.).

---

## 4. ADDING A NEW HTTP STATUS (Rare)

If you truly need a **new** status code (e.g. 422 Unprocessable Entity):

1. **Application:** Add **one** new exception class in `Application/Common/Exceptions/`, e.g. `UnprocessableEntityException`.
2. **Web:** In `CustomExceptionHandler`, add **one** entry to `_exceptionHandlers` and **one** handler method that sets that status and writes a JSON body (e.g. ProblemDetails with `Detail = exception.Message`).
3. **Do not** add multiple exception types for the same status.

---

## 5. RESPONSE SHAPE FOR FRONTEND

- Handled exceptions return **JSON** (e.g. ProblemDetails: `detail`, `title`, `status`). The frontend `apiClient` reads `detail` or `message` for the user-facing text.
- Keep consistency: same status → same response shape (e.g. all 409s use the same handler and structure).

---

## 6. CHECKLIST FOR AI / VIBE CODING

Before generating or refactoring exception-related code:

- [ ] Am I reusing an existing generic exception (e.g. `ConflictException`) with a new message, instead of creating a new exception class?
- [ ] Am I throwing only from the Application layer and mapping only in `CustomExceptionHandler`?
- [ ] Did I avoid adding try-catch in endpoints to turn exceptions into HTTP responses?
- [ ] Did I leave `AddExceptionHandler<CustomExceptionHandler>()` and `UseExceptionHandler()` in place in `Program.cs`?

If any answer is no, correct the approach before committing.

---

**Summary:** One exception type per HTTP status; message varies. All mapping in `CustomExceptionHandler`; no endpoint-level exception-to-HTTP logic. This keeps the design stable and prevents exception sprawl when using AI or “vibe” coding.
