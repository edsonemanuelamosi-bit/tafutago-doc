
TafutaGo — Product Requirements Document (PRD)
Version: 1.0
Date: 25 October 2026
Status: Initial full-stack MVP specification
Target market: Tanzania
Product type: Local discovery and marketplace platform

1. Product Overview
TafutaGo helps people in Tanzania discover nearby products, services, businesses, accommodation, places to visit, and events. Users can choose a region, district, and street or area, or optionally share their location, then search for relevant listings and compare details such as descriptions, prices, photos, location, and contact options.

Business owners and service providers can create and manage listings. Administrators can moderate listings and handle reports. Paid promotions, in-app messaging, subscriptions, and payment features may be added after the core discovery experience is working.

The product must remain useful without AI. If AI is introduced later, it must act as an assistant and not independently make important decisions for users.

2. Problem Statement
Information about local services, products, accommodation, and places is scattered across social media, personal contacts, and separate websites. TafutaGo aims to make local discovery easier by organizing listing information and providing search, location filters, details, contact options, and directions.

3. Goals
Help users discover local products, services, businesses, accommodation, places, and events.
Support search by region, district, and area.
Let listing owners create and update their own listings.
Show prices when supplied by owners; never invent missing prices.
Offer practical contact and directions options.
Give administrators moderation and reporting tools.
Build a secure, maintainable full-stack application.
Keep paid features optional for the first release.
4. User Roles
4.1 Visitor
Can browse and search public, published listings.

4.2 Registered User
Can manage permitted profile information, contact listing owners, and use any later account-based features.

4.3 Listing Owner
Can create and manage only their own listings, upload images, update descriptions and prices, and view publication status.

4.4 Administrator
Can moderate listings, review reports, manage categories, and access administrative functions according to assigned permissions.

Administrator privileges must be enforced on the backend. Users must never be able to grant themselves administrator roles.

5. Required Technology Stack
Use this stack unless a documented technical limitation makes a requirement impossible. Do not silently replace technologies.

Web application: React with TypeScript.
Landing page: Next.js with TypeScript.
Database: Supabase PostgreSQL.
Authentication: Supabase Auth.
File storage: Supabase Storage with secure access policies.
Backend: Supabase-supported backend capabilities and server-side functions where appropriate.
Hosting, caching, and CDN: Cloudflare where suitable.
Version control: Git and GitHub.
Automation: GitHub Actions for appropriate checks and deployment workflows.
Payments: FimiPay, based strictly on its official documentation.
Maps: Start with external map-direction links. Evaluate OpenStreetMap and Leaflet for map display.
Do not assume Google Maps API keys or Google Search API keys are available. Core search must use TafutaGo's own database and must not depend on paid Google Search APIs.

6. Functional Requirements
FR-01 — Landing Page
Explain the purpose of TafutaGo.
Provide a prominent search interface.
Provide region, district, and area selection.
Display listing categories.
Include login, registration, and listing-owner entry points.
Work on mobile, tablet, and desktop.
FR-02 — Authentication
Support registration and login using Supabase Auth.
Support logout and password recovery using supported Supabase functionality.
Validate input and display clear error messages.
Protect private pages and actions.
Never store plain-text passwords in application tables.
FR-03 — Profiles
Allow users to manage basic profile information.
Collect only information needed for platform functionality.
Apply privacy rules to contact and profile fields.
FR-04 — Location Selection
Let users choose a region, district, and street/neighbourhood/area.
Offer optional device location with permission.
Provide manual location selection if GPS is denied or unavailable.
Do not require GPS for ordinary searches.
Validate region/district relationships where structured location data exists.
FR-05 — Search and Filters
Search listings by text.
Filter by category, subcategory, listing type, region, district, and area.
Support price filters when price data is available.
Support sorting by relevance and other implemented criteria.
Use pagination or scalable result loading.
Show a helpful empty state when no results match.
Initially use PostgreSQL/Supabase search over TafutaGo's own listings.
Do not fabricate results, prices, ratings, or availability.
FR-06 — Categories
Support configurable categories including:

Products and goods.
Construction materials.
Fundi and skilled services.
Other professional and local services.
Hotels, guest houses, lodges, and accommodation.
Restaurants and cafes.
Swimming pools and recreation.
Tourist attractions and places to visit.
Events.
Houses or rooms for short stays, subject to suitable listing rules.
Administrators should be able to manage categories without a code change for every addition.

FR-07 — Listing Details
A listing can include:

Title and description.
Category and subcategory.
Listing type.
Price, currency, and price unit where applicable.
Region, district, area, and optional address text.
Optional coordinates.
Photos.
Business/provider name.
Products, services, or facilities.
Availability information where relevant.
Public contact options.
Created and updated timestamps.
Publication/moderation status.
Clearly distinguish owner-provided information from information verified by the platform.

