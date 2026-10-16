# FeatureShip

**A deployed feature-feedback application demonstrating hands-on API troubleshooting, authentication, third-party integrations, and production support.**

**[Explore the live application →](https://feature-ship.com)**

FeatureShip helps product teams collect feature requests, share feedback boards, and prioritize ideas through community voting. The application includes authenticated user workflows, MongoDB-backed data, subscription billing, and integrations with external services.

I developed and deployed FeatureShip through the **CodeFast guided project**, then worked through application defects and production integration issues. The most valuable part was learning to **reproduce a problem, trace it across system boundaries, isolate its cause, make a targeted change, and verify the end-to-end result**—the same investigative approach used in Support Engineering.

## What the application does

- **Feedback boards:** Create, share, and manage boards for feature requests.
- **Feature requests and voting:** Submit ideas, upvote requests, and sort by popularity.
- **Authentication:** Sign in with Google OAuth or email magic links, with session-aware access to protected workflows.
- **Subscription management:** Subscribe through Stripe Checkout and manage subscriptions in the Stripe Billing Portal.
- **Administration and user experience:** Delete boards, moderate posts, and interact with loading states and toast notifications.

## Technical skills demonstrated

| Area | Hands-on experience |
| --- | --- |
| **API and application troubleshooting** | Next.js API routes, CRUD workflows, HTTP `404`/`500` errors, tracing browser-to-API-to-database behavior |
| **Third-party integrations** | Stripe Checkout, subscription webhooks, Billing Portal, Resend email sending, Stripe CLI |
| **Authentication and access** | Auth.js, Google OAuth, email magic links, sessions, protected workflows, redirect configuration |
| **Database and persistence** | MongoDB, Mongoose, vote persistence, subscription-access state |
| **Production configuration** | Vercel deployments, environment variables, custom-domain/DNS configuration, API-key permissions, npm dependency conflicts |
| **Development tools** | JavaScript, Node.js, React, Next.js, Git/GitHub, VS Code, npm, terminal, Tailwind CSS, DaisyUI |

**Troubleshooting method:** Reproduce the symptom → identify the failing layer → inspect errors, logs, and configuration → apply a targeted correction → retest the complete workflow.

## Troubleshooting case studies

These are actual problems I encountered while building, testing, and deploying FeatureShip—not hypothetical support scenarios.

### 1. Stripe payment succeeded, but subscription access did not update

**Symptom:** Stripe Checkout successfully processed a subscription, but the production application continued to show **Subscribe** instead of the expected billing-management option.

**Investigation and resolution:** Investigated the production subscription workflow across Stripe and the application, including environment-specific credentials, subscription configuration, and Billing Portal setup. Corrected mismatched production configuration, enabled the Stripe Billing Portal, redeployed, and verified that subscription management worked. I also tested cancellation through the portal.

**Support Engineering relevance:** Multi-service troubleshooting, distinguishing payment success from downstream application state, production configuration diagnosis, and end-to-end verification.

### 2. Resend magic-link authentication failed after a sending-domain change

**Symptom:** After updating the application's email-sending domain, email sign-in failed with a server-configuration error. The failing authentication route referenced `/api/auth/signin/resend` and `sendVerificationRequest`.

**Investigation and resolution:** Confirmed the new sending domain in Resend, reviewed the API-key configuration, created a new key with **Sending Access**, updated the application's Vercel environment variable, and redeployed. A subsequent magic-link sign-in succeeded. The successful key replacement identified the previous key/configuration as the practical blocker; the precise restriction on the old key was not independently confirmed.

**Support Engineering relevance:** API credentials and permissions, third-party email integration, authentication error isolation, environment-variable management, and production retesting.

### 3. Google OAuth required configuration changes for the production domain

**Symptom:** Google sign-in encountered a `redirect_uri_mismatch` during the transition to the production domain.

**Investigation and resolution:** Updated the relevant authorized redirect/domain settings and production URL references, then verified that Google authentication worked on the deployed application.

**Support Engineering relevance:** OAuth callbacks, identity-provider configuration, custom-domain migrations, and cross-service dependencies.

### 4. Voting appeared successful but did not reliably persist

**Symptom:** Clicking an upvote changed the interface, but the result could disappear after refreshing the page.

**Investigation and resolution:** Distinguished temporary React UI state from data persisted through the backend API. Corrected the voting workflow and addressed restoration of the user's selected-vote state as a separate browser-side concern.

**Support Engineering relevance:** Reproducing defects, tracing UI → API → database flows, and verifying persisted outcomes rather than relying on visual feedback.

### 5. Deployment, routing, and API errors had different root causes

**Symptoms:** A Vercel build failed with an npm `ERESOLVE` dependency conflict; board creation returned HTTP `500`; a dynamic board route returned `404`; and deletion actions failed.

**Investigation and resolution:** Used build and server errors to investigate the separate failures. Resolved a MongoDB-related dependency incompatibility blocking deployment; traced the `404` to a file named `page,js` rather than `page.js`; and identified missing `onClick` wiring and route-name casing issues affecting deletion. Investigated board-creation errors through the API and server-side diagnostics.

**Support Engineering relevance:** Build-log interpretation, HTTP error analysis, dependency troubleshooting, framework routing, and isolating frontend, backend, and deployment failures.

## System overview

```text
Browser / React + Next.js UI
  |
  +-- Next.js API routes -------> MongoDB / Mongoose
  |
  +-- Auth.js ------------------> Google OAuth
  |                              Resend email magic links
  |
  +-- Stripe Checkout ---------> Stripe
  |                                |
  |                                +-- Webhook events --> Subscription state
  |
  +-- Stripe Billing Portal ---> Subscription management

GitHub ------------------------> Vercel --> Production domain
```

**Key lesson:** A successful button click does not prove an API write persisted; a successful payment does not prove application access updated; and a working local environment does not guarantee that production credentials, redirects, and external services are configured correctly.

## Development journey

FeatureShip began as a guided application in the **CodeFast** program. I completed **221 of 221 course lessons** in April 2026 and worked hands-on with React/Next.js components, API routes, MongoDB models, authentication, Stripe, and deployment. I later returned to the project to investigate and finish production billing, email authentication, and custom-domain configuration.

That progression—from following implementation lessons to diagnosing unexpected behavior in a deployed application—strengthened my ability to work through unfamiliar technical problems systematically. The production issues were especially useful because several failures came from **configuration and integration boundaries**, not from the visible UI code.

## Development workflow and credential handling

I used **VS Code, Git/GitHub, npm, terminal tooling, and Vercel** to develop and deploy the application. Production setup involved custom-domain configuration, OAuth redirects, Stripe billing and webhooks, Resend sending-domain settings, and environment variables.

**Security:** Database connection strings, OAuth secrets, Stripe secret keys and webhook signing secrets, and Resend API keys belong in protected environment configuration—not in source control. No secrets are included in this README.

## Project context and career relevance

FeatureShip is a **guided CodeFast portfolio project**, not an independently conceived commercial product or a claim of professional full-stack software engineering employment. The troubleshooting, implementation, and production-configuration work described above reflect hands-on experience gained while completing and deploying it.

I include this project to demonstrate transferable skills for **Support Engineer, Technical Support Engineer, Product Support Engineer, and Application Support Engineer** roles: technical investigation, API and authentication troubleshooting, interpreting errors and logs, isolating root causes, and verifying fixes across application and service boundaries.

## Explore FeatureShip

**[Open the live application →](https://feature-ship.com)**
