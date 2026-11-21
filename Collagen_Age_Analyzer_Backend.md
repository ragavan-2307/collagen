# COLLAGEN AGE ANALYZER --- BACKEND DEVELOPMENT SPECIFICATION

**Project:** Understanding Collagen Changes in Aging Skin\
**Document scope:** Backend only\
**Purpose:** Implement secure APIs, assessment processing, educational
scoring, recommendation delivery, authentication, report generation, and
administrative services.

------------------------------------------------------------------------

## 1. Backend Objective

Build a maintainable, secure backend for the Collagen Age Analyzer using
Node.js and Express.js. The backend must validate questionnaire
responses, apply documented educational scoring rules, persist permitted
records in PostgreSQL, return results, provide recommendations, support
optional accounts, and enforce access control.

The backend is the source of truth for assessment calculations and
result categories.

**Scientific limitation:** The product is educational. Questionnaire
responses do not directly measure collagen levels, biological skin age,
or clinically established collagen degradation. The backend must never
label an educational score as a clinical measurement or diagnosis.

------------------------------------------------------------------------

## 2. Technology Stack

-   Node.js.
-   Express.js.
-   JavaScript (or TypeScript if selected consistently).
-   PostgreSQL database.
-   Prisma ORM.
-   bcrypt for password hashing.
-   Secure cookie-based sessions or a carefully implemented JWT
    approach.
-   Request validation library.
-   Rate limiting middleware.
-   Centralized logging and error handling.
-   Automated unit and integration tests.

Choose compatible stable versions and document them.

------------------------------------------------------------------------

## 3. Backend Architecture

Suggested folder structure:

``` text
backend/
  src/
    config/
      database.js
      environment.js
    controllers/
      authController.js
      assessmentController.js
      resultController.js
      recommendationController.js
      userController.js
      adminController.js
      reportController.js
    middleware/
      authenticate.js
      authorize.js
      validateRequest.js
      rateLimit.js
      errorHandler.js
    routes/
      authRoutes.js
      assessmentRoutes.js
      resultRoutes.js
      recommendationRoutes.js
      userRoutes.js
      adminRoutes.js
      reportRoutes.js
    services/
      authService.js
      assessmentService.js
      scoringService.js
      recommendationService.js
      reportService.js
    validators/
      authSchemas.js
      assessmentSchemas.js
      profileSchemas.js
      adminSchemas.js
    utils/
      apiResponse.js
      asyncHandler.js
      logger.js
    app.js
    server.js
  prisma/
    schema.prisma
    migrations/
  tests/
  .env.example
  package.json
  README.md
```

Keep HTTP handling in controllers and business logic in services. Keep
database access organized and avoid duplicating scoring logic across
endpoints.

------------------------------------------------------------------------

## 4. API Design Principles

-   Use REST conventions.
-   Use consistent JSON response structures.
-   Validate all request bodies, route parameters, and query parameters.
-   Return appropriate HTTP status codes.
-   Never trust user-supplied user IDs for ownership decisions.
-   Derive the authenticated user's identity from the verified
    session/token.
-   Apply authorization on the server.
-   Paginate potentially large listings.
-   Avoid exposing stack traces or internal database details to clients.
-   Document request and response schemas.

Example success response:

``` json
{
  "success": true,
  "data": {},
  "message": "Request completed"
}
```

Example error response:

``` json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Please review the submitted information",
    "details": []
  }
}
```

Do not expose sensitive values in error messages.

------------------------------------------------------------------------

## 5. Authentication and Account Services

Implement:

  Method   Endpoint               Purpose
  -------- ---------------------- -----------------------------------
  POST     `/api/auth/register`   Create an account
  POST     `/api/auth/login`      Authenticate a user
  POST     `/api/auth/logout`     End the session
  GET      `/api/auth/me`         Return current authenticated user

### Registration

-   Validate name and email.
-   Normalize email consistently.
-   Enforce unique email.
-   Enforce an appropriate password policy.
-   Hash passwords using bcrypt.
-   Never store plaintext passwords.
-   Return only safe user fields.

### Login

-   Verify credentials securely.
-   Use generic failure messages.
-   Apply rate limiting.
-   Create a secure session or token.
-   Do not reveal whether a particular email is registered.

### Session security

Prefer secure, HTTP-only, same-site cookies for browser sessions where
appropriate. If JWT is selected, define expiration, refresh, revocation,
and storage behavior carefully. Do not place long-lived secrets in
browser local storage.

### Guest access

Guests must be able to complete the educational assessment without
registering. Define whether guest results are transient or saved. If
saved, use a privacy-conscious retention policy and a non-guessable
identifier. Do not let one guest retrieve another guest's assessment.

------------------------------------------------------------------------

## 6. Assessment API and Processing

### Endpoint

`POST /api/assessments`

### Required processing sequence

