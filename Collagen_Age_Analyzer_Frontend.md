# COLLAGEN AGE ANALYZER --- FRONTEND DEVELOPMENT SPECIFICATION

**Project:** Understanding Collagen Changes in Aging Skin\
**Document scope:** Frontend only\
**Purpose:** Build the complete responsive user interface and connect it
to the backend APIs.

------------------------------------------------------------------------

## 1. Frontend Objective

Build a polished, accessible, responsive React application for the
Collagen Age Analyzer. The frontend must let visitors learn about
collagen, complete an educational lifestyle questionnaire, review
results, view charts, read general recommendations, and optionally
manage an account and assessment history.

The frontend must not calculate authoritative assessment results
independently. The backend is the source of truth for scoring, result
categories, and recommendation generation.

**Scientific limitation:** Present the product as an educational
assessment, not a medical diagnostic tool. Never claim that the
questionnaire measures actual collagen levels, biological skin age, or
clinically established collagen degradation.

------------------------------------------------------------------------

## 2. Frontend Technology

-   React
-   JavaScript
-   HTML5 semantic elements
-   CSS3
-   Tailwind CSS (or an equivalent consistent styling system)
-   React Router for navigation
-   Chart.js with a React integration for charts
-   A reusable API client using `fetch` or Axios
-   A suitable PDF viewing/download flow using the backend-generated
    report

Use stable compatible package versions and record them in
`package.json`.

------------------------------------------------------------------------

## 3. Visual Design System

### Design direction

Create a clean scientific and skincare-education aesthetic:

-   White and soft blush-pink surfaces.
-   Dark navy or charcoal typography.
-   Restrained rose/coral accents.
-   Subtle borders and shadows.
-   Scientific illustrations of skin layers, collagen fibers, elastin
    fibers, and fibroblasts.
-   Calm, purposeful transitions.
-   Clear visual hierarchy and generous but intentional spacing.

Avoid excessive gradients, glassmorphism, random icons, unnecessary
cards, fake statistics, decorative dashboards, and generic AI-generated
design patterns.

### Design tokens

Create reusable tokens for:

-   Brand, accent, background, surface, border, and text colors.
-   Success, warning, and error states.
-   Font family, font sizes, line heights, and weights.
-   Spacing based on a consistent 4px/8px rhythm.
-   Border radii and shadows.
-   Button, input, card, modal, table, and chart styles.

### Accessibility

-   Use semantic HTML.
-   Associate labels with every input.
-   Support keyboard navigation.
-   Provide visible focus indicators.
-   Maintain sufficient color contrast.
-   Do not rely on color alone to communicate status.
-   Provide accessible names for icon-only controls.
-   Announce validation errors and important status changes
    appropriately.
-   Respect reduced-motion preferences.

------------------------------------------------------------------------

## 4. Responsive Requirements

Design intentionally for:

-   Desktop.
-   Laptop.
-   Tablet.
-   Mobile.

Mobile must have a simplified navigation pattern, comfortable touch
targets, stacked form layouts, readable charts, and no horizontal
overflow. Tables should adapt to narrow screens through responsive
layouts or controlled horizontal scrolling.

------------------------------------------------------------------------

## 5. Pages and User Journeys

### Page 1 --- Home

Include:

-   Brand/logo area.
-   Navigation: Home, About Collagen, Assessment, Contact.
-   Optional account actions.
-   Hero heading: **"Understand Your Skin. Learn About Collagen."**
-   Supporting description: "Explore how age and everyday lifestyle
    factors are associated with collagen changes through an interactive
    educational assessment."
-   Primary action: **Start Assessment**.
-   Secondary action: **Explore Collagen Science**.
-   Scientific skin illustration.
-   Short introduction to collagen, fibroblasts, elastin, and skin
    aging.
-   Clear educational-tool notice.

### Page 2 --- About Collagen

Present educational sections:

1.  What collagen is.
2.  Types of collagen, including Type I and Type III in skin.
3.  Fibroblast function.
4.  Collagen production and remodeling.
5.  Collagen and elastin differences.
6.  Factors associated with skin aging.

Use readable diagrams, captions, glossary terms, and credible references
supplied or approved for the project. Do not invent scientific
citations.

