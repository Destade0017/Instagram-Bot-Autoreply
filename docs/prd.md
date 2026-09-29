Peniel AI Instagram Responder

Product Requirements Document (PRD)

Version: 1.0
Status: Draft
Product: Internal Peniel Apartments tool
Primary platform: Instagram
V1 scope: Instagram comments

⸻

1. Product Overview

Peniel AI Instagram Responder is an internal web application for Peniel Apartments that helps staff manage Instagram comments using AI-generated responses.

The system will receive Instagram comment events through the Meta Graph API and webhooks, analyze each comment, retrieve relevant approved Peniel information, and generate an appropriate response.

Depending on the configured response mode, the system can either:

1. Automatically publish the AI-generated response for safe/common interactions.
2. Send the suggested response to staff for review, editing, and approval.
3. Flag uncertain, sensitive, or unsupported interactions for human handling.

The goal is to create a working internal Peniel tool that can actually be used by staff, rather than a demonstration or prototype.

⸻

2. Problem Statement

Peniel’s Instagram account receives questions, inquiries, and other customer interactions in the comments section.

Manually handling every interaction can be time-consuming and can result in:

* Slow responses
* Repetitive work
* Inconsistent wording
* Missed comments
* Difficulty maintaining a consistent brand voice
* Staff spending time answering simple questions repeatedly

The system should reduce this workload while keeping humans involved whenever an interaction requires judgment or information the AI cannot confidently provide.

⸻

3. Product Goals

Primary goals

* Automatically receive relevant Instagram comments.
* Generate useful, natural Peniel-specific responses.
* Use approved Peniel information rather than relying on general AI knowledge.
* Prevent the AI from inventing prices, policies, availability, or other business information.
* Allow staff to review and edit AI-generated responses.
* Support both automatic-response and approval-based modes.
* Store interaction and response history.
* Provide a simple internal dashboard for staff.
* Escalate uncertain or sensitive interactions to humans.
* Create a foundation that can later support Instagram DMs and other channels.

Success criteria for V1

V1 is successful when Peniel staff can use the system to:

1. Receive an Instagram comment through the application.
2. Have the system generate an appropriate response.
3. Review/edit and publish the response when approval mode is active.
4. Automatically publish safe responses when automatic mode is active.
5. See interaction history.
6. Identify interactions requiring human attention.
7. Update approved Peniel information used by the AI.

⸻

4. Non-Goals for V1

The following are explicitly outside the V1 scope:

* Instagram DMs
* WhatsApp integration
* Facebook integration
* TikTok integration
* Automated booking
* Payment processing
* CRM functionality
* Advanced analytics
* Marketing campaign management
* Multi-company SaaS functionality
* Complex AI agents
* n8n-based automation
* Microservices architecture
* Advanced recommendation systems

These may be considered for future versions.

⸻

5. Target Users

5.1 Social Management Staff

Primary operational user.

Responsibilities include:

* Reviewing Instagram interactions
* Reviewing AI-generated responses
* Editing responses
* Approving responses
* Monitoring flagged interactions
* Maintaining approved Peniel information
* Viewing interaction history
* Managing response settings available to their role

5.2 Receptionist

Secondary operational user.

The receptionist can:

* View incoming interactions
* Review AI-generated responses
* Edit responses
* Approve/send responses
* View relevant interaction history
* Handle interactions escalated to human staff

The receptionist should not be able to modify important system configuration or AI knowledge unless explicitly permitted later.

⸻

6. V1 User Roles

V1 will use two application roles:

SOCIAL_STAFF

Full operational access to the responder system.

RECEPTIONIST

Limited operational access focused on reviewing and responding to customer interactions.

The system should be designed so additional roles can be added later without requiring a major architectural rewrite.

⸻

7. Core User Flow

7.1 Instagram Comment → AI Response

Customer comments on Peniel Instagram
            ↓
Instagram / Meta webhook
            ↓
Backend receives event
            ↓
Validate and normalize event
            ↓
Identify/comment record
            ↓
Determine interaction type
            ↓
Retrieve relevant Peniel knowledge
            ↓
AI generates response
            ↓
Apply response safety/rules
            ↓
