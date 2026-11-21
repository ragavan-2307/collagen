# COLLAGEN AGE ANALYZER --- DATABASE DEVELOPMENT SPECIFICATION

**Project:** Understanding Collagen Changes in Aging Skin\
**Document scope:** Database only\
**Purpose:** Define the PostgreSQL relational database, Prisma schema,
relationships, constraints, indexes, migrations, privacy behavior, and
database testing.

------------------------------------------------------------------------

## 1. Database Objective

Design a reliable, secure, maintainable PostgreSQL database for the
Collagen Age Analyzer.

The database may store:

-   Optional user accounts.
-   User assessment records.
-   Questionnaire responses.
-   Educational factor scores.
-   Educational recommendations.
-   Versioned scoring configurations.
-   Consent records.
-   Approved educational content.
-   Administrative audit events.

Collect and retain only the information necessary for the application.

**Scientific limitation:** Stored scores are educational
questionnaire-based indicators. They are not measured collagen levels,
biological skin age, medical diagnoses, or clinically validated
measurements.

------------------------------------------------------------------------

## 2. Database Technology

-   PostgreSQL.
-   Prisma ORM.
-   Prisma schema and migrations.
-   Foreign keys and relational constraints.
-   Transactions for multi-table assessment writes.
-   Indexes for common query patterns.

Use compatible stable versions and document the database version and
Prisma version.

------------------------------------------------------------------------

## 3. Entity Relationship Overview

Recommended relationships:

``` text
User
  | 1
  |----< Assessment (0..many)
              |
              | 1
              |----< AssessmentResponse (1..many)
              |
              | 1
              |----< AssessmentFactor (0..many)
              |
              | 1
              |----< AssessmentRecommendation (0..many)

User
  | 1
  |----< ConsentRecord (0..many)

ScoringRuleVersion
  | 1
  |----< ScoringRule (1..many)

User (administrator)
  | 1
  |----< AdminAuditLog (0..many)

EducationalContent
  | independent approved content records
```

A guest assessment may have a null `user_id`, but guest access and
retention must be handled securely by the application. Do not make guest
records publicly retrievable.

------------------------------------------------------------------------

## 4. Tables and Fields

### 4.1 Users

Purpose: Optional registered accounts.

  Field           Suggested type   Constraints
  --------------- ---------------- --------------------------------------
  id              UUID             Primary key
  name            VARCHAR(120)     Required
  email           VARCHAR(255)     Required, unique
  password_hash   TEXT             Required for password-based accounts
  role            VARCHAR/enum     Default `USER`
  created_at      TIMESTAMPTZ      Required
  updated_at      TIMESTAMPTZ      Required

Requirements:

-   Store only password hashes, never plaintext passwords.
-   Normalize email consistently before enforcing uniqueness.
-   Never return `password_hash` from public APIs.
-   Use explicit roles such as `USER` and `ADMIN`.
-   Consider account deletion and data-retention rules.

### 4.2 Assessments

Purpose: One completed educational assessment.

  -----------------------------------------------------------------------
  Field                   Suggested type          Constraints
  ----------------------- ----------------------- -----------------------
  id                      UUID                    Primary key

  user_id                 UUID                    Nullable foreign key to
                                                  Users

  age_range               VARCHAR(40)             Optional, validated

  educational_score       DECIMAL                 Nullable until
                                                  calculated; bounded by
                                                  application rules

  result_category         VARCHAR(80)             Educational category
                                                  only

  scoring_version_id      UUID                    Foreign key to
                                                  scoring-rule version

  created_at              TIMESTAMPTZ             Required

  updated_at              TIMESTAMPTZ             Required
  -----------------------------------------------------------------------

Requirements:

-   Do not name the score `collagen_percentage` or `biological_age`.
-   Store the scoring configuration version used for reproducibility.
-   Define guest record retention and deletion.
-   Ensure registered users can access only their own assessments.

### 4.3 AssessmentResponses

