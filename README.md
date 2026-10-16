# FeatureShip

### A deployed full-stack application — and a hands-on demonstration of application troubleshooting, API integrations, authentication, and production support.

**[Live application](https://feature-ship.com)** · **Focus: Support Engineering | Technical Support | Product Support Engineering**

FeatureShip is a feature-request and voting platform where product teams can collect feedback, manage public boards, and prioritize requests using community votes. It includes user authentication, persistent data, subscription billing, and a live production deployment.

**Why this project matters:** I worked across the application stack—from React components and Next.js API routes to MongoDB, OAuth, Stripe, Resend, and Vercel—and investigated issues that crossed those boundaries. The experience reinforced the work I enjoy most: isolating failures, tracing system behavior, identifying root causes, and validating fixes.

## What the application does

- **Feedback boards and feature requests:** Create boards, submit requests, share board links, and manage content.
- **Community voting:** Upvote requests, persist votes, and sort requests by popularity.
- **Authentication and sessions:** Sign in with Google OAuth or email magic links; protect authenticated workflows.
- **Subscription management:** Subscribe with Stripe Checkout and manage subscriptions through the Stripe Billing Portal.
- **Usable application states:** Interactive components, loading indicators, and toast notifications for user feedback.

## Technical stack

| Area                    | Technologies and services                                          |
| ----------------------- | ------------------------------------------------------------------ |
| Language and runtime    | JavaScript, Node.js, npm                                           |
| Frontend                | React, Next.js, Tailwind CSS, DaisyUI                              |
| Backend and APIs        | Next.js API routes                                                 |
| Data                    | MongoDB, Mongoose                                                  |
| Authentication          | Auth.js, Google OAuth, email magic links                           |
| External integrations   | Stripe Checkout, Stripe Billing Portal, Stripe webhooks, Resend    |
| Deployment and workflow | Vercel, Git, GitHub, VS Code, custom domain, environment variables |

## Troubleshooting case studies

These examples reflect issues encountered while building, testing, and deploying FeatureShip. They demonstrate the investigative process behind the finished application—not just the final features.

### 1. Stripe checkout worked, but subscription access did not update

**Symptom:** A customer could complete Stripe Checkout, but the application continued to show **Subscribe** instead of **Billing Portal**.

**Investigation and resolution:** Examined the production subscription flow and its dependencies, including Stripe configuration, application environment variables, and billing management setup. Corrected production key/configuration issues and activated the Stripe Billing Portal link. Verified that the subscription and billing-management experience worked afterward.

**Support Engineering relevance:** Troubleshooting a multi-service workflow, distinguishing successful payment processing from downstream application behavior, and validating configuration changes in production.

### 2. Authentication and email integrations broke during domain migration

**Symptom:** Google OAuth failed with a redirect mismatch after the application moved to its production domain. Email magic-link delivery also required updated sending-domain configuration.

**Investigation and resolution:** Corrected the Google OAuth redirect/domain settings. For Resend, verified the production sending domain, updated the relevant API key and Vercel environment configuration, redeployed, and confirmed email sign-in worked.

**Support Engineering relevance:** OAuth redirects, identity-provider configuration, API-key permissions, email delivery dependencies, and environment-specific troubleshooting.

### 3. Votes appeared to work but disappeared after refresh

**Symptom:** Voting updated the interface, but the result was not consistently retained after refreshing the page.

**Investigation and resolution:** Traced the difference between client-side UI state and persistent server-side data. Corrected the voting request/persistence workflow and addressed the separate problem of restoring the user's selected-vote state.

**Support Engineering relevance:** Reproducing issues, separating frontend symptoms from backend state, and following data through an API-backed workflow.

### 4. Production deployment failed on dependency resolution

**Symptom:** A Vercel build failed with an npm `ERESOLVE` dependency conflict involving MongoDB-related packages.

**Investigation and resolution:** Used the dependency error to identify an incompatibility between installed package versions and the authentication adapter's MongoDB requirement. Resolved the deployment blocker and restored a working production build.

**Support Engineering relevance:** Reading build logs, interpreting package dependency constraints, and isolating environment/deployment failures from application behavior.

### 5. API and routing errors required investigation at different layers

**Symptoms:** Board creation produced a server-side `500`; a dynamic route produced a `404`; and deletion actions failed at different points in the UI-to-API flow.

**Investigation and resolution:** Used server errors and logs to investigate board creation. Found that a route file was named `page,js` instead of `page.js`, causing the `404`. Traced delete failures to missing frontend event-handler wiring and a separate server-route casing issue.

**Support Engineering relevance:** HTTP status interpretation, log-guided debugging, Next.js routing conventions, and isolating client-side versus server-side failures.

## How the pieces connect

```text
User / browser
    |
    v
React + Next.js interface
    |
    +--> Next.js API routes --> MongoDB / Mongoose
    |
    +--> Auth.js --> Google OAuth / Resend magic links
    |
    +--> Stripe Checkout / Billing Portal
               |
               +--> Webhook events --> Application subscription state

Deployment: GitHub --> Vercel --> feature-ship.com
```

Understanding these connections was essential to diagnosing failures. A successful browser interaction does not necessarily mean an API request succeeded; a successful payment does not necessarily mean the application's access state updated; and a working local configuration does not guarantee correct OAuth, domain, or credential settings in production.

## Development and production workflow

I used **VS Code, terminal tooling, npm, Git, and GitHub** during development and deployed the application through **Vercel**. Production work included custom-domain configuration, environment variables, authentication redirects, Stripe subscription settings, and Resend email configuration.

**Configuration and security:** The application relies on environment-specific credentials and settings. Database connection strings, OAuth credentials, Stripe secret keys and webhook signing secrets, and Resend API keys belong in protected environment configuration—not in the repository. No real secrets are documented here.

## Project background

FeatureShip originated as a **guided application built through the CodeFast course**. I completed the implementation, worked through application and integration defects, and configured and troubleshot its production deployment. The examples above describe hands-on problems encountered during that process.

This is a **technical portfolio project**, not a claim of professional full-stack software engineering employment. I include it because it provides concrete evidence of the systems knowledge and investigative skills I bring to **Support Engineer, Technical Support Engineer, and Product Support Engineer** work.

## Explore FeatureShip

**[Open the live application →](https://feature-ship.com)**