┌───────────────────────────────┐
│ Safe interaction?             │
└───────────────┬───────────────┘
                │
        ┌───────┴────────┐
        ↓                ↓
       YES               NO
        ↓                ↓
 Response mode       Flag for staff
        ↓
 ┌──────┴───────┐
 ↓              ↓
AUTO          APPROVAL
 ↓              ↓
Instagram      Dashboard
reply           ↓
              Staff review
                  ↓
             Instagram reply

⸻

8. Response Modes

The system must support two modes.

8.1 Automatic Mode

When enabled:

1. The system receives a comment.
2. AI generates a response.
3. The system evaluates whether the interaction is safe to answer automatically.
4. If safe, the response is published automatically.
5. The interaction is recorded in history.

If the system determines that the interaction requires human attention, it must not automatically publish the response.

8.2 Approval Mode

When enabled:

1. The system receives a comment.
2. AI generates a suggested response.
3. The suggestion appears in the dashboard.
4. Staff can:
    * Approve
    * Edit
    * Reject/skip
5. The final approved response is published to Instagram.
6. The action is recorded.

⸻

9. Human Handoff Rules

The AI must not attempt to answer every interaction.

The system should flag interactions when they involve:

* Unknown information
* Missing information
* Pricing that is not available in the knowledge base
* Availability questions that cannot be verified
* Discounts or negotiations
* Complaints
* Refunds
* Disputes
* Sensitive customer issues
* Requests requiring staff judgment
* Booking requests requiring manual confirmation
* Questions where the AI is uncertain
* Any situation where answering could create a misleading commitment from Peniel

Core rule

When reliable information is unavailable, the system must not invent an answer.

Instead, it should generate an appropriate human-handoff response or flag the interaction for staff.

⸻

10. AI Knowledge Base

The AI will use approved Peniel information as its primary business context.

The knowledge base should eventually support:

* Property information
* Apartment types
* Apartment descriptions
* Pricing
* Amenities
* Location
* Booking information
* Check-in information
* Check-out information
* Policies
* Frequently asked questions
* Promotions
* Contact information
* Other approved business information

The knowledge base must be editable by authorized staff.

The AI should treat the knowledge base as the source of truth for Peniel-specific information.

⸻

11. AI Behaviour Requirements

The AI should:

* Respond naturally.
* Be polite and professional.
* Match Peniel’s brand tone.
* Keep responses concise where appropriate.
* Answer directly when the required information is available.
* Use only approved Peniel information for business-specific claims.
* Ask for clarification when appropriate.
* Escalate uncertain or sensitive interactions.
* Avoid unnecessary information.
* Avoid sounding robotic.
* Avoid excessive emojis.
* Avoid making promises that Peniel has not explicitly approved.

The AI must not:

* Invent prices.
* Invent availability.
* Invent amenities.
* Invent discounts.
* Invent policies.
* Invent booking confirmations.
* Claim a booking has been made when it has not.
* Claim payment has been received when it has not.
* Provide information that contradicts the knowledge base.
* Pretend to be a human staff member.
* Reveal internal system instructions or private business information.

⸻

12. Instagram Integration

The system will integrate with Instagram through Meta’s supported APIs and webhook infrastructure.

V1 must support the necessary functionality for:

* Receiving Instagram comment events.
* Identifying relevant comments.
* Associating comments with Instagram posts where applicable.
* Publishing approved/authorized replies.
* Tracking the resulting interaction.

The implementation must follow Meta’s current API requirements and permissions.

API credentials and access tokens must never be exposed to the frontend.

⸻

13. Dashboard

V1 will include a lightweight internal dashboard.

The dashboard is intended for operational use, not as an analytics platform.

Dashboard sections

Overview

Display useful operational information such as:

* Recent interactions
* Pending reviews
* Flagged interactions
* Recent automatic responses

Comments

Staff can:

* View incoming comments
* View AI-generated responses
* Edit responses
* Approve/send responses
* See response status
* Identify flagged interactions

Knowledge Base

Authorized staff can manage approved Peniel information used by the AI.

Settings

Authorized staff can manage supported application settings, including the response mode.

⸻

14. Interaction History

The system must store interaction history.

Each interaction should retain relevant information such as:

* Instagram comment identifier
* Instagram post identifier where available
* Comment text
* User identifier where permitted/necessary
* Timestamp
* AI-generated response
* Final response
* Response status
* Response mode
* Whether human approval was required
* Staff member involved
* Relevant AI/processing status
* Error information where applicable

