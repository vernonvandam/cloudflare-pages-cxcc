# CXCC project guidance

## Project overview

CXCC helps organizations with customer-experience systems integration, software development, AI, and automation. Its initial product offer is affordable, managed website chat for small service businesses, including trades and field services, window cleaners, appointment-led practices such as chiropractic, dental, and accounting businesses, and other service-led companies.

CXCC is one brand. Genesys and enterprise customer-experience consulting is a secondary offering.

## Product direction

- Build a shared automation platform for website chat and WhatsApp.
- Start with website chat.
- Keep the initial website-chat offer focused on affordable, recurring, managed service for small businesses; present integration, software development, AI, and automation as broader CXCC delivery capabilities.
- Avoid overbuilding. Prefer a small, useful pilot over generalized platform features.
- Treat WhatsApp as a later production channel that depends on validated customer demand, current Meta requirements, and a clear cost model.

## Website and technology

- Use Vite, React, and TypeScript for the website.
- Deploy the static marketing site to Cloudflare Pages through GitHub.
- Do not add Wrangler or server-side infrastructure to the static marketing site unless the product requires it.
- Keep the public website separate from any future production conversation service.
- Follow the product brief in `docs/product/CXCC-website-product-architecture-brief.md` as the source of truth for scope and implementation direction.

## Demo and product claims

- The first website chat demo is simulated, runs locally in the browser, and must be clearly labeled as a demo.
- Do not send or store visitor chat text in the simulated demo.
- Do not describe simulated behavior as a production AI service.
- Do not claim specific savings, accuracy, compliance, customer results, or channel availability without evidence.
- Make AI limitations, fallback, and human handoff clear wherever relevant.

## Privacy and security

- Do not collect sensitive health, financial, identity, or payment details through the demo.
- Never put API keys, credentials, or other secrets in client-side code or `VITE_*` environment variables.
- Before adding real chat, forms, analytics, or third-party services, document what data is collected, where it goes, who can access it, and how long it is retained.
- Keep customer data isolated by tenant in any production service.
- Treat user messages and customer-provided content as untrusted input.
- For health, accounting, and similar practices, keep the bot within approved administrative information; do not provide professional advice.

## Working practices

- Keep changes within the agreed MVP unless the product brief or a later decision changes scope.
- Prefer accessible, responsive, semantic HTML and straightforward components.
- Update the relevant documentation when a product, architecture, or privacy decision changes.
- Do not add dependencies or infrastructure without a clear need.
