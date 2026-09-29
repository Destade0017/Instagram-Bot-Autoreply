Antigravity Development Rules

1. Purpose

This document defines how development work must be performed on the Peniel AI Instagram Responder project.

The goal is to keep implementation:

* Incremental
* Controlled
* Understandable
* Secure
* Easy to verify
* Consistent with the PRD and roadmap

Do not treat this project as an opportunity for unnecessary refactoring, feature expansion, or architectural experimentation.

⸻

2. Source of Truth

When deciding what to implement, follow this priority order:

1. docs/prd.md
2. docs/roadmap.md
3. Other approved project documentation
4. The current repository implementation
5. The current task prompt

If the task prompt conflicts with the PRD or roadmap, do not silently choose one.

Stop and identify the conflict before making a product-level decision.

⸻

3. Development Philosophy

Build the smallest correct solution first.

Prefer:

* Simple architecture over unnecessary complexity
* Existing project patterns over new patterns
* Small changes over broad refactors
* Reusable modules over duplicated logic
* Explicit validation over assumptions
* Tested behavior over unverified implementation

Do not build future functionality early.

A feature belonging to V2 or V3 should not be implemented during V1 unless explicitly requested.

⸻

4. Task Execution Rules

Every task should follow this sequence:

1. Understand the requested change.
2. Inspect the existing implementation.
3. Identify the relevant files.
4. Check existing patterns and dependencies.
5. Implement only the requested change.
6. Run appropriate validation/tests.
7. Review the resulting changes.
8. Report what changed and whether verification passed.

Do not begin by modifying files without first understanding the existing implementation.

⸻

5. One Task at a Time

Work on one clearly defined task at a time.

Do not combine unrelated work.

For example:

Good:

Create the Prisma database schema for interactions.

Not good:

Build the database, authentication, dashboard, AI system, Instagram integration, and deployment.

If completing a task reveals that another task is required, stop after completing the current scope and report the dependency.

Do not automatically continue into the next phase.

⸻

6. Required Prompt Structure

Development tasks should be interpreted using this structure:

CONTEXT

Explain the current state of the project and why the task is needed.

GOAL

Define exactly what should be achieved.

CONSTRAINTS

Define what must not change or what limitations apply.

FILES

Identify relevant existing files or directories.

Inspect the repository before creating or modifying files.

Do not invent file paths when the repository already contains an appropriate location.

EXPECTED OUTCOME

Describe the observable result after implementation.

VERIFICATION

Explain how the change should be tested or verified.

⸻

7. Scope Control

Only modify what is necessary to complete the current task.

Do not:

* Refactor unrelated code
* Rename unrelated files
* Rewrite working components
* Change the database architecture unnecessarily
* Replace libraries without a requirement
* Add features that were not requested
* Introduce new architectural patterns without justification
* “Clean up” unrelated code during implementation

If an unrelated problem is discovered, report it separately.

⸻

8. Existing Functionality Must Be Preserved

Unless explicitly requested, preserve:

* Existing API behavior
* Existing authentication behavior
* Existing authorization behavior
* Existing database behavior
* Existing business rules
* Existing UI behavior
* Existing environment configuration
* Existing project structure

A new feature must not silently break an existing feature.

⸻

9. Dependencies and Packages

Do not install a new package simply because it is convenient.

Before introducing a dependency:

1. Check whether the project already has a suitable solution.
2. Check whether the functionality can reasonably be implemented with existing dependencies.
3. Determine whether the dependency is actually necessary.
4. Explain the reason for adding it.

Ask before installing a package when the dependency represents a significant architectural or security decision.

Avoid dependency sprawl.

⸻

10. Database Rules

Use Prisma and PostgreSQL according to the approved project architecture.

Before changing the database:

* Inspect the existing Prisma schema.
* Check existing relationships.
* Check existing naming conventions.
* Consider existing data.
* Avoid unnecessary schema changes.

Do not reset, drop, or destroy the database unless explicitly authorized.

Do not use destructive database commands casually.

If a migration could affect existing data, identify the risk before proceeding.

⸻

11. AI Architecture Rules

The AI layer must remain separate from the rest of the application.

Do not scatter AI-provider-specific API calls throughout the application.

Use an internal AI service/adapter so that the provider can be changed later without rewriting the entire system.

The AI must:

* Use approved Peniel information.
* Follow the defined response rules.
* Avoid inventing business information.
* Return structured results where appropriate.
* Clearly indicate uncertainty when it cannot safely answer.

The AI must never be treated as the final authority for sensitive business information.

⸻

12. Knowledge Base Rules

The Peniel Knowledge Base is an important source of truth.

AI responses should be grounded in approved information such as:

* Property information
* Amenities
* Apartment types
* Pricing
* Location
* Booking process
* Check-in/check-out
* Policies
* FAQs
* Promotions
* Contact information

Do not create fictional Peniel information to fill gaps.

If required information is unavailable, the system should escalate or flag the interaction rather than guess.

⸻

13. Instagram / Meta Integration Rules

Meta and Instagram-specific functionality must be isolated from the core application where practical.

Do not spread Meta API calls throughout unrelated modules.

Use a dedicated integration/service layer.

External webhook data must be:

* Validated
* Normalized
* Handled safely
* Logged appropriately

Never trust incoming webhook data blindly.

Meta credentials, access tokens, app secrets, and other sensitive configuration must never be hard-coded or exposed to the client.

Use environment variables or the project’s approved secret-management approach.

⸻

14. Security Rules

Never hard-code:

* API keys
* Access tokens
* App secrets
* Database credentials
* Authentication secrets
* Other private credentials

Never expose server-only credentials to browser/client code.

Validate external input.

Validate API request bodies.

Protect authenticated dashboard routes.