Purpose: Store normalized answers for an assessment when storage is
permitted.

  Field            Suggested type   Constraints
  ---------------- ---------------- ----------------------
  id               UUID             Primary key
  assessment_id    UUID             Required foreign key
  question_key     VARCHAR(100)     Required
  response_value   JSONB or TEXT    Required, validated
  created_at       TIMESTAMPTZ      Required

Constraints:

-   Add a unique constraint on `(assessment_id, question_key)` if each
    question can have only one response.
-   Validate question keys against the application's questionnaire
    definition.
-   Avoid storing unnecessary free-text personal information.
-   Define whether responses are retained, deleted, or anonymized with
    the assessment.

### 4.4 AssessmentFactors

Purpose: Store the educational factor breakdown returned to the user.

  Field           Suggested type   Constraints
  --------------- ---------------- -----------------------------
  id              UUID             Primary key
  assessment_id   UUID             Required foreign key
  factor_name     VARCHAR(100)     Required
  factor_score    DECIMAL          Bounded by documented rules
  explanation     TEXT             Required
  created_at      TIMESTAMPTZ      Required

Add a unique constraint on `(assessment_id, factor_name)` if there is
one factor result per assessment.

### 4.5 AssessmentRecommendations

Purpose: Store recommendations associated with a completed assessment,
if the application needs a historical snapshot.

  Field                 Suggested type   Constraints
  --------------------- ---------------- ----------------------
  id                    UUID             Primary key
  assessment_id         UUID             Required foreign key
  category              VARCHAR(100)     Required
  recommendation_text   TEXT             Required
  content_version       VARCHAR(40)      Optional
  created_at            TIMESTAMPTZ      Required

Use reviewed educational content. Avoid storing generated medical
advice.

### 4.6 ScoringRuleVersions

Purpose: Preserve scoring configuration versions for reproducibility.

  Field          Suggested type   Constraints
  -------------- ---------------- -------------------------------
  id             UUID             Primary key
  version_name   VARCHAR(80)      Required, unique
  description    TEXT             Optional
  status         VARCHAR/enum     Draft, Active, Retired
  created_by     UUID             Optional foreign key to Users
  created_at     TIMESTAMPTZ      Required
  activated_at   TIMESTAMPTZ      Optional

Only one version should be active at a time unless the product
explicitly supports multiple assessment versions.

### 4.7 ScoringRules

Purpose: Store individual configurable educational scoring rules.

  Field                Suggested type   Constraints
  -------------------- ---------------- ---------------------------------------------
  id                   UUID             Primary key
  version_id           UUID             Required foreign key to ScoringRuleVersions
  factor_name          VARCHAR(100)     Required
  weight               DECIMAL          Validated range
  rule_configuration   JSONB            Required, schema-validated
  is_active            BOOLEAN          Default true
  created_at           TIMESTAMPTZ      Required
  updated_at           TIMESTAMPTZ      Required

Requirements:

-   Validate rule configuration before activation.
-   Document the meaning of every weight.
-   Preserve old versions rather than silently changing rules used by
    past assessments.
-   Use a uniqueness constraint on `(version_id, factor_name)` where
    appropriate.
-   Do not claim rules are clinically validated without evidence.

### 4.8 ConsentRecords

Purpose: Record relevant user choices about storage and privacy.

  Field            Suggested type   Constraints
  ---------------- ---------------- -------------------------------
  id               UUID             Primary key
  user_id          UUID             Nullable foreign key to Users
  consent_type     VARCHAR(80)      Required
  consent_status   BOOLEAN          Required
  policy_version   VARCHAR(40)      Optional
  created_at       TIMESTAMPTZ      Required

Requirements:

-   Record only meaningful consent events.
-   Keep the policy or notice version where appropriate.
-   Define how guest consent is associated with a guest assessment.
-   Do not treat consent to one purpose as consent to unrelated
    purposes.