1.  Validate the request schema.
2.  Verify that question keys and answer values are recognized.
3.  Check required responses.
4.  Load the active scoring-rule configuration.
5.  Pass normalized responses to the scoring service.
6.  Calculate the educational score and factor breakdown.
7.  Generate educational interpretation.
8.  Generate recommendations or invoke the recommendation service.
9.  Persist the assessment and related records only under the defined
    consent and retention rules.
10. Return a structured result.

Use a database transaction when the assessment and its related
responses, factors, and recommendations must be saved together.

### Assessment service

The assessment service should coordinate the workflow but should not
contain duplicated scoring formulas. Use a dedicated scoring service
with unit tests.

------------------------------------------------------------------------

## 7. Educational Scoring Engine

Implement transparent, deterministic, configurable scoring.

### Requirements

-   Scoring rules must be documented.
-   Factor weights must be configurable.
-   Input values must be normalized and validated.
-   Missing answers must be handled explicitly.
-   Scores must stay within defined bounds.
-   Category thresholds must be configurable and documented.
-   Every factor score must have an explanation.
-   The calculation must be reproducible from the same inputs and rule
    version.
-   Store or reference the scoring-rule version used for each saved
    assessment.

Do not invent clinically validated collagen measurements or present a
questionnaire score as a measured collagen percentage, biological age,
diagnosis, or validated medical risk.

Use a clearly named educational score. Use neutral result categories,
and explain that they are educational groupings rather than medical
diagnoses.

### Suggested result structure

``` json
{
  "assessmentId": "opaque-assessment-id",
  "score": {
    "value": 0,
    "label": "Educational assessment score",
    "category": "Configured educational category",
    "explanation": "Explanation based on the configured rules"
  },
  "factors": [
    {
      "key": "sun_protection",
      "label": "Sun protection habits",
      "score": 0,
      "explanation": "Explanation based on submitted responses"
    }
  ],
  "limitations": [
    "This is an educational assessment, not a medical diagnosis.",
    "The questionnaire does not measure actual collagen levels."
  ]
}
```

The example values above are schema placeholders only. Do not use them
as real result data.

------------------------------------------------------------------------

## 8. Recommendation Service

Create a recommendation service that maps validated response patterns
and factor results to approved educational content.

Topics may include:

-   Sun protection.
-   Balanced nutrition.
-   Hydration.
-   Healthy sleep habits.
-   Physical activity.
-   General skincare education.

Requirements:

-   Use reviewed content rather than generating unsupported medical
    advice.
-   Explain why a recommendation is shown.
-   Keep advice general and non-prescriptive.
-   Do not prescribe medication or supplements.
-   Do not recommend restrictive diets.
-   Do not promise that a habit will increase collagen or reverse aging.
-   Avoid stigmatizing language.
-   Return recommendations in a predictable schema.

------------------------------------------------------------------------

## 9. Results and History APIs

  --------------------------------------------------------------------------------------
  Method                  Endpoint                               Purpose
  ----------------------- -------------------------------------- -----------------------
  GET                     `/api/results/:assessmentId`           Retrieve an assessment
                                                                 result

  GET                     `/api/assessments`                     List current user's
                                                                 saved assessments

  GET                     `/api/assessments/:id`                 Retrieve one assessment

  DELETE                  `/api/assessments/:id`                 Delete an authorized
                                                                 assessment

  GET                     `/api/recommendations/:assessmentId`   Retrieve
                                                                 recommendations
  --------------------------------------------------------------------------------------

### Authorization rules

-   A registered user may retrieve only their own records.
-   An administrator may access records only under explicitly defined
    permissions and privacy policy.
-   Guest records must use a secure access mechanism.
-   Do not rely on an opaque ID alone as the only authorization control
    for registered-user records.
-   Use pagination and sensible page-size limits.

------------------------------------------------------------------------

## 10. Profile and Account APIs

  -----------------------------------------------------------------------
  Method                  Endpoint                Purpose
  ----------------------- ----------------------- -----------------------
  GET                     `/api/users/profile`    Get current user's
                                                  profile

  PUT                     `/api/users/profile`    Update allowed profile
                                                  fields

  DELETE                  `/api/users/account`    Delete account and
                                                  handle related records
  -----------------------------------------------------------------------

Account deletion must follow a documented data-deletion policy. Define
whether related assessments are deleted or anonymized. Do not leave
orphaned personal data unintentionally.

------------------------------------------------------------------------

## 11. Report Generation

Implement a report endpoint, for example:

`GET /api/reports/assessments/:assessmentId.pdf`

The service should:

-   Verify access to the assessment.
-   Retrieve the saved result and factor breakdown.
-   Generate a properly formatted PDF.
-   Include project title, assessment date, educational score,
    explanation, factor breakdown, charts if supported, general
    recommendations, and limitations.
-   Use readable typography and sensible page margins.
-   Avoid clipping and broken page breaks.
-   Return the correct PDF content type and safe filename.
-   Avoid caching private reports publicly.

The PDF must be a report document, not a screenshot of the application.

------------------------------------------------------------------------

