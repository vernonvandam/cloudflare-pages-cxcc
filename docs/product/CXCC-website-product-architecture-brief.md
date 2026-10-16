# CXCC website and product architecture brief

**Status:** Implementation brief · **Audience:** product, design, engineering · **Brand:** CXCC

## 1. Product direction

CXCC presents as a customer-experience solution provider with capabilities across systems integration, software development, AI, and automation. Its initial product offer is affordable, managed AI customer service and self-service for small businesses. The first target customers are trades and field services (including window cleaners), appointment-led practices such as chiropractors, dental practices, and accountants, and other service-led businesses. Sell an outcome—fewer missed enquiries and less repetitive admin—with a monthly managed subscription. Keep the small-business product path clear while showing the wider CXCC delivery capabilities and enterprise CX work under the same brand.

**Initial channel:** a shared automation platform is the destination; the first demonstrable channel is website chat. WhatsApp is a planned production channel subject to customer demand, Meta approval and pricing, and integration readiness. The first website demo is a simulation unless and until it is connected to a real, disclosed service.

## 2. MVP definition

### Public website MVP

- Responsive marketing site explaining the customer problem, the managed service, how onboarding works, and who it suits.
- Interactive website-chat demo with a small, authored knowledge base, sample FAQs, suggested questions, graceful fallback, and a visible “demo” disclosure. It must not collect or transmit personal information.
- Lead form that asks only for name, business email or phone, business type, and a short optional note; disclose its purpose and route submissions to a real, secured destination before launch. If no backend is ready, use a clearly labeled email/contact link rather than a nonfunctional form.
- Pricing approach described as a managed monthly subscription, with no unvalidated price promises. Use “talk to us” or an explicitly marked pilot offer until interviews and cost estimates support published tiers.
- Privacy notice, terms, contact details, accessibility basics, and clear consent around any optional analytics.

### Product MVP behind the demo

1. One customer workspace and one website chat widget.
2. Business-provided FAQs and service information, curated and approved during onboarding; start with retrieval over a small, bounded knowledge set rather than arbitrary autonomous actions.
3. Responses constrained to that content, with “I don’t know” behavior and clear handoff to the business.
4. Conversation transcript and basic usage/quality review for the business, with access controls and a retention policy.
5. Manual onboarding and support. Do not build self-serve tenant management, complex workflow builders, CRM integrations, booking/payment actions, voice, or a multi-model orchestration layer for MVP.

**Not in the marketing-site MVP:** real customer conversations, production AI inference, authenticated business dashboard, WhatsApp sending, or claims of 24/7 automated service. These require the service layer and operational safeguards below.

## 3. Website structure and content

| Route | Purpose and essential content | Primary action |
|---|---|---|
| `/` | Outcome-led headline for small service businesses; short explanation; chat demo; proof/benefits; onboarding steps; pricing approach; trust and handoff explanation; enterprise consulting link | Try the demo / book a discovery call |
| `/how-it-works` | Discovery, knowledge capture, configuration, testing, launch, monthly tuning and support; realistic limits and human handoff | Discuss a pilot |
| `/capabilities` | Systems integration, software development, AI and automation, and customer-experience platform delivery; distinguish concepts from available software | Discuss a challenge |
| `/solutions` | Solutions first, followed by related prototypes, product concepts, and illustrative use cases mapped to business sectors; do not imply industry-specific compliance certification | Find your use case |
| `/solutions/trades` | Missed calls/enquiries, service-area and quote FAQs, after-hours lead capture; include window cleaners | Discuss a pilot |
| `/solutions/practices` | Common service, hours, preparation, and appointment FAQs; no diagnosis, tax/legal advice, or sensitive case handling | Discuss a pilot |
| `/solutions/service-businesses` | Common service, coverage, hours, quote and next-step FAQs for service-led businesses | Discuss a pilot |
| `/enterprise` | Genesys/customer-experience consulting, contact-centre modernization and integration advisory; distinct qualification path | Contact CXCC |
| `/about` | CXCC operating approach, service ownership, support expectations and contact details | Contact CXCC |
| `/contact` | Discovery call/contact route and expected response time | Send enquiry |
| `/privacy` and `/terms` | Actual data practices, processors, retention, rights, subscription scope, and limitations | — |

