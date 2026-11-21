# UNIVERSAL UI/UX & PRODUCT DESIGN SKILL

## Purpose

This skill is a reusable design and product-quality rulebook for any application, regardless of industry.

The agent must think like a senior product designer, UX designer, UI designer, visual designer, and usability-focused product engineer—not like an AI generating a generic dashboard.

The goal is to produce interfaces that are:
- visually intentional
- simple to understand
- efficient to use
- consistent
- accessible
- appropriate to the product and industry
- free from unnecessary AI-generated content
- polished enough that the design does not feel like a template

## Core Principle

> Build for the user's task, not for the AI's ability to describe the task.

Never add information, labels, cards, explanations, statistics, controls, animations, or decoration merely because they are technically possible or because they make the interface look fuller.

If the user does not need it to understand, decide, act, or recover from an action, question whether it belongs in the primary UI.

---

## 1. Analyze Before Designing

Before changing a screen:

1. Understand the product purpose.
2. Identify the actual user.
3. Identify the user's main tasks.
4. Identify existing functionality and dependencies.
5. Identify information that must remain available even if it is hidden from the UI.
6. Identify the current information hierarchy.
7. Identify unnecessary UI.
8. Identify existing patterns that should be preserved.
9. Identify accessibility and responsive requirements.
10. Only then propose or implement visual changes.

Do not redesign blindly.

### Preserve functionality

Hiding a UI element does not mean deleting the underlying functionality.

If data or configuration is used by another feature, preserve it unless explicitly instructed to remove the functionality itself.

---

## 2. User-Centered Workflow

Design around the user's real workflow.

Ask:

> What is the user trying to accomplish?

Then minimize the path between intention and completion.

Prefer:
**Task → Required information → Action → Result**

Avoid:
**Explanation → Technical terminology → Multiple decisions → Configuration → Action**

The user should not have to understand how the software works internally.

---

## 3. Information Hierarchy

Every screen needs a clear hierarchy.

Use three broad levels:

### Primary
What the user needs to act on now.

### Secondary
Information supporting the current task.

### Tertiary
Useful but non-competitive information.

Do not give equal visual weight to everything.

The page title, primary task, important data, and primary action should be immediately identifiable.

---

## 4. Gestalt Principles

Use Gestalt principles intentionally:

### Proximity
Keep related information physically close.

### Similarity
Elements with similar purposes should share visual treatment.

### Common Region
Group related information when a container genuinely improves understanding.

Do not put everything into cards by default.

### Continuity
Arrange content so the eye naturally follows the workflow.

### Closure
Use visual structure efficiently without unnecessary borders and containers.

The goal is intuitive grouping without explanatory paragraphs.

---

## 5. Cognitive Load

Reduce unnecessary mental effort.

Ask:
- Can a decision be removed?
- Can a field be pre-filled?
- Can the system infer something safely?
- Can several choices be reduced to one clear action?
- Can technical terminology be replaced with user language?
- Can irrelevant information be hidden until needed?

Do not make the user interpret the application.

---

## 6. Progressive Disclosure

Show information when it becomes relevant.

Do not expose every possible option simultaneously.

Use:
- conditional fields
- expandable sections
- contextual controls
- sensible defaults
- focused workflows

Do not hide information that is essential to the current task.

---

## 7. Hick's Law

More choices generally increase decision time.

Avoid unnecessary buttons with overlapping meanings such as:
- Create
- Generate
- Create Document
- Generate Document
- Proceed
- Continue

Prefer one clear action when multiple actions provide the same outcome.

---

## 8. Fitts's Law

Important and frequently used actions should be easy to reach and click.

Consider:
- target size
- spacing
- placement
- frequency of use
- importance

Do not make critical actions tiny or difficult to locate.

---

## 9. Typography

Typography must communicate hierarchy.

Define consistent styles for:
- page titles
- section titles
- field labels
- values
- supporting text
- buttons
- status text

Do not make every heading large and bold.

Prioritize readability over decorative typography.

Use a restrained type scale and consistent weights.

---

## 10. Spacing System

Use a consistent spacing rhythm, commonly based on a 4px/8px-derived scale.

Example:
4, 8, 12, 16, 24, 32, 40, 48

These are guidelines, not rigid requirements.

Spacing should communicate:
- grouping
- separation
- hierarchy
- breathing room

Do not confuse intentional white space with wasted space.

---

## 11. Proportion and Golden Ratio

Golden-ratio thinking may be used as a compositional reference for:
- layout proportions
- visual emphasis
- image/text relationships
- primary/secondary regions

Do not force the golden ratio into functional UI.

Usability and content requirements always take priority.

---

## 12. Color Theory

Do not choose colors randomly or because a color is associated with an industry.

Create a semantic color system.

Typical roles:
- primary/brand
- neutral background
- surface
- border
- primary text
- secondary text
- success
- warning
- error
- optional accent

Use color with purpose.

If everything is colorful, nothing communicates priority.

Use restrained color rather than excessive gradients, saturated cards, or decorative color blocks.

The palette should fit the product's personality and audience.

---

## 13. Contrast and Accessibility

Maintain sufficient contrast for:
- body text
- labels
- buttons
- inputs
- borders where necessary
- status indicators
- interactive states

Do not communicate important information through color alone.