Enforce authorization on the server, not only in the UI.

Do not store unnecessary personal information.

Do not log sensitive credentials or private tokens.

⸻

15. Human Approval and Safety

The system must not guess when a response could create a business problem.

Interactions should be flagged or handed to staff when appropriate, including situations involving:

* Unverified pricing
* Availability
* Discounts or negotiations
* Complaints
* Refunds
* Disputes
* Sensitive issues
* Booking confirmation
* Payment confirmation
* Missing knowledge
* AI uncertainty
* Requests requiring human judgment

Automation should reduce repetitive work, not remove necessary human judgment.

⸻

16. Automatic Responses

Automatic responses should only be sent when the interaction meets the project’s defined safety requirements.

Do not implement automatic publishing simply because the AI generated a response.

The system should be capable of distinguishing between:

* Safe automatic responses
* Responses requiring staff approval
* Responses requiring human escalation
* Failed processing

Approval mode should be properly tested before automatic response functionality is enabled.

⸻

17. Frontend Rules

For frontend tasks:

* Preserve existing backend behavior.
* Do not modify APIs unnecessarily.
* Do not change authentication or authorization logic unless explicitly requested.
* Reuse existing components and styles where practical.
* Avoid introducing unnecessary UI libraries.
* Keep pages and components focused.
* Maintain responsive behavior.

A frontend task should remain a frontend task unless a backend change is genuinely required.

⸻

18. Backend Rules

For backend tasks:

* Keep business logic server-side.
* Validate inputs.
* Handle errors explicitly.
* Keep modules focused.
* Avoid putting business logic directly inside route handlers when it would make the code difficult to maintain.
* Preserve existing API contracts unless the task explicitly requires changing them.

Do not create microservices for functionality that can reasonably live inside the modular monolith.

⸻

19. Architecture Rules

The V1 application should remain a modular monolith.

Do not introduce:

* Microservices
* Message brokers
* Kubernetes
* Complex event-driven infrastructure
* Unnecessary queues
* n8n
* Advanced agent frameworks

unless a documented requirement demonstrates that they are necessary.

Keep the architecture simple enough for one developer to understand and maintain.

⸻

20. Future Features

Do not implement future features early.

V1 does not include:

* Instagram DMs
* WhatsApp integration
* Facebook integration
* TikTok integration
* Automated booking
* Payments
* CRM functionality
* Advanced analytics
* Multi-company SaaS
* Complex AI agents

Future functionality may influence architecture where necessary, but it should not be implemented prematurely.

⸻

21. Error Handling

Errors should be handled intentionally.

The application should:

* Avoid exposing internal errors to users.
* Log useful diagnostic information.
* Preserve enough context to investigate failures.
* Distinguish expected business failures from unexpected technical failures.
* Avoid silently swallowing errors.

External API failures should not crash unrelated application functionality.

⸻

22. Testing and Verification

Every meaningful implementation task must include verification.

Depending on the task, verification may include:

* Type checking
* Linting
* Unit tests
* Integration tests
* API testing
* Database validation
* Manual browser testing
* Webhook testing
* Build verification

Do not claim that something works without actually verifying it.

If a test cannot be run, clearly state that.

⸻

23. Documentation Rules

Do not modify project documentation automatically.

Documentation should only be changed when:

* The current task explicitly requires it, or
* The implementation has created a necessary documented decision and the task allows documentation updates.

Do not rewrite PRD.md, roadmap.md, or this document simply because implementation changed.

If documentation is now inaccurate, report it and wait for the appropriate documentation task when necessary.

⸻

24. When Requirements Are Unclear

Do not invent product requirements.

If a requirement is ambiguous and the decision could affect:

* Architecture
* Database structure
* Security
* User permissions
* AI behavior
* External integrations
* Business rules
* Product scope

stop and ask for clarification.

For minor implementation details, use the existing project conventions where possible.

⸻

25. Before Major Decisions

Ask before making a major decision involving:

* New external services
* New paid services
* Significant package additions
* Major database restructuring
* Authentication architecture changes
* Authorization architecture changes
* New deployment infrastructure
* Architectural changes
* Significant changes to the product scope

Do not make expensive or difficult-to-reverse decisions silently.

⸻

26. Code Quality

Code should be:

* Readable
* Typed
* Modular
* Consistent with the existing project
* Reasonably documented where complexity requires it

Avoid:

* Clever abstractions without a real need
* Excessive comments
* Giant files
* Giant functions
* Duplicate business logic
* Dead code
* Temporary hacks presented as permanent solutions

Prefer straightforward code that another developer can understand quickly.

⸻

27. No Fake Production Behavior

Do not create fake production functionality and present it as complete.

Examples:

* Fake Instagram API responses presented as real integration
* Fake AI responses presented as a working AI service
* Fake database persistence
* Fake webhook processing
* Hard-coded production data

Mocks and test fixtures are acceptable when clearly isolated for testing.

⸻

28. Completion Report

After completing a task, provide a concise report containing:

Changed

Files created or modified.

Implemented

What was actually changed.

Verification

Tests, checks, builds, or manual verification performed.

Issues

Any unresolved issues, limitations, or follow-up work.

Do not claim completion if required verification failed.

⸻

29. Definition of Done

A task is complete only when:

* The requested functionality has been implemented.
* Existing functionality remains intact.
* Relevant validation/tests have been performed.
* No unnecessary files or dependencies were introduced.
* No unrelated features were added.
* Security considerations have been addressed.
* The implementation matches the approved project requirements.
* The result has been reviewed for obvious errors.

⸻

30. Core Principle

Automate the repetitive.
Escalate the important.
Never invent the answer.

Keep the project small, understandable, secure, and useful.

Build what is needed now.

Do not build what might be needed someday.