### 4.9 EducationalContent

Purpose: Store reviewed content for science explanations and
recommendations.

  Field             Suggested type   Constraints
  ----------------- ---------------- -------------------------------
  id                UUID             Primary key
  title             VARCHAR(200)     Required
  content           TEXT             Required
  category          VARCHAR(100)     Required
  status            VARCHAR/enum     Draft, Published, Archived
  content_version   VARCHAR(40)      Optional
  updated_by        UUID             Optional foreign key to Users
  created_at        TIMESTAMPTZ      Required
  updated_at        TIMESTAMPTZ      Required

Only published, approved content should be available to ordinary users.

### 4.10 AdminAuditLogs

Purpose: Record important administrative actions.

  Field           Suggested type   Constraints
  --------------- ---------------- -------------------------------
  id              UUID             Primary key
  admin_user_id   UUID             Required foreign key to Users
  action          VARCHAR(120)     Required
  target_type     VARCHAR(80)      Optional
  target_id       VARCHAR(120)     Optional
  metadata        JSONB            Optional, no secrets
  created_at      TIMESTAMPTZ      Required

Do not place passwords, session tokens, or unnecessary personal data in
audit metadata.

------------------------------------------------------------------------

## 5. Prisma Schema Requirements

Create `prisma/schema.prisma` with:

-   PostgreSQL datasource.
-   Prisma Client generator.
-   UUID primary keys.
-   Appropriate enums for roles and statuses.
-   Relations and referential actions.
-   Unique constraints.
-   Indexes.
-   Date/time fields.
-   Decimal types for scores and weights.
-   JSON fields for validated configuration and response values.

Suggested model names:

-   `User`
-   `Assessment`
-   `AssessmentResponse`
-   `AssessmentFactor`
-   `AssessmentRecommendation`
-   `ScoringRuleVersion`
-   `ScoringRule`
-   `ConsentRecord`
-   `EducationalContent`
-   `AdminAuditLog`

Use consistent naming conventions. Ensure Prisma relation definitions
match the intended deletion and retention policy.

------------------------------------------------------------------------

## 6. Referential Integrity and Deletion Policy

Define explicit behavior for every foreign key.

Examples to decide and document:

-   When a user deletes an account, should their assessments be deleted
    or anonymized?
-   When an assessment is deleted, should its responses, factors, and
    recommendations be deleted through cascading relations?
-   Should scoring-rule versions remain available for historical
    reproducibility?
-   Should audit records be retained according to an administrative
    policy?
-   How are guest records expired?

Do not rely on accidental database defaults. Set explicit referential
actions and test them.

------------------------------------------------------------------------

## 7. Indexing and Query Performance

Add indexes for common queries, such as:

-   Unique user email.
-   Assessments by `user_id` and `created_at`.
-   Assessment responses by `assessment_id`.
-   Factors by `assessment_id`.
-   Recommendations by `assessment_id`.
-   Scoring rules by `version_id` and `factor_name`.
-   Consent records by `user_id` and `created_at`.
-   Audit logs by `admin_user_id` and `created_at`.

Avoid adding indexes without a query need. Review query plans if the
application grows.

------------------------------------------------------------------------

## 8. Data Validation

Enforce validation at both application and database levels where
practical.

Examples:

-   Email format and uniqueness.
-   Allowed role values.
-   Valid age-range options.
-   Recognized questionnaire keys.
-   Valid response values.
-   Score bounds.
-   Valid scoring weights.
-   Valid scoring configuration structure.
-   Allowed status transitions.
-   Required foreign keys.

Do not assume frontend validation is sufficient.

------------------------------------------------------------------------

## 9. Privacy and Data Minimization

-   Store only necessary information.
-   Avoid collecting full birth dates if an age range is sufficient.
-   Avoid unnecessary sensitive personal information.
-   Clearly document which data is saved and why.
-   Provide guest assessment behavior.
-   Define retention periods.
-   Support account deletion.
-   Support assessment deletion where appropriate.
-   Avoid storing raw IP addresses unless there is a documented security
    need and retention policy.