Consider:
- keyboard access
- focus states
- readable font sizes
- touch/click target sizes
- disabled states
- error clarity
- responsive layouts

Accessibility is part of quality, not an optional add-on.

---

## 14. Design Tokens and Consistency

Create reusable rules for:
- colors
- typography
- spacing
- radii
- shadows
- borders
- buttons
- inputs
- cards
- tables
- badges
- icons
- modals
- status states

New screens should feel like parts of the same product.

Avoid mixing unrelated visual styles.

---

## 15. Components

Use reusable components rather than creating every UI element independently.

A component should have:
- consistent visual behavior
- predictable interaction
- reusable spacing
- consistent states

Do not create unnecessary component complexity.

---

## 16. White Space

Use white space to separate concepts and improve scanning.

Avoid:
- cramped layouts
- arbitrary giant gaps
- empty areas caused by poor layout
- decorative whitespace with no structural purpose

For every large empty area, ask whether it improves comprehension.

---

## 17. Motion and Transitions

Animation should communicate state or improve orientation.

Useful motion:
- opening/closing
- expanding/collapsing
- state changes
- successful completion
- navigation transitions

Avoid:
- decorative animation everywhere
- slow transitions
- distracting effects
- excessive parallax
- animation that delays task completion

For professional applications, motion should generally feel calm and fast.

---

## 18. Responsive Design

Design for the actual devices users will use.

Consider:
- desktop
- laptop
- tablet
- mobile when relevant

Do not simply shrink the desktop UI.

Reconsider:
- hierarchy
- navigation
- field grouping
- table behavior
- button placement
- content density

---

## 19. Empty, Loading, Error, and Success States

Every important workflow should have intentional states.

### Empty
Explain what the user can do next, without unnecessary marketing text.

### Loading
Show meaningful feedback when an operation takes time.

### Error
Explain what went wrong and what the user can do.

### Success
Confirm completion without excessive celebration.

Do not create fake data merely to avoid an empty state.

---

## 20. AI Fingerprint Removal

Actively look for signs of generic AI-generated UI:

- unnecessary explanatory paragraphs
- generic marketing language
- redundant headings
- fake statistics
- decorative cards
- excessive badges
- excessive gradients
- random icons
- excessive glassmorphism
- oversized typography
- repeated information
- sample records
- unnecessary feature descriptions
- unnecessary disclaimers
- “demo” language in normal user workflows
- technical implementation details exposed to users
- excessive empty states
- too many buttons
- too many sections

Remove or simplify them when they do not serve the user.

---

## 21. Product Language

Use language the user naturally understands.

Prefer:
- Create
- Save
- Edit
- Print
- Confirm
- Cancel
- Search

over unnecessarily technical wording.

Do not explain obvious functionality.

The UI should communicate through clear labels and structure rather than documentation.

---

## 22. Preserve Content and Functionality

Do not rewrite domain-specific content merely to improve the UI.

When existing content is authoritative:
- preserve it
- improve its presentation
- improve the surrounding workflow

Do not invent business rules, medical rules, financial rules, legal rules, or domain-specific facts.

---

## 23. Digital UI vs Physical Documents

If an application generates printable documents, treat the screen and document as two related but different experiences.

The digital interface may contain:
- controls
- navigation
- interactive fields
- buttons

The printed document should not look like a screenshot of the application.

Printed output must be checked for:
- page size
- margins
- hierarchy
- readability
- page breaks
- clipping
- blank space
- print preview
- physical usability

---

## 24. Visual Quality Control

Before declaring a screen complete, inspect it as a designer.

Check:
- Is the hierarchy obvious?
- Are spacing relationships consistent?
- Are labels aligned?
- Are fields comfortable to use?
- Are buttons appropriately sized?
- Is anything unnecessary?
- Is anything missing?
- Does anything compete for attention unnecessarily?
- Does the page feel balanced?
- Does it feel like one product?
- Does it look intentionally designed rather than generated?

Then inspect the actual workflow, not only the static appearance.

---

## 25. Real-User Test

Final test:

> Can the intended user understand the purpose of the screen within a few seconds?

> Can they identify the next action?

> Can they complete the task without unnecessary decisions?

> Does the interface stay out of their way?

If not, redesign.

---

## 26. Do Not Over-Design

Good product design is not the maximum amount of visual decoration.

Avoid designing for the reaction:
> “Look how much design this has.”

Design for:
> “This is easy, clear, professional, and feels right.”

The target is:
**clarity + usability + visual quality + restraint.**

---

## 27. Implementation Rule

When asked to modify an existing application:

1. Inspect the existing UI and functionality.
2. Identify the user's task.
3. Identify dependencies.
4. Preserve working functionality.
5. Remove only what is explicitly unnecessary.
6. Improve hierarchy and workflow.
7. Apply the visual system consistently.
8. Test real interactions.
9. Test responsive behavior.
10. Test printing where applicable.
11. Remove AI-generated clutter.
12. Perform a final visual and functional review.

Do not rewrite the entire application when a focused change is sufficient.

---

## Final Design Standard

The finished product should feel like it was designed intentionally by a strong human product team.

It should not feel:
- generic
- over-explained
- template-driven
- AI-generated
- visually noisy
- unnecessarily futuristic
- unnecessarily complicated

It should feel:
- clear
- calm
- precise
- coherent
- useful
- polished
- appropriate to its industry
- effortless to operate.