## 12. Administration APIs

  Method   Endpoint                     Purpose
  -------- ---------------------------- -------------------------------------
  GET      `/api/admin/dashboard`       Administrative overview
  GET      `/api/admin/users`           Paginated user listing
  GET      `/api/admin/assessments`     Paginated assessment listing
  PUT      `/api/admin/scoring-rules`   Update scoring configuration
  GET      `/api/admin/scoring-rules`   Retrieve current configuration
  CRUD     `/api/admin/content`         Manage approved educational content

All admin routes must use server-side role authorization.

Record important administrative changes, especially scoring-rule
changes, with administrator identity and timestamps. Validate
configurations before activation. Never expose password hashes or
unnecessary personal data in admin responses.

------------------------------------------------------------------------

## 13. Database Integration

Use Prisma ORM with PostgreSQL.

The backend must:

-   Use a single documented database schema.
-   Use migrations for changes.
-   Validate data before persistence.
-   Use transactions for multi-record assessment saves.
-   Add indexes for common lookups.
-   Handle database errors safely.
-   Avoid logging credentials or sensitive assessment data.
-   Support a documented development seed process that is clearly
    separate from production data.

See the separate **Database Development Specification** for the full
schema and database requirements.

------------------------------------------------------------------------

## 14. Security Requirements

Implement:

-   bcrypt password hashing.
-   Secure session or JWT handling.
-   Server-side authorization.
-   Request validation.
-   Input sanitization where appropriate.
-   Rate limiting, especially on authentication and public submission
    endpoints.
-   CORS allowlist configuration.
-   HTTPS in production.
-   Secure headers.
-   CSRF protection when using cookie-based sessions.
-   Protection against SQL injection through safe ORM usage and
    parameterized queries.
-   Safe file/report handling.
-   Environment variable management.
-   Minimal and privacy-conscious logging.
-   Account and assessment deletion behavior.
-   Data retention rules.

Never commit `.env` files or real credentials.

------------------------------------------------------------------------

## 15. Environment Configuration

Provide `.env.example` with placeholders such as:

``` env
NODE_ENV=development
PORT=5000
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE
FRONTEND_ORIGIN=http://localhost:5173
SESSION_SECRET=replace-with-a-long-random-secret
```

Document which variables are required. Validate environment
configuration at startup and fail clearly when required configuration is
missing. Do not use these placeholder values in production.

------------------------------------------------------------------------

## 16. Testing Requirements

### Unit tests

-   Scoring calculations.
-   Factor normalization.
-   Category assignment.
-   Missing-answer handling.
-   Recommendation mapping.
-   Input validation.

### Integration tests

-   Registration and login.
-   Logout and session behavior.
-   Assessment submission.
-   Assessment retrieval.
-   Ownership checks.
-   Recommendation retrieval.
-   Profile updates.
-   Account deletion.
-   Report generation.
-   Admin authorization.

### Security tests

-   Invalid authentication.
-   Unauthorized record access.
-   Role escalation attempts.
-   Malformed request payloads.
-   Rate-limit behavior.
-   Session expiration.
-   Guest assessment isolation.

Use a test database. Never run destructive tests against production
data.

------------------------------------------------------------------------

## 17. Deployment Requirements

Prepare for deployment on a suitable Node.js host such as Render or
Railway.

-   Configure production environment variables.
-   Connect to managed PostgreSQL.
-   Run Prisma migrations as part of a controlled release process.
-   Configure CORS for the real frontend origin.
-   Enable HTTPS.
-   Add a health-check endpoint such as `GET /health`.
-   Configure structured logs.
-   Avoid exposing internal errors.
-   Verify database connectivity and critical API workflows after
    deployment.

Provide local development and production deployment instructions.

------------------------------------------------------------------------

## 18. Backend Acceptance Checklist

-   [ ] Express server starts correctly.
-   [ ] Environment configuration is validated.
-   [ ] PostgreSQL connection works.
-   [ ] Prisma migrations apply successfully.
-   [ ] Registration and login work.
-   [ ] Passwords are securely hashed.
-   [ ] Guest assessments work according to policy.
-   [ ] Assessment input is validated.
-   [ ] Scoring runs only on the backend.
-   [ ] Scoring rules are configurable and versioned.
-   [ ] Results are reproducible and explained.
-   [ ] Recommendations use approved educational content.
-   [ ] Users can access only their own records.
-   [ ] Account deletion follows policy.
-   [ ] PDF reports are generated securely.
-   [ ] Admin routes are protected.
-   [ ] Rate limiting and error handling work.
-   [ ] Automated tests pass.
-   [ ] No secrets are committed.
-   [ ] API documentation is complete.

------------------------------------------------------------------------

## 19. Final Backend Instruction

Build the complete backend and connect it to the PostgreSQL database.
Implement real validation, authentication, authorization, assessment
processing, result retrieval, recommendations, report generation, and
administration. Do not return hardcoded success responses or fabricated
results. Test the APIs and access controls before declaring the backend
complete.