FR-08 — Images
Allow authorized owners to upload listing images.
Validate file type and size.
Store files using configured storage policies.
Restrict image changes to authorized users.
Handle failed uploads safely.
Do not rely on client-side checks alone for access control.
FR-09 — Owner Dashboard
Owners should be able to:

Create, view, and edit their own listings.
Update descriptions, prices, and relevant availability.
Upload or remove their listing images.
See pending, published, rejected, or suspended status where applicable.
See rejection reasons when provided.
View basic engagement statistics if implemented.
Never edit another owner's listings.
FR-10 — Publication and Moderation
New listings may require administrator approval.
Only published listings should appear in public search.
Rejected or suspended listings should not appear as ordinary public results.
Record important moderation actions.
Make the moderation policy configurable.
FR-11 — Contact
Display public contact methods supplied by the listing owner.
Where supported, allow opening a call, SMS, or messaging application.
Design for possible future in-app messaging.
If paid messaging is later implemented, disclose the fee before charging.
Paid chat must not be required for the initial MVP.
FR-12 — Directions and Maps
Offer directions when a usable location exists.
Initially open an external directions link.
Evaluate OpenStreetMap and Leaflet for map display.
Do not claim to provide turn-by-turn navigation unless implemented.
Handle missing or approximate coordinates.
Ask permission before accessing device location.
FR-13 — Reviews and Reports
A review system may be added as an extension.
Validate review ratings and text.
Users must not edit other users' reviews.
Let users report misleading, abusive, fraudulent, or inappropriate listings.
Label user-generated content appropriately.
Do not invent ratings or testimonials.
FR-14 — Promotions and Advertisements
Eligible owners may request paid promotion when this feature is enabled.
Define price, duration, start, and end times clearly.
Label sponsored listings and advertisements.
Track promotion status separately from payment status.
Never mark an unpaid promotion as paid.
Stop paid placement when a promotion expires.
Basic organic discovery should not require payment.
FR-15 — FimiPay Integration
Use only verified information from FimiPay's official documentation.

Before coding the integration:

Review the official API documentation.
Confirm the documented authentication method.
Confirm the payment-creation request and response.
Confirm the documented payment-status verification process.
Confirm webhook delivery and authenticity-verification requirements.
Confirm supported payment methods and transaction states.
Test using test credentials first.
Security requirements:

Keep secret API credentials on the server.
Store credentials in environment variables or the deployment secret manager.
Never commit real credentials to GitHub.
Never expose secret keys in browser code or mobile application bundles.
Verify payment status server-side using documented provider functionality.
Verify webhook authenticity exactly as documented by FimiPay.
Make webhook handling idempotent so repeated notifications cannot grant duplicate benefits.
Store the transaction references needed for reconciliation.
Do not trust a browser redirect or success screen as proof of payment.
Activate paid features only after server-side verification.
Do not invent endpoints, request fields, authentication headers, signatures, or status values.
If the documentation does not explain a required behavior, stop and ask for clarification instead of guessing.
FR-16 — Payment Records and Entitlements
When payment features are implemented:

Give each internal payment a unique identifier.
Associate the payment with the relevant user and order, promotion, or subscription.
Store provider references, amount, and currency.
Separate transaction status from entitlement status.
Prevent duplicate payment processing.
Restrict transaction access to authorized users and administrators.
Grant paid access only after verified payment.
Keep an audit trail for important payment changes.
Never store card security codes.
FR-17 — Notifications
When implemented:

Provide notifications for important account and listing events.
Notify owners of listing approval or rejection.
Send payment-related confirmation only after server-side verification.
Do not expose secrets in notifications.
Protect private notifications from unauthorized access.
FR-18 — Admin Dashboard
Authorized administrators should be able to:

View and manage listings.
Approve or reject listings.
Suspend listings that violate platform rules.
Review reports.
Manage categories.
Review relevant user/account status.
View payment records according to assigned permissions.
Manage promotions when implemented.
Review moderation and administrative audit logs.
Backend authorization is required; hiding a button in the frontend is not security.