### Page 3 --- Assessment

Build a multi-step questionnaire with:

-   Age range.
-   Sun exposure habits.
-   Sun-protection habits.
-   Sleep habits.
-   Hydration habits.
-   Dietary patterns.
-   Physical activity.
-   Smoking exposure.
-   General skincare habits.

Collect only information required by the assessment. Use neutral wording
and explain why each category is included.

Questionnaire behavior:

-   Progress indicator.
-   Previous and Next controls.
-   Required and optional field indicators.
-   Inline validation.
-   Preserve answers when navigating backward.
-   Prevent accidental duplicate submission.
-   Review-answers screen.
-   Submit Assessment action.
-   Guest assessment option.
-   Clear consent and data-storage explanation.

Do not request unnecessary sensitive information.

### Page 4 --- Results

Display only results returned by the backend:

-   Educational assessment score.
-   Result category as defined by the backend.
-   Explanation of the score.
-   Lifestyle factor breakdown.
-   Educational interpretation.
-   Assessment date, when available.
-   Important limitations.

Clearly state that the result is based on questionnaire responses and is
not a measured collagen level, diagnosis, or biological skin-age
measurement.

### Page 5 --- Charts

Use Chart.js for relevant visualizations:

-   Bar chart for factor-level results.
-   Doughnut chart only when the displayed segments have a clearly
    defined meaning.
-   Radar chart only if it improves readability.

Charts must use actual API response data. Include titles, labels,
legends where useful, tooltips, responsive sizing, and a text
explanation. Do not show fabricated sample data as real results.

### Page 6 --- Recommendations

Display general educational recommendations returned by the backend.
Group by topic, such as sun protection, nutrition, hydration, sleep,
physical activity, and general skincare.

Each item should show the topic, explanation, and practical healthy
habit. Do not present recommendations as treatment, prescriptions, or
guaranteed collagen outcomes.

### Page 7 --- Account and Profile (optional)

Provide:

-   Registration.
-   Login.
-   Logout.
-   Profile view/edit.
-   Privacy and consent settings.
-   Account deletion.

Guest assessment must remain available without registration.

### Page 8 --- Assessment History (registered users)

Display only the signed-in user's saved assessments:

-   Date.
-   Educational score.
-   Result category.
-   View report action.
-   Delete record action where supported.

Include pagination if the history becomes long. Provide a useful empty
state when no saved assessments exist.

### Page 9 --- Admin UI

Create admin pages only if the backend supports them. Hide admin
navigation from ordinary users, but remember that hiding UI is not
security: backend authorization is mandatory.

------------------------------------------------------------------------

## 6. Reusable Components

Build reusable components with consistent behavior:

-   `AppLayout`
-   `Navbar`
-   `Footer`
-   `HeroSection`
-   `EducationalSection`
-   `ProgressBar`
-   `QuestionStep`
-   `FormField`
-   `ReviewAnswers`
-   `ResultSummary`
-   `FactorBreakdown`
-   `ChartPanel`
-   `RecommendationList`
-   `LoadingState`
-   `ErrorMessage`
-   `EmptyState`
-   `ConfirmationDialog`
-   `ProtectedRoute`
-   `ConsentNotice`

Do not over-engineer components. Reuse components where the behavior and
presentation are genuinely shared.

------------------------------------------------------------------------

## 7. Suggested Frontend Folder Structure

``` text
frontend/
  public/
    images/
    icons/
  src/
    app/
      App.jsx
      routes.jsx
    components/
      layout/
      navigation/
      forms/
      assessment/
      results/
      charts/
      recommendations/
      feedback/
    pages/
      HomePage.jsx
      AboutPage.jsx
      AssessmentPage.jsx
      ReviewPage.jsx
      ResultsPage.jsx
      RecommendationsPage.jsx
      LoginPage.jsx
      RegisterPage.jsx
      ProfilePage.jsx
      HistoryPage.jsx
      AdminPage.jsx
      NotFoundPage.jsx
    services/
      apiClient.js
      authService.js
      assessmentService.js
      resultService.js
      recommendationService.js
      profileService.js
    hooks/
    context/
    utils/
    styles/
      globals.css
      tokens.css
    tests/
    main.jsx
  .env.example
  package.json
  README.md
```

