# h-architecture-case-study
Technical system architecture, database schema layout, and multi-channel integration design for the production-deployed SaaS application.

Production-Ready Cross-Platform SaaS Framework
A commercial, production-deployed web and native mobile software-as-a-service (SaaS) wellbeing application engineered from the ground up for cross-platform delivery.
•	Status: Live in Production (Google Play Store)
•	Architecture: Decoupled Monorepo / Cross-Platform Client Layers
•	Target Platforms: Android, iOS, Web (Responsive)

System Architecture Overview
Humijin is built as a highly performant, data-driven application utilizing a modern serverless backend paired with native mobile and web client layers. The system architecture emphasizes code reuse, zero-downtime deployment pipelines, and efficient real-time state synchronization.


[ Web Client: Next.js ] <----\
                              +----> [ Real-time Gateway / REST / Auth ] ----> [ Supabase DB (PostgreSQL) ]
[ Mobile Client: Expo ] <----/                     |
                                                   v
                                      [ Webhooks / Edge Functions ]
                                                   |
                             +---------------------+---------------------+
                             v                     v                     v
                     [ RevenueCat API ]      [ Paddle / Stripe ]      [ Firebase Cloud Messaging ]


Technical Stack Breakdown

1. Client & Frontend Layer
•	React Native & Expo (Mobile): Engineered a native mobile experience using Expo. Leveraged Expo's ecosystem for native feature bridging, splash screen handling, and binary compilation.
•	Next.js (Web Framework): Constructed the web application and marketing layer using Next.js, optimizing server-side rendering (SSR) and static site generation (SSG) for search optimization and performance.
•	TypeScript: Implemented strict, type-safe development patterns across web and mobile surfaces, reducing runtime execution bugs and formalizing API contract interfaces.

2. Backend & Infrastructure Layer
•	Supabase (PostgreSQL Data Store): Architected a normalized, relational database infrastructure. Deployed advanced PostgreSQL triggers, automated functions, and granular Row Level Security (RLS) policies to protect secure user data.
•	Vercel: Automated continuous deployment pipelines (CI/CD) directly from version-controlled branches, achieving automated production staging.
•	GitHub Actions: Configured code formatting checks, build testing lint rules, and integrated pre-deployment code checks.

3. Integrations & Functional Tooling
•	Subscription Engine (RevenueCat, Stripe, Paddle): Built a multi-channel payment layer. Integrated RevenueCat SDK on mobile to handle cross-platform receipt validation, combined with direct webhooks to manage lifecycle subscription events safely.
•	Notification System (Firebase Cloud Messaging): Deployed FCM pipelines to deliver asynchronous, targeted user notification events based on client activity telemetry.
•	Product Observability (PostHog & Google Analytics): Wired event-driven tracking schemas directly into client event paths to measure user retention, cohort behavior, and user flow conversions.

Key Engineering & Performance Wins

Database Query Optimization & Secure Row Level Security (RLS)
•	Problem: Minimizing client-side fetches while guaranteeing wellbeing data compliance per user tenant.
•	Solution: Developed secure, index-optimized PostgreSQL relational tables inside Supabase. Applied comprehensive RLS policies ensuring users can only read/write data passing cryptographic matching parameters. Deployed optimized indices on compound foreign keys (user_id, created_at), keeping query lookups under 50ms.

Dual-Platform Subscription & Webhook Synchronization
•	Problem: Managing web and mobile billing states simultaneously without letting unauthorized database access occur during payment timeouts.
•	Solution: Engineered a central serverless edge function that consumes webhooks from Stripe, Paddle, and RevenueCat. 
Incoming payloads undergo structural validation, update user authorization metadata in the PostgreSQL layer, and dynamically update the mobile/web client state via real-time database subscription hooks.

Cross-Platform Logical Component Sharing
•	Problem: Writing separate web and mobile layers usually duplicates business logic, validation scripts, and state models.
•	Solution: Isolated business logic hooks, analytical schemas, and validation scripts into a shared directory, achieving significant code reuse between the Next.js web application and the Expo mobile application.