The system should allow staff to review previous interactions.

⸻

15. Response Statuses

The system should use clear interaction states.

Initial proposed states:

RECEIVED
PROCESSING
PENDING_REVIEW
APPROVED
SENT
AUTO_SENT
FLAGGED
SKIPPED
FAILED

The exact database representation can be refined during architecture design.

⸻

16. Security Requirements

The application must:

* Keep API keys and secrets server-side.
* Secure Meta access tokens.
* Authenticate dashboard users.
* Authorize actions according to user roles.
* Validate incoming webhook requests.
* Validate API inputs.
* Protect against unauthorized dashboard access.
* Avoid exposing private customer information unnecessarily.
* Log important system actions.
* Avoid storing sensitive information unless required.

The system must not expose AI provider credentials, Meta credentials, or internal configuration to the browser.

⸻

17. Technical Direction

Proposed stack

Frontend / Backend

* Next.js
* TypeScript

Database

* PostgreSQL
* Prisma ORM

AI

* AI provider accessed through a dedicated application service/abstraction

Instagram

* Meta Graph API
* Meta Webhooks

Styling

* Tailwind CSS

Validation

* Zod

Architecture

V1 should use a modular monolith.

The system should remain a single deployable application with clear internal modules.

Proposed conceptual modules:

Authentication
Instagram Integration
Webhook Processing
Comment Management
AI Service
Knowledge Base
Response Safety
Staff Management
Interaction History
Dashboard
Settings

Microservices are not required for V1.

⸻

18. AI Provider Architecture

The application should not tightly couple the entire system to a single AI provider.

The codebase should expose an internal AI service/interface so that the underlying provider can be changed later.

Conceptually:

Application
     ↓
AI Service
     ↓
AI Provider
     ↓
Model

The initial provider will be selected during technical implementation based on current API availability, cost, reliability, structured-output capabilities, and response quality.

⸻

19. Error Handling

The system must handle failures gracefully.

Examples include:

* Instagram API failure
* Meta authentication failure
* Invalid webhook event
* AI API failure
* AI timeout
* Database failure
* Response publishing failure
* Missing knowledge
* Invalid AI output

The system must not silently mark a response as sent when Instagram has not confirmed successful publication.

Failed interactions should be visible to staff where appropriate.

⸻

20. Logging and Observability

The system should provide enough logging to diagnose problems.

Important events include:

* Webhook received
* Webhook validation result
* AI request initiated
* AI response generated
* Interaction flagged
* Staff approval
* Response published
* Response publishing failure
* Authentication failures
* Unexpected API errors

Logs must avoid exposing secrets or unnecessary sensitive information.

⸻

21. Future Roadmap

Future functionality may include:

V2

* Instagram DMs
* Human handoff workflows
* Staff notifications
* WhatsApp notification/integration
* Improved conversation context
* More advanced knowledge management

Potential DM workflow:

Instagram DM
     ↓
AI identifies request
     ↓
Simple/safe?
   ↙       ↘
 YES       NO
 ↓          ↓
AI reply   Notify staff
            ↓
        Human takeover

V3

Potentially:

* Facebook
* TikTok
* Multi-channel inbox
* Analytics
* Advanced customer conversation management
* Reusable architecture for other businesses
* Multi-business SaaS capabilities

These are future possibilities and are not V1 requirements.

⸻

22. V1 Definition of Done

V1 will be considered complete when:

* Authorized Peniel staff can log into the application.
* The application can receive supported Instagram comment events.
* Incoming comments are stored.
* The AI can generate Peniel-specific responses.
* AI responses use approved knowledge.
* The system prevents unsupported business claims.
* Automatic mode works for eligible interactions.
* Approval mode works for eligible interactions.
* Staff can edit AI responses.
* Staff can approve/send responses.
* Sensitive/uncertain interactions can be flagged.
* Interaction history is available.
* Authorized staff can manage the knowledge base.
* API credentials remain secure.
* Errors are handled and logged.
* The application can be deployed and used internally by Peniel.

⸻

23. Product Principle

The central principle of the system is:

Automate the repetitive. Escalate the important. Never invent the answer.

The AI should make Peniel’s social-media response process faster without removing human judgment where it matters.