FR-19 — Error Handling
Show clear validation errors.
Handle network and server failures gracefully.
Do not expose stack traces or internal secrets to ordinary users.
Log appropriate diagnostics server-side.
Provide retry options for recoverable errors.
Prevent accidental duplicate submissions for sensitive actions.
FR-20 — Accessibility and Responsive Design
Support mobile, tablet, and desktop.
Use readable typography and sufficient contrast.
Label form controls accessibly.
Support keyboard navigation for core functions.
Do not communicate status by colour alone.
Provide loading, empty, error, and success states.
7. Non-Functional Requirements
Security
Enable Supabase Row Level Security (RLS) on tables exposed to client access.
Create policies based on authenticated identity and permissions.
Enforce ownership and role checks on the backend.
Use least-privilege access.
Validate input on both client and server.
Protect administrative functions.
Keep secrets out of source control.
Use HTTPS in production.
Apply safe upload rules.
Restrict private user, report, and payment information.
Do not log passwords, secret keys, or unnecessary personal data.
Reliability and Integrity
Use database constraints where appropriate.
Use transactions for operations that must succeed or fail together.
Prevent duplicate payment entitlements.
Handle concurrent updates safely.
Establish and test backup and recovery procedures before production.
Do not claim backups exist unless the mechanism is configured and verified.
Performance
Index frequently queried columns.
Paginate large result sets.
Optimize uploaded images.
Avoid fetching every listing for each search.
Use caching only where it will not expose private or incorrectly stale data.
Maintainability
Use TypeScript for React and Next.js.
Organize code by responsibility.
Keep database migrations version-controlled.
Document environment variables without revealing secret values.
Use consistent naming and formatting.
Avoid hard-coded credentials.
Testing
Test core business logic.
Test authentication and authorization.
Test listing ownership and RLS policies.
Test search filters and pagination.
Test image access rules.
Test payment verification and duplicate webhook handling when payment is implemented.
Test responsive layouts.
Run available checks before deployment.
8. Suggested Data Model
Create the schema using reviewed, version-controlled database migrations. Adjust details when necessary without weakening security.

profiles
id: UUID matching the authenticated user.
display_name.
Optional public contact fields.
created_at.
updated_at.
user_roles
user_id.
role.
created_at.
Only a trusted administrative process may assign administrator privileges.

regions
id.
name.
Optional code.
districts
id.
region_id.
name.
areas
id.
district_id.
name.
categories
id.
Optional parent_id.
name.
slug.
is_active.
listings
id.
owner_id.
category_id.
title.
description.
listing_type.
Optional price, currency, and price_unit.
region_id.
district_id.
Optional area_id and address_text.
Optional latitude and longitude.
status.
created_at.
updated_at.
listing_images
id.
listing_id.
storage_path.
Optional alt_text.
created_at.
listing_contacts
id.
listing_id.
contact_type.
contact_value.
created_at.
Apply public-display and owner-access rules to contact information.

reports
id.
reporter_id.
Optional listing_id.
reason.
Optional description.
status.
created_at.
Optional reviewed_at.
reviews (optional later extension)
id.
listing_id.
reviewer_id.
rating.
Optional comment.
created_at.
updated_at.
Define duplicate-review rules and authorization before enabling reviews.

promotions (later phase)
id.
listing_id.
owner_id.
promotion_type.
starts_at.
ends_at.
status.
Optional payment_id.
created_at.
payments (only when payments are implemented)
id.
user_id.
related_entity_type.
related_entity_id.
provider.
provider_reference.
amount.
currency.
status.
Optional idempotency key where applicable.
created_at.
updated_at.
Align provider-specific fields and status mapping with official FimiPay documentation.

audit_logs
id.
Optional actor_id.
action.
entity_type.
entity_id.
Safe metadata.
created_at.
Restrict audit-log access and never store secrets in metadata.

9. Authorization Rules
Visitors may search and view published public listings.
Authenticated users may manage their own permitted profile fields.
Owners may manage only their own listings and listing images.
Users cannot grant themselves privileged roles.
Administrators may perform only actions allowed by their assigned role.
Reports and payment records must not be publicly readable.
Every exposed database operation must follow RLS and server-side authorization rules.
Frontend route protection alone is insufficient.
Test both allowed and denied operations.
10. MVP Scope
Required for first release
Landing page and navigation.
Authentication.
Basic user profiles.
Region, district, and area selection.
Categories and subcategories.
Listing creation and editing.
Secure image uploads.
Public listing search.
Category and location filters.
Listing detail pages.
Basic contact options.
External directions links when location is available.
Admin listing moderation.
Secure database policies.
Responsive design.
Basic error handling and tests.
Setup and deployment documentation.
Later enhancements
In-app chat and paid messaging.
Subscriptions.
Paid promotions and advertisements.
FimiPay payment flows for selected paid features.
Advanced reviews and reputation.
Saved listings and alerts.
Dedicated mobile applications.
AI-assisted discovery.
Advanced analytics.
Do not block the core MVP on payment integration, AI, or paid map APIs.

11. Suggested Pages
Home.
Search results.
Category results.
Listing details.
Login.
Registration.
Password recovery.
User profile.
Owner dashboard.
Create listing.
Edit listing.
My listings.
Help and reporting.
Admin dashboard.
Listing moderation.
Category management.
Payment and promotion pages only when implemented.
12. Environment Configuration
Create .env.example with variable names and safe placeholder values only. Use the exact names required by the actual implementation and provider documentation.

Possible configuration categories:

Public Supabase project URL.
Public Supabase client key suitable for browser use.
Server-only Supabase secret credentials, if needed by the selected architecture.
FimiPay test credentials after reviewing official documentation.
FimiPay live credentials for production only.
Documented webhook configuration.
Public application URL.
Other verified deployment settings.
Rules:

Exclude real .env files from Git.
Commit .env.example, never real secrets.
Never expose server-only credentials in client-side variables.
Do not paste secrets into source files, prompts, screenshots, or public repositories.
Separate test and production credentials.
Rotate credentials if exposed accidentally.
13. Implementation Plan
Phase 1 — Foundation
Inspect the existing repository and files.
Confirm the existing framework and dependencies.
Set up React and TypeScript.
Set up the Next.js landing page.
Configure formatting, linting, and tests.
Document local setup.
Phase 2 — Database and Authentication
Configure Supabase.
Create reviewed database migrations.
Configure authentication and profiles.
Enable and test RLS policies.
Phase 3 — Listings and Search
Implement categories and location selection.
Implement listing creation and editing.
Implement secure image uploads.
Implement detail pages, search, filters, sorting, and pagination.
Phase 4 — Owner and Admin Workflows
Implement the owner dashboard.
Implement moderation and reporting.
Verify permissions and audit important actions.
Phase 5 — Location and Experience
Add external directions links.
Evaluate OpenStreetMap and Leaflet.
Improve responsive design, accessibility, loading, and error states.
Phase 6 — Payments
Review official FimiPay documentation.
Document the integration plan based only on verified behavior.
Implement test-mode payment flows.
Verify payments server-side.
Verify webhook authenticity as documented.
Implement idempotent processing.
Test successful, failed, pending, cancelled, retry, and duplicate-notification scenarios as supported by the provider.
Enable production only after testing and review.
Phase 7 — Quality and Deployment
Run automated checks.
Test core workflows and security.
Configure production environment variables.
Configure hosting and deployment.
Document backup, recovery, and maintenance.
Perform a security review before launch.
14. Acceptance Criteria
The MVP is ready for release consideration when:

Visitors can view and search published listings.
Users can filter results by supported categories and locations.
Manual location selection works without GPS permission.
Owners can create and edit their own listings.
Owners cannot modify another user's listings.
Image access follows defined policies.
Unpublished listings are excluded from public search.
Administrators can moderate listings.
Search handles empty results and invalid filters safely.
The interface works on mobile and desktop.
No real secret credentials exist in the public repository.
RLS and authorization rules have been tested.
Errors do not expose sensitive internal details.
Setup and deployment instructions are documented.
Payment-dependent features remain disabled until payment verification is implemented and tested.
15. Risks and Constraints
Initial listing coverage may be limited; provide helpful empty states and owner onboarding.
Owners may supply outdated information; provide editing and reporting workflows.
GPS may be unavailable or denied; preserve manual location selection.
Paid maps can introduce costs; choose an appropriate initial strategy.
Payment behavior must be confirmed from official FimiPay documentation.
Unauthorized access can damage trust; test backend authorization.
User-generated content requires moderation and reporting.
Do not promise exact availability, verified prices, or business outcomes without a process to establish them.
16. Instructions for Claude Code
Act as a careful senior full-stack engineer helping build TafutaGo from the existing repository.

Before changing code:

Inspect the repository, files, framework, dependencies, and completed work.
Do not overwrite working code without understanding it.
Identify missing configuration and ask for necessary non-secret information.
Present a short plan for the next implementation phase.
While implementing:

Follow this PRD and the specified technology stack.
Build real end-to-end functionality, not mock screens alone.
Connect UI components to real database operations where appropriate.
Add migrations and security policies.
Validate inputs on client and server.
Keep secrets out of source control.
Add useful loading, empty, success, and error states.
Add tests for important behavior.
Do not invent API behavior.
Do not claim a feature works until it has been implemented and tested.
Do not perform destructive database changes without explaining the impact and obtaining confirmation.
Avoid unnecessary dependencies and unexplained architecture changes.
Keep the project maintainable for a beginner.
After each phase:

Summarize what changed.
List files created or modified.
Explain required configuration.
Report tests actually run and their results.
Identify limitations and next steps.
17. External Service Status
At the time of writing:

Google Maps API credentials are not assumed to be available.
Google Search API credentials are not assumed to be available.
FimiPay credentials must be configured securely by the project owner.
Official FimiPay documentation must be reviewed before implementing provider-specific requests or webhook verification.
Core discovery and search must work from TafutaGo's own database.
Never expose secret credentials in public documentation.

18. Product Principles
User trust and data security come first.
Basic discovery must be useful without paid APIs.
AI is optional assistance, not an autonomous decision-maker.
Never invent listing information.
Distinguish verified information from owner-provided information.
Make paid functionality transparent.
Build and test incrementally.
Prefer reliable, maintainable solutions over unnecessary complexity.
End of TafutaGo PRD.