------------------------------------------------------------------------

## 8. API Integration

Keep API calls in service modules rather than scattering requests
throughout components.

Integrate with these backend endpoints:

  Method   Endpoint                               Frontend use
  -------- -------------------------------------- ----------------------
  POST     `/api/auth/register`                   Registration
  POST     `/api/auth/login`                      Login
  POST     `/api/auth/logout`                     Logout
  GET      `/api/auth/me`                         Current session
  POST     `/api/assessments`                     Submit assessment
  GET      `/api/assessments`                     Assessment history
  GET      `/api/assessments/:id`                 View assessment
  DELETE   `/api/assessments/:id`                 Delete assessment
  GET      `/api/results/:assessmentId`           Load results
  GET      `/api/recommendations/:assessmentId`   Load recommendations
  GET      `/api/users/profile`                   Load profile
  PUT      `/api/users/profile`                   Update profile
  DELETE   `/api/users/account`                   Delete account

Use the actual API contract agreed with the backend developer. Do not
assume undocumented response fields.

### API client requirements

-   Configure the backend base URL through an environment variable.
-   Handle network errors and non-success HTTP responses.
-   Handle expired sessions.
-   Prevent duplicate submissions.
-   Show loading states.
-   Never put database credentials, private API keys, or server secrets
    in frontend code.
-   Use secure cookie-based authentication when that is the selected
    backend design.

Example frontend environment variable:

``` env
VITE_API_BASE_URL=http://localhost:5000
```

This is a public API URL, not a secret.

------------------------------------------------------------------------

## 9. State and Form Behavior

-   Keep questionnaire answers in a predictable state model.
-   Validate required fields before advancing.
-   Preserve answers between steps.
-   Clear sensitive transient data when the user exits or resets the
    assessment, as appropriate.
-   Do not store passwords or sensitive assessment responses in browser
    local storage.
-   Treat backend results as authoritative.
-   Show clear retry options after recoverable network errors.
-   Confirm destructive actions such as deleting an assessment or
    account.

------------------------------------------------------------------------

## 10. PDF Report Experience

Use the backend report endpoint or agreed report-generation method.

The frontend must:

-   Request the report for an authorized assessment.
-   Show progress while generating or retrieving it.
-   Handle report-generation errors.
-   Provide a clear download action.
-   Avoid constructing report contents from fabricated values.

The report itself should be a professionally formatted document, not a
screenshot of the UI.

------------------------------------------------------------------------

## 11. Frontend Testing

Test:

-   Navigation and routes.
-   Responsive behavior at common screen widths.
-   Questionnaire progression.
-   Answer preservation.
-   Required-field validation.
-   Review and submission.
-   Loading, empty, success, and error states.
-   API failure handling.
-   Results rendering from actual API data.
-   Chart labels and values.
-   Guest flow.
-   Authenticated flow.
-   Protected page behavior.
-   PDF download.
-   Accessibility and keyboard interaction.

Use unit/component tests and end-to-end tests for the main user journey.

------------------------------------------------------------------------

## 12. Frontend Acceptance Checklist

-   [ ] Home page is complete.
-   [ ] About Collagen page is complete.
-   [ ] Assessment questionnaire works.
-   [ ] Answers persist while moving between steps.
-   [ ] Review screen works.
-   [ ] API submission works.
-   [ ] Results are loaded from the backend.
-   [ ] Charts use actual returned data.
-   [ ] Recommendations are loaded correctly.
-   [ ] Guest assessment is supported.
-   [ ] Authentication screens work when enabled.
-   [ ] History shows only the user's records.
-   [ ] Admin UI is role-aware.
-   [ ] PDF download works.
-   [ ] All visible buttons work.
-   [ ] Mobile and desktop layouts are tested.
-   [ ] Accessibility checks are completed.
-   [ ] No fake statistics or fabricated results are displayed.

------------------------------------------------------------------------

## 13. Final Frontend Instruction

Build the complete frontend, not just a landing-page mockup. Connect
every implemented workflow to the documented backend API. Use a
restrained, professional scientific design. Verify the complete user
journey on desktop and mobile. Do not claim frontend completion until
the questionnaire, API integration, result display, charts, and
supported account workflows have been tested.