Use a shared header/footer and a focused CTA. Keep enterprise consulting visible but secondary in navigation and page prominence. Avoid unsupported testimonials, quantified savings, security certifications, or customer logos.

## 4. Technical architecture

### Phase A: static marketing site and safe demo

- **App:** Vite + React + TypeScript; small component system; semantic HTML; responsive CSS; routes via a lightweight client router or static route handling.
- **Hosting/deploy:** Cloudflare Pages connected to GitHub. Preview deployments on pull requests; production deploy from the protected main branch. Build command `npm run build`; output directory `dist`. No Wrangler or server-side function is needed for a static marketing site.
- **Demo:** browser-only scripted conversation with a fixed set of questions and responses. Label it “interactive demo — simulated responses”; do not send the visitor’s messages to an AI provider or persist them. Add a reset control and avoid asking for names, account details, health information, financial information, or other sensitive data.
- **Lead handling:** add only after selecting and configuring a real destination. Prefer a small server endpoint or reputable form service with spam controls, validation, rate limiting, secrets kept server-side, and a retention/deletion process. Never place provider keys in Vite `VITE_*` variables or client bundles.
- **Analytics:** optional, minimal and privacy-conscious; disclose it, avoid session replay on forms/chat, and honor applicable consent requirements.

### Phase B: managed production service

```text
Website widget ─┐
                ├─> channel adapters ─> authenticated API ─> tenant configuration/knowledge retrieval
WhatsApp ───────┘                              │                         │
                                               ├─> model provider         └─> approved response policy
                                               ├─> human handoff/notifications
                                               └─> metering, audit and retention controls
```

Keep the marketing site separate from the runtime. Use a Cloudflare Worker or another small backend for authenticated APIs, webhook verification, rate limiting, tenant isolation, secrets, and provider calls; choose a durable store for tenant configuration, approved content, and operational records. Select data storage and model providers only after documenting data location, access, retention, deletion, and contractual terms. The widget should identify its tenant using a public site identifier, while authorization and tenant isolation are enforced server-side. Never trust a client-supplied tenant ID as authorization.

Build a channel-neutral conversation interface (`message`, `conversation`, `handoff`, `delivery status`) with separate website-chat and WhatsApp adapters. Keep customer content, channel identifiers, provider credentials, and internal instructions out of public logs and analytics. Apply strict input/output size limits, abuse controls, prompt-injection resistance, approved-source grounding, safe fallback, and human escalation.

## 5. WhatsApp plan and cost controls

Treat WhatsApp as a separately scoped production integration, not a checkbox in the website demo. Confirm the customer’s WhatsApp Business Platform setup, business/number verification, opt-in and template requirements, webhook ownership, and approved provider path before committing dates. Use Meta’s current pricing and the chosen BSP/provider fees in customer-specific estimates; message charges and provider charges can change and may vary by market, message category, and delivery model. Do not publish fixed WhatsApp unit costs without checking current terms.

Before enabling it, define opt-in evidence, unsubscribe handling, allowed message purposes, template approval ownership, inbound/outbound limits, webhook signature checks, retry/idempotency behavior, delivery/error monitoring, and a route to a human. Pass through or clearly include usage charges in the monthly offer, set alerts and caps where available, and make overages understandable. Validate customers’ willingness to pay for the channel before building generalized WhatsApp administration.

## 6. Security, privacy, and service boundaries

- Collect the minimum data needed; do not invite sensitive health, financial, identity, or payment details in the demo or general chat.
- Publish accurate privacy terms before taking real enquiries or operating a customer bot. Identify CXCC/customer roles, processors, retention periods, deletion/export process, and contact route.
- Keep credentials in server-side secret storage; rotate them and restrict access. Enforce TLS, input validation, rate limiting, origin controls where appropriate, webhook signature checks, and per-tenant authorization.
- Encrypt stored data in transit and at rest through selected providers; restrict staff access and record administrative access. Set a short default transcript retention period, with customer-configured deletion where feasible.
- Treat website content and user messages as untrusted input. Retrieve only approved customer content, avoid executing instructions found in that content, and require confirmation/human review for consequential actions.
- Provide visible AI disclosure, limitations, fallback, and human contact. For dental/chiropractic or accounting customers, keep the bot to approved administrative FAQs; do not provide clinical, financial, legal, or tax advice.
- Establish incident response, backup/deletion procedures, vendor review, and a process for reporting harmful or incorrect answers before production rollout.