-   Never store passwords in plaintext.
-   Never store session secrets in database content fields.
-   Do not log private questionnaire responses unnecessarily.

------------------------------------------------------------------------

## 10. Migrations and Seed Data

Use Prisma migrations for all schema changes.

Expected commands should be documented, for example:

``` bash
npx prisma generate
npx prisma migrate dev
npx prisma migrate deploy
npx prisma studio
```

Create optional development seed data only for local development and
automated tests.

Important:

-   Clearly mark seed records as test data.
-   Never present seed data as real user activity.
-   Never seed default production administrator credentials.
-   Use a secure, documented process to create the first administrator.
-   Do not run destructive database reset commands against production.

------------------------------------------------------------------------

## 11. Database Backup and Recovery

Document:

-   Backup frequency.
-   Backup retention.
-   Restore procedure.
-   Migration rollback or forward-fix strategy.
-   Access restrictions for backups.
-   Encryption and secure storage expectations.

Test restoration in a non-production environment. Do not claim backups
exist unless configured and verified.

------------------------------------------------------------------------

## 12. Database Testing

Test:

-   User creation and unique email enforcement.
-   Assessment creation.
-   Guest assessment behavior.
-   Response relationships.
-   Factor and recommendation relationships.
-   Scoring-version references.
-   Invalid foreign keys.
-   Unique constraints.
-   Score and weight validation.
-   Assessment deletion behavior.
-   Account deletion/anonymization behavior.
-   Consent record creation.
-   Admin audit record creation.
-   Migration from a clean database.
-   Migration on an existing test database.

Use an isolated test database. Never run destructive tests against
production.

------------------------------------------------------------------------

## 13. Example Assessment Data Flow

1.  Frontend submits questionnaire responses to the backend.
2.  Backend validates the request.
3.  Backend loads the active scoring-rule version.
4.  Backend calculates the educational score and factor breakdown.
5.  Backend starts a database transaction if the result is to be
    persisted.
6.  Backend creates the assessment record.
7.  Backend stores normalized responses when permitted.
8.  Backend stores factor results and recommendation snapshots if
    required.
9.  Backend commits the transaction.
10. Backend returns the result to the frontend.

If validation or persistence fails, return a safe error and do not leave
partial related records.

------------------------------------------------------------------------

## 14. Environment Configuration

Provide a `.env.example` for backend database configuration:

``` env
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE
```

Use a separate database for development, testing, and production. Keep
real credentials out of source control and frontend bundles.

------------------------------------------------------------------------

## 15. Database Acceptance Checklist

-   [ ] PostgreSQL database is created.
-   [ ] Prisma connects successfully.
-   [ ] Schema models are complete.
-   [ ] Primary keys and foreign keys are correct.
-   [ ] Relations match the application requirements.
-   [ ] Unique constraints are applied.
-   [ ] Useful indexes are created.
-   [ ] Scoring-rule versions are preserved.
-   [ ] Assessment data can be stored and retrieved.
-   [ ] Guest assessment policy is implemented.
-   [ ] User ownership is enforced by backend queries.
-   [ ] Deletion and retention behavior is explicit.
-   [ ] Consent records are handled correctly.
-   [ ] Migrations work from a clean database.
-   [ ] Test data is clearly separated from production.
-   [ ] Backups and restore instructions are documented.
-   [ ] No plaintext passwords or secrets are stored.
-   [ ] Database tests pass.

------------------------------------------------------------------------

## 16. Final Database Instruction

Build the complete PostgreSQL schema and Prisma models. Implement
relationships, constraints, indexes, migrations, and a documented
retention/deletion policy. Verify the schema with automated tests and
confirm that the backend can persist and retrieve assessment data
securely. Do not use an in-memory database or fake database responses as
a substitute for PostgreSQL.