## 7. Pricing validation

Do not anchor pricing to an assumed competitor price or promise “unlimited” use. Interview 8–12 businesses across at least two target segments. Ask about missed enquiries, repetitive questions, current handling time, seasonality, channel preference, acceptable onboarding effort, and the monthly value of an additional qualified enquiry or saved staff time. Show concept packages with explicit limits to test willingness to pay; label figures as research concepts, not published offers.

Then pilot with 2–3 businesses. Track setup hours, support hours, conversation volume, useful-answer rate, handoff rate, unresolved rate, provider and channel costs, and customer-perceived value. Set a sustainable monthly floor from direct per-customer costs plus support and margin; separate onboarding if setup work is material. Potential packaging dimensions: included conversations, number of locations, knowledge updates, support response time, and optional WhatsApp usage. Publish tiers only when actual pilot economics and customer feedback support them.

Marketing may explain potential sources of value, such as reducing repeat handling time or helping visitors reach the right next step. Distinguish staff capacity returned from cash savings: capacity only becomes a financial saving when cost is genuinely avoided or is used productively. Use a customer-specific baseline and include setup, service, usage, integration, oversight, and follow-up costs. Do not publish estimated savings as established results or guarantees.

## 8. Implementation sequence and acceptance criteria

1. **Confirm offer and boundaries.** Choose the initial pilot segment, geographic service area, CTA, contact route, and demo script. Write the simulated-demo and no-sensitive-data disclosure.
2. **Create the site foundation.** Initialize the Vite React TypeScript app, shared layout, typography/colors, responsive navigation, footer, and page routes. Configure GitHub repository and Cloudflare Pages preview/production builds.
3. **Build the home page and scripted demo.** Keep the demo deterministic, local-only, resettable, accessible by keyboard, and labeled simulated. Add empty/error states and mobile sizing.
4. **Build supporting pages.** Add how-it-works, segment pages, enterprise consulting, about, contact, privacy, and terms. Use content that matches operational reality and avoid invented proof.
5. **Wire contact and release hygiene.** Configure a real submission destination or use a direct contact link; validate inputs, protect the endpoint, test preview deployments, add page titles/descriptions, sitemap/robots, favicon, and accessibility checks.
6. **Run discovery and pricing interviews.** Record segment and channel preference, willingness to pay, objections, and the most valuable FAQs. Adjust copy/demo before making production promises.
7. **Pilot the production website widget.** Select backend/model/storage providers; complete privacy/vendor review; implement tenant separation, approved knowledge retrieval, fallback/handoff, monitoring, retention, and a small pilot dashboard/operations view.
8. **Add WhatsApp only after a validated pilot case.** Verify current Meta/provider requirements and pricing, obtain customer opt-ins, implement the channel adapter and webhook safeguards, then meter and monitor a controlled rollout.

### Release gate

The public site is ready when all routes work on mobile and desktop; the simulated demo never sends or stores chat text; contact works and its data use is disclosed; production and preview deployments are distinguishable; no secrets are in client assets; privacy/terms match the actual setup; and the site makes no unsupported AI, savings, compliance, or channel claims.

### Initial implementation backlog

- [ ] Confirm CXCC domain, logo, colors, service area, contact destination, and pilot segment.
- [ ] Create Vite + React + TypeScript project and GitHub repository.
- [ ] Connect Cloudflare Pages and verify preview and production branch behavior.
- [ ] Implement global layout, routes, and mobile navigation.
- [ ] Write and review segment-specific copy and simulated demo FAQ set.
- [ ] Implement accessible local-only demo and disclosure.
- [ ] Add contact destination, privacy notice, terms, page metadata, sitemap, and favicon.
- [ ] Conduct pricing interviews; document pilot cost and outcome measures.
- [ ] Specify and review production service architecture before collecting real chat content.
