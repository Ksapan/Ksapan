# GitHub Portfolio Audit — Master Report

**Audit date:** 2026-11-24  
**Scope:** The 10 repositories named in the conversation.  
**Evidence policy:** Claims below are limited to repository metadata, README/package files, and source files that were retrievable for this audit. README feature lists are treated as claims, not proof, unless confirmed in implementation. No performance metrics are invented or presented as independently measured.

> **Scope note:** This environment exposed GitHub repository/file access, not a local filesystem browser. I inspected the repository contents through GitHub, and saved this report to the known profile repository `Ksapan/Ksapan` because no local workspace write tool was available. Some repositories are private; their visible metadata and files could be inspected with the provided access, but this report is not a complete line-by-line audit of every source file. Items explicitly marked “verify” should not be stated as fully implemented until checked against the named paths and a running system.

## Executive recommendation

Your strongest evidence-backed resume projects are:

1. **JD Matching / Standard Recruiter** — best for AI/GenAI and also strong for SDE/full-stack: implemented resume/JD parsing, a deterministic ATS scoring path, selectable AI/local parsing, hybrid NLP services, tailored-resume and export endpoints, and substantial test scripts. The source supports a real, integrated product rather than a prompt-only demo.
2. **HuntScraper / AOH Job Intelligence Engine** — best backend/SDE project: FastAPI, MongoDB, crawler orchestration, ATS detection, filtering/normalization, API surfaces, worker lifecycle, metrics, and job-description repair. It overlaps heavily with `scrape-automationowl-`; select one canonical project entry, not both.
3. **AutomationOwl Social Media SaaS** — best full-stack / backend breadth if the user-facing workflows are demonstrated live: React + Express + MongoDB + Redis/BullMQ, separate worker deployment, integrations and multi-service automation are documented. README is detailed but source paths were not fully audited in this pass, so verify workflows before claiming each integration.
4. **Payroll SMTL Backend** — strong domain-specific backend example: Express 5, MongoDB/Mongoose, payroll calculations, attendance/leave handling, role scope, run lifecycle, statutory deductions and payslip generation. It is a backend-only repository; do not call it MERN/full-stack without its actual frontend repo.
5. **AI On Hunt (AOH)** — substantial recruiting platform with a large JS codebase and resume parser; use as an additional SDE/full-stack/AI example only after checking current branch/runtime. Its README and its package/source evidence do not completely align on what is implemented and which models/providers are used.

**Do not list duplicate “projects”** for the CRM frontend and backend (they are one CRM product), and do not list both scraper repositories as independent products unless you can show distinct shipped deployments/use cases. `Ksapan/Ksapan` is a profile README, not a software project.

---

## 1. Repository inventory and audit confidence

| Repository | Default/audited ref | Available evidence | Audit status |
|---|---|---|---|
| `Automation-Owl-projects/AOH` | `Staging` | README, server/client package manifests, resume parser source, top-level structure and extension README | Partial implementation audit; key claims need manual runtime/source verification |
| `Automation-Owl-projects/Hunt-scraper` | `sapan` | README, requirements, `backend/main.py`, Docker/repo structure | Strong backend evidence; README/source contradictions noted |
| `Automation-Owl-projects/SAAS-Social-Media` | `main` | README, backend package manifest, Docker Compose, backend tree | Good architecture/documentation evidence; individual services not exhaustively source-verified |
| `Ksapan/Ksapan` | `main` | Profile README | Profile/skills claims only; not a project repository |
| `ShriMarutinandan/yogliving-backend` | `main` | Main server source, route inventory, chat route, package files | Backend code exists; AI behavior cannot be confirmed from route alone |
| `Automation-Owl-projects/scrape-automationowl-` | `sapan-01` | README, requirements, `backend/main.py`, app structure | Strong backend evidence; likely closely related to HuntScraper; README claims need verification |
| `ShriMarutinandan/payroll.smtl.backend` | `main` | package, app bootstrap, payroll service/controller and route inventory | Strong backend/business-logic evidence |
| `ShriMarutinandan/CRM-frontend` | `main` | package, app/router, pages/component structure | Frontend implementation evidenced; backend integration should be verified before presenting as complete stack |
| `ShriMarutinandan/CRM-backend` | `main` | package, app bootstrap, route inventory, auth/contact source | Backend API evidenced; repository documentation is weak |
| `Automation-Owl-projects/JD-matching` | `main` | README, manifests, recruiter controller, Groq and ONNX services, context, tests structure | Strongest AI/full-stack implementation evidence |

### Language metadata supplied

The supplied language compositions show primarily JavaScript for AOH, SaaS Social Media, YogLiving, Payroll, CRM frontend/backend, and JD Matching; Python is the largest share in both scraper repositories. These percentages describe repository file composition, not verified skills by themselves. `Ksapan/Ksapan` had no language composition reported.

---

## 2. Individual project audits

### A. JD Matching / Standard Recruiter — `Automation-Owl-projects/JD-matching`

- **Purpose:** ATS-oriented resume and job-description parsing, matching/gap analysis, tailored resume generation, and document export.
- **Languages/frameworks:** JavaScript/Node.js, Express; React 19/Vite client. A Python match subprocess is present. `@xenova/transformers` and `node-llama-cpp` appear in the server dependencies.
- **Frontend/backend/database:** React/Vite client and Express API; MongoDB/Mongoose with an in-memory fallback described in README. Docker Compose is provided.
- **APIs/integrations:** Groq LLM API; code imports Gemini/GLiNER/NLP services and local SLM/parser modules. Puppeteer/PDFKit, `docx`, PDF/DOCX extraction dependencies. Exact deployed provider availability is environment-dependent.
- **AI implementation verified:** `server/src/controllers/recruiter.controller.js` selects parser modes; has deterministic parser + cascade/fallback paths; routes JD parse, resume parse, ATS match/gap analysis, tailored resume, recommendations and PDF/DOCX exports. `server/src/services/groq.service.js` implements Groq chat requests, API-key failover, JSON mode and deterministic extraction fallback. `server/src/services/onnxClassifier.service.js` loads local Transformers feature-extraction and computes similarity for domain/workplace categories. `server/package.json` lists local transformer and llama packages. This is meaningful AI/NLP integration, but not evidence of training/fine-tuning a model.
- **Implemented features with direct evidence:** file/text ingestion; parsing routes; matching; fallback to Python or JS; generated tailored resume; PDF/DOCX download; selectable local and AI parsing; NLP/ONNX classification service; tests declared in `server/package.json` and client E2E scripts.
- **Partial/verify:** README’s “calibrated” weights, accuracy, universal ATS compatibility, “gold-standard,” performance/token claims, fallback semantics and real-world PDF quality need tests/data/runtime proof. `CONTEXT.md` is a domain specification; it is not implementation proof for every named invariant. README environment config says auto-configured JWT fallback; review whether this is acceptable for production.
- **Testing/deployment:** numerous test scripts declared (including parser/JD fixtures and 100-resume batch) and Playwright E2E script; Docker Compose, Nginx/Coolify guidance. Test scripts being present does not mean they passed in this audit.
- **Architecture:** client/server separation, controllers/services/utilities and multiple deterministic/AI parser paths. Interview value is high: explain how deterministic parsing and LLM enrichment are combined, evidence fidelity/fallback, parser evaluation, and safeguards against hallucinated resume content.
- **Weaknesses/risks:** very large feature surface and multiple parser/provider paths are hard to maintain; verify current route wiring and actual installed models. AI generated achievement bullets can be ethically risky if not strictly evidence-grounded—avoid claiming “gap filling” inserts skills the candidate does not actually have.
- **Evidence:** `README.md`; `package.json`; `client/package.json`; `server/package.json`; `server/src/server.js`; `server/src/controllers/recruiter.controller.js`; `server/src/services/groq.service.js`; `server/src/services/onnxClassifier.service.js`; `server/src/services/aiResumeParser.service.js`; `CONTEXT.md`; `docker-compose.yml`; `server/tests/*` and `client/e2e/*` (test existence declared in scripts).
- **Resume fit:** AI/GenAI **#1**; SDE **#1–2**; Node backend **#2**; MERN/full-stack **#2** (React + Express + MongoDB evidenced in README/manifests).

### B. HuntScraper — `Automation-Owl-projects/Hunt-scraper`

- **Purpose:** job/ATS crawling, normalization, storage, search/query and external API access.
- **Languages/frameworks:** Python/FastAPI and a React/Vite frontend according to README; requirements confirm FastAPI, Uvicorn, Motor, HTTPX, BeautifulSoup, `fastembed`, PDF and DOCX dependencies.
- **Database/deployment:** MongoDB Motor; Docker Compose and backend Dockerfile exist. Redis is claimed for queue/DLQ in README; dependency/source wiring should be checked (it is not listed in the retrieved requirements file).
- **AI technologies:** no generative AI implementation established in the inspected code. `fastembed` is declared and README mentions skill enrichment; inspect the enrichment/parser modules before describing embeddings or semantic search as shipped.
- **Implemented evidence:** `backend/main.py` has FastAPI app/lifespan, Mongo connect/close, router registration, job retrieval, queue/start/stop/status API, admin pipeline/run logs, repair endpoints, analytics cache/single-flight refresh, WebSocket status, search profile/keywords, and test pipeline calling ATS detector/crawler/normalizer/filter/schema. Requirements and app tree show a structured Python backend. README names ATS adapters and 10 workers, but the code not fully inspected to validate those counts.
- **Partially implemented / claims to verify:** README describes 150+ portals, 17,000+ active roles, ten parallel workers, DLQ, email notifier, 7-day freshness, HMAC, and performance claims. These are not independently validated by this code sample. README says rate cap of 100/min while `backend/main.py` sets `MAX_REQUESTS = 2000`, uses 60 seconds, and bypasses external/admin/crawler/security paths; this is a direct contradiction. Code CORS allows `*` in retrieved `main.py`, contrary to restrictive-CORS security messaging in related scraper README claims. The current Hunt README says a `diffDays <= 15` condition alongside a “7-day maximum” label—verify freshness logic and wording.
- **Testing:** README mentions load/concurrency tests; actual tests not reviewed. Docker Compose and Dockerfile available.
- **Architecture:** async FastAPI service, Mongo repository/queue, crawler/worker orchestration, frontend. Strong backend/system-design interview project if worker concurrency, deduplication and failure recovery can be demonstrated.
- **Evidence:** `README.md`; `backend/main.py`; `backend/requirements.txt`; `backend/Dockerfile`; `docker-compose.yml`; `backend/app/` tree.
- **Resume fit:** Backend/Node.js (backend section, but Python/FastAPI makes it non-Node) **SDE #1–2**; AI **low/verify**; Full-stack **#3** if frontend integration is demonstrated.

### C. AutomationOwl Social Media SaaS — `Automation-Owl-projects/SAAS-Social-Media`

- **Purpose:** productized business automation SaaS, including social content, lead generation, blogging, e-commerce and voice/recruitment workflows.
- **Languages/frameworks:** JavaScript; React/Vite/Tailwind frontend; Node/Express backend; MongoDB/Mongoose, Redis, BullMQ/IORedis, Socket.IO. README describes Docker Compose and Coolify.
- **AI and integrations:** README and backend manifest list LLM/provider SDKs (Anthropic, OpenAI, Mistral, Groq, Google/Gemini), fal.ai, Vapi/Twilio, Shopify, WordPress, Gmail/social networks, Cloudflare R2, Brevo/Mailgun/Hunter, Stripe/Razorpay, Telegram. These are declared capabilities/dependencies, not all independently verified active integrations.
- **Implemented evidence:** README describes trial/subscription model, services, APIs, admin dashboard, webhook handling and worker separation. `Backend/package.json` confirms provider/integration/queue/storage packages and worker scripts. `docker-compose.yml` configures Mongo, Redis with persistence/password, separate HTTP backend and worker service, frontend, healthchecks, resource limits and volumes. This provides credible architecture evidence; individual feature code was not exhaustively inspected in this pass.
- **Partial / verify:** confirm each integration has live endpoints, credentials, complete error handling and working external callbacks; README says trial and automation paths are operational but no runtime check was performed. Avoid wording like “fully production-ready” or claiming customer/usage outcomes without evidence.
- **Testing:** no test scripts surfaced in retrieved backend package excerpt; validate project tests and CI before asserting test coverage. Container configs are present.
- **Architecture:** multi-service SaaS with HTTP/worker process split, queue-based background work and external integrations. High recruiter value if you can demo one or two end-to-end workflows rather than enumerate every listed service.
- **Evidence:** `README.md`; `Backend/package.json`; `docker-compose.yml`; `Backend/Dockerfile`; `Backend/controllers`, `models`, `routes`, `services`, `src/workers` directories (structure evidence; full code review pending).
- **Resume fit:** SDE **#2–3**; Node backend **#1–2**; MERN/full-stack **#1**; AI/GenAI **#2**, contingent on verifying actual model-backed flows.

### D. AI On Hunt (AOH) — `Automation-Owl-projects/AOH`

- **Purpose:** recruiting/candidate/job management platform with resume processing, matching/automation and a Chrome extension, per README.
- **Languages/frameworks:** JavaScript-heavy; React/Vite/Tailwind client, Node/Express server, MongoDB/Mongoose; Socket.IO and optional Redis described. Server manifest includes LangChain, OpenAI/Pinecone, Transformers.js, Groq-adjacent and document processing dependencies; deployment files include Dockerfiles and Compose.
- **AI implementation verified:** `project/apps/server/services/resumeParser/resumeParser.service.js` implements substantial deterministic resume preprocessing/section parsing, skills/work history/education extraction and Groq API fallbacks for industry classification and title resolution. It returns `localParse: true` and a “Local rule-based candidate parsing” message. This is actual parser logic plus optional LLM augmentation; it is not proof every advertised RAG/semantic matching or agent workflow is working.
- **Other capabilities:** README describes role-based candidate/recruiter/admin workflows and extension; package manifests establish dependencies, not functional activation. Extension README describes installation and autofill but code behavior and supported sites were not reviewed here.
- **Partial / verify:** README lists Groq/OpenAI/Vapi/Tesseract, Pinecone, S3/Cloudinary, payment/email integrations and claims multi-board aggregation/RAG. Retrieved source is insufficient to confirm all are wired and functional. The README says React Query and multiple UI libraries; package manifest is available, but a full client inspection is pending.
- **Testing/deployment:** server package declares Node test scripts and client Docker/nginx setup; README offers build/syntax commands. Tests were not run.
- **Architecture:** monolithic Express server with many controller/service domains, separate React SPA and Chrome Extension. Strong product-engineering evidence, though feature breadth and several integration paths make source-of-truth/runtime verification important.
- **Evidence:** `README.md`; `project/apps/server/package.json`; `project/apps/server/services/resumeParser/resumeParser.service.js`; `project/apps/server/index.js`; client package manifest and source tree; `project/extraApps/chromeExtension/README.md`; `docker-compose.yml`.
- **Resume fit:** SDE **#2**; Node backend **#3**; MERN/full-stack **#2–3**; AI **#2–3** (below JD Matching until AI workflows and retrieval are source/runtime verified).

### E. Payroll SMTL Backend — `ShriMarutinandan/payroll.smtl.backend`

- **Purpose:** payroll, employee, attendance, leave, tax/statutory settings and payslip APIs.
- **Languages/frameworks:** JavaScript, Node.js ES modules, Express 5, MongoDB/Mongoose. Dependencies include bcrypt, JWT, Helmet, express-rate-limit, validators, Excel/PDF generators, cron and date libraries.
- **Implementation verified:** `src/app.js` registers security middleware, CORS allowlist logic, Helmet, proxy trust, request logging, JSON parsing, rate limiting, health checks and route modules for employees, leaves, attendance, holidays, payroll, payslips, TDS and sub-admin settings. `src/services/payroll.service.js` calculates month working days, holiday/attendance/leave effects, prorated earnings, pay split, PF/ESIC/professional tax/TDS, deductions/net pay and leave balances. `src/controllers/payroll.controller.js` implements preview, payroll run creation, run locking, payslip generation, role-aware run listing and access checks.
- **Database/API:** Mongoose models and route/controller/service layout. No frontend in this repository.
- **Testing/deployment:** package script explicitly says no test specified and exits 1; manual/Postman guides and utility scripts exist but are not automated test evidence. Deployment is not established by inspected files.
- **Weaknesses:** manually verify statutory formulas and jurisdiction/year correctness before representing as compliance-grade. Controller uses `runPayrollForEmployee` but retrieved imports show `calculateEmployeePayroll, runPayroll`; this apparent undefined reference in `createRun` requires verification (`src/controllers/payroll.controller.js`, around run creation and helper definitions). Also inspect error response leakage and authorization for admin-only controller actions.
- **Evidence:** `package.json`; `src/app.js`; `src/services/payroll.service.js`; `src/controllers/payroll.controller.js`; `src/routes/*`; `postman-tests.md`.
- **Resume fit:** Node backend **#1** for traditional business backend; SDE **#3**; full-stack **not independently**; AI **not relevant**.

### F. CRM product — `ShriMarutinandan/CRM-backend` + `ShriMarutinandan/CRM-frontend`

Treat the two repositories as **one project**, not two resume entries.

- **Purpose:** CRM admin portal with users, projects, inquiries, contacts, dashboard and sales analytics, based on routes/pages.
- **Frontend evidence:** React 18/Create React App, React Router, protected/admin route components, MUI, Tailwind configuration, charting, Axios and form libraries in `package.json`; `src/App.jsx` wires public/auth/protected/admin pages including project/inquiry/sales analytics screens.
- **Backend evidence:** Express 5, MongoDB/Mongoose, JWT, bcrypt, upload middleware and Gemini/OpenAI SDK dependencies in backend `package.json`. `index.js` mounts auth, user, project, dashboard, inquiry, master, analytics and contact APIs. `controllers/authController.js` demonstrates register/login/JWT and password match; `controllers/contactController.js` demonstrates create/list/read contact messages.
- **AI:** provider dependencies exist, but no verified AI feature/source was inspected. Do not list GenAI as demonstrated without finding invocation/controller code.
- **Testing/deployment:** frontend has React Testing Library packages and test script, but no results reviewed. Backend test script is placeholder and exits 1. Frontend has `vercel.json`; no complete end-to-end deployment proof reviewed.
- **Weaknesses:** CRM backend README was deleted (a tracked `tore README.md` contains a diff artifact); API security/access control should be verified, since `index.js` logs request bodies and auth source logs credentials-related payloads/headers. Never claim these as security best practices.
- **Evidence:** backend `package.json`, `index.js`, `controllers/authController.js`, `controllers/contactController.js`, `routes/*`; frontend `package.json`, `src/App.jsx`, `src/pages/*`, `vercel.json`.
- **Resume fit:** Full-stack/MERN **#3**; Node backend **#4**; SDE **#4**; AI **not verified**.

### G. YogLiving backend — `ShriMarutinandan/yogliving-backend`

- **Purpose:** commerce/booking/partner platform backend, evidenced by product, booking, payment, partner, wishlist, blog, user/auth and admin routes.
- **Languages/frameworks:** JavaScript/CommonJS, Express, MongoDB/Mongoose, Passport/session, uploads/Multer, Cloudinary likely in configuration; packages and route wiring should be checked to confirm integrations.
- **Implementation verified:** `index.js` configures CORS, sessions/Passport, JSON, upload preprocessors/static files, MongoDB connection/migration, public/private product routes, auth, partner/product/location/wishlist/payment/booking/chat/ad/blog/admin APIs, error handling and health check. `routes/chatRoutes.js` mounts `chatWithAI`, but the controller source was not retrievable at the guessed path; the AI behavior itself remains unverified.
- **Partial / risks:** README is only a two-line stub. Main source uses fallback session secret `'your-session-secret'` and permits all origins when no CORS allowlist is configured; verify production environment settings. Large/duplicated `productdetails` route files merit review.
- **Testing/deployment:** not established from inspected files.
- **Evidence:** `index.js`; `routes/*.js`; `README.md`.
- **Resume fit:** Node backend **#3–4**; full-stack only if associated frontend is available and demonstrably integrated; SDE **#4–5**; AI **not verified**.

### H. Scrape AutomationOwl — `Automation-Owl-projects/scrape-automationowl-`

- **Purpose:** job intelligence/crawling platform. Its README is highly similar in domain and terminology to HuntScraper; likely overlapping generation/product line, but independent repository history/use-case was not audited.
- **Languages/frameworks:** Python/FastAPI, Uvicorn, Motor, HTTPX, BeautifulSoup, plus JS/React frontend per README. Dockerfiles/Compose are present.
- **Implementation verified:** `backend/main.py` implements Mongo lifecycle; rate limit (100 requests/60s by client IP); security headers; configurable CORS; external/admin routers; URL queue/discovery; crawler start/stop/status; job list/detail; search profile, keywords; test pipeline for detection/crawl/normalization/filter/schema. `requirements.txt` contains FastAPI, Uvicorn, Motor, dotenv, HTTPX, BeautifulSoup only.
- **Partial / verify:** README advertises 130+ ATS, 21-field schema, MD5 dedup, security audit, low latency and load testing; implementation directories exist but the relevant adapters/dedup/test files were not individually read. README describes AI & keyword enrichment but dependencies and inspected API do not establish which AI model is used. Do not call it GenAI without source evidence.
- **Testing/deployment:** Dockerfile and Docker Compose; README claims concurrency/performance testing, but tests were not run.
- **Evidence:** `README.md`; `backend/main.py`; `backend/requirements.txt`; `backend/Dockerfile`; `docker-compose.yml`; `backend/app/*` structure.
- **Resume fit:** SDE **#1–2**; backend **strong Python, not Node**; full-stack **#3** if UI works; AI **low/unverified**.

### I. Profile repo — `Ksapan/Ksapan`

- **Purpose:** profile and portfolio README, no application project source established.
- **Content:** claims skills across GenAI, RAG, LangChain/LangGraph, multiple vector DBs, Node, React, Python, cloud and several featured builds. The README contains self-reported proficiency percentages and employment impact numbers; they are not corroborated by this repository audit and should not be copied as objective resume evidence without independent verification.
- **Resume fit:** do not list as a project. Use as an index only after linked projects are verified.
- **Evidence:** `README.md`.

---

## 3. Duplicate projects/features and consolidation

### Consolidate / do not duplicate

1. **HuntScraper + scrape-automationowl-:** Both are job crawling/scraping, ATS detection, Mongo-backed job storage/query, queue/crawler APIs, Docker and React/FastAPI platform. The older `scrape-automationowl-` README and `Script JOB_databse.md` have the same blob SHA; HuntScraper appears to be a more detailed/updated presentation but repository history was not compared. Treat as one **Job Intelligence / ATS Scraper** project on the resume until distinct functionality/deployments are proven. Use HuntScraper as the stronger entry based on inspected source breadth, but verify README claims first.
2. **CRM frontend + backend:** one CRM application with two repos; cite both together in a single entry.
3. **JD Matching vs AOH recruiting platform:** Related problem domain but materially distinct repositories. Do not claim duplicated implementation. On a one-page resume, prefer JD Matching for AI role and AOH for broader recruiting SaaS/extension only if the latter’s capabilities are verified.
4. **YogLiving and CRM:** Distinct business domains; no evidence they are duplicates.
5. **Social Media SaaS:** Can be a distinct SaaS product. Avoid repeating generic features (auth, dashboards, CRUD, AI integration) across multiple project descriptions; highlight its distinct worker/queue and automation integration system.

### De-duplicate feature bullets

Across resumes, state common capabilities once under the relevant chosen project: JWT/auth/RBAC, CRUD/admin dashboards, MongoDB, REST APIs, Docker, and AI provider integration should not be repeated as if separate accomplishments. Reserve unique bullets for the differentiators: JD’s multi-parser ATS matching and document export; scraper’s crawler/normalization/queue; SaaS’s worker separation and connected automation workflows; Payroll’s payroll computation and lifecycle.

---

## 4. Project ranking by objective resume value

Ranks combine implementation evidence, complexity, usefulness, engineering structure, recruiter relevance and interview depth. Rankings are comparative, not claims about production usage.

### Overall portfolio ranking

1. **JD Matching / Standard Recruiter** — broadest verified combination of working AI/NLP code, backend APIs, deterministic algorithms, file processing, tests and UI.
2. **HuntScraper / Job Intelligence Engine** — strong async backend, crawl pipeline and operational controls; claims about scale/worker count need confirmation.
3. **AutomationOwl Social Media SaaS** — broad full-stack system and thoughtful container/worker split, with multiple third-party integrations documented; implementation depth needs a source/runtime pass.
4. **Payroll SMTL Backend** — strongest concentrated Node business-logic implementation evidenced in code; actual test coverage is weak/unverified.
5. **AI On Hunt** — substantial recruiting platform/parser but broad README claims exceed evidence inspected here; confirm current running state.
6. **CRM full-stack product** — useful conventional full-stack app with auth, business modules and dashboards; thin backend tests/documentation and security logging concerns.
7. **YogLiving backend** — real route breadth, but sparse documentation and unresolved AI behavior; not enough evidence for top resume slot.
8. **scrape-automationowl-** — substantial related scraper implementation, but overlaps HuntScraper and has less evidence of differentiated value.
9. **Ksapan profile README** — not a software product.

### Role-specific shortlists

| Resume | Project order | Best evidence emphasis |
|---|---|---|
| **Software Engineer / SDE** | JD Matching → HuntScraper → Payroll SMTL → Social Media SaaS | algorithms/processing pipeline, async work, API design, reliability, tests, containers; quantify only with measured results |
| **Backend Developer / Node.js** | Payroll SMTL → Social Media SaaS → JD Matching → AOH → CRM backend → YogLiving | Express APIs, Mongo/Mongoose, auth/access controls, domain logic, background workers, integrations; mention Python scraper only as cross-stack supplemental |
| **Full Stack / MERN** | Social Media SaaS → JD Matching → AOH → CRM pair → HuntScraper | React + Node/Express + Mongo evidenced/documented; demonstrate complete user workflow and frontend-to-API integration |
| **AI Engineer / Generative AI** | JD Matching → AOH → Social Media SaaS → HuntScraper (only if embedding implementation confirms) | model-backed parsing/structured extraction, local Transformers/ONNX, deterministic-vs-LLM cascade, grounded extraction/fallback, evaluation. Do not call simple provider SDK dependencies “AI implementation.” |

---

## 5. Technologies actually demonstrated vs merely claimed

### Strongly evidenced in implementation or manifests

- **JavaScript / Node.js / Express**: present across AOH, JD Matching, SaaS, Payroll, CRM and YogLiving.
- **React**: evidenced by client repo structures/manifests for JD Matching, SaaS, CRM and AOH; scraper frontends documented and directories present.
- **MongoDB / Mongoose**: source/manifests across payroll, CRM, YogLiving, JD Matching and AOH; Mongo async Motor in Python scraper projects.
- **Python / FastAPI**: explicitly implemented and dependency-evidenced in both scraper backends.
- **REST APIs, controllers/routes, middleware, auth and validation**: evidenced across backend repos (quality and access control vary).
- **Docker / Docker Compose**: configuration exists for JD Matching, AOH, both scraper repos and SaaS.
- **Redis / BullMQ**: dependency/configuration evidence is strongest in SaaS; README/Compose document queue services and separate workers.
- **AI/ML actual source evidence:** Groq calls and parser enrichment in AOH; Groq calls, local Transformers/ONNX classifiers, multi-stage parsing and AI/local fallback in JD Matching. JD is the strongest verified GenAI/AI implementation.
- **PDF/DOCX processing/output:** JD Matching has extraction and generation dependencies/code routes; AOH and Payroll have relevant document libraries/dependencies, but feature-specific workflows need verification.

### Mention only after confirming hands-on depth or use in the target project

- LangChain, Pinecone, OpenAI, Gemini, Vapi, Cloudinary/S3, Stripe/Razorpay, Redis across every project: package entries/README claims do not prove the integration was implemented or used.
- “RAG,” vector search, agents, semantic search, fine-tuning, multi-agent systems: not established broadly by dependency names. JD has semantic/ONNX classification and GenAI pipelines, but the inspected evidence did not establish model fine-tuning or a production RAG pipeline. Do not claim fine-tuning.
- TypeScript, Java, AWS, Kubernetes, LangGraph, Pinecone/Chroma/Qdrant/pgvector and listed proficiency percentages appear on the profile README, not proven by the inspected project source. Exclude from “demonstrated technologies” unless supported by other code or work experience.

---

## 6. Current resume gaps and missing proof

No current resume files were supplied or found in the repository set, so I cannot compare against your actual resume text. From the project evidence alone:

- **AI Engineer gap:** No verified fine-tuning/training pipeline, model evaluation benchmark, deployed RAG evaluation, or model-quality/latency/cost measurement was established. The portfolio is strongest in applied LLM extraction/classification and AI-backed product integration.
- **Node backend gap:** Automated backend test evidence is weak in Payroll/CRM (scripts are placeholders). Add meaningful unit/integration tests before emphasizing engineering quality.
- **SDE gap:** Add clearly documented test results, API contracts, architecture diagrams, error/retry semantics and reproducible setup for the largest projects.
- **MERN gap:** Ensure one repo/project contains a demonstrable end-to-end React-to-Express-to-Mongo workflow and live demo; do not infer full-stack completeness from separate repos alone.
- **Metrics gap:** No independent metrics were collected in this audit. Capture reproducible measures (test corpus size, parsing field accuracy, scrape success rate, p95 latency, queue throughput, failure/retry rate) before adding numeric resume bullets.
- **Ownership gap:** Repository access does not prove your individual contribution or employment context. Make sure each resume claim distinguishes your work from team/shared project work.

---

## 7. Exact contradictions and manual verification queue

1. **HuntScraper rate limiting and security claims**  
   README says **100 requests/minute**; source `backend/main.py` sets `MAX_REQUESTS = 2000` and `WINDOW_SECONDS = 60`, and skips rate limiting on several endpoint prefixes. README also describes HMAC/security controls; verify the route-level access controls because the global middleware bypasses external/admin endpoints.  
   **Manual files:** `Automation-Owl-projects/Hunt-scraper/backend/main.py`, `backend/app/auth/*`, `backend/app/api/*`, `README.md`.
2. **Scraper CORS security statement**  
   `scrape-automationowl-` README describes restricted origins, while HuntScraper `backend/main.py` configures `allow_origins=["*"]`. The older scraper `backend/main.py` uses `settings.ALLOWED_ORIGINS`, but warns rather than blocks wildcard configuration. Confirm each repository/ref and production env separately.  
   **Manual files:** `Automation-Owl-projects/Hunt-scraper/backend/main.py`; `Automation-Owl-projects/scrape-automationowl-/backend/main.py`; each `backend/app/config.py`; Docker/env examples.
3. **“7-day maximum” vs `diffDays <= 15`** in HuntScraper README. Confirm intended freshness behavior in actual filter implementation.  
   **Manual files:** `Automation-Owl-projects/Hunt-scraper/README.md`; `backend/app/filtering/*`.
4. **Resume parser claims vs implementation naming in AOH**  
   AOH README calls the platform LLM-powered; the inspected parser explicitly says local rule-based parsing and only invokes Groq for some fallback enrichment paths. This can both be true, but the resume should describe the hybrid implementation, not imply every resume is parsed by an LLM. README also lists multiple provider/model/vector capabilities not verified by the inspected parser.  
   **Manual files:** `Automation-Owl-projects/AOH/project/apps/server/services/resumeParser/resumeParser.service.js`; `services/llm/llmRegistry.service.js`; resume parser subdirectory; matchingEngine services; relevant controllers/routes.
5. **Scraper dependency vs architecture documentation**  
   HuntScraper README lists Playwright and Redis Queue, but retrieved `backend/requirements.txt` has no Playwright or Redis dependency. These may live in optional modules/other dependency files or represent stale docs.  
   **Manual files:** `Automation-Owl-projects/Hunt-scraper/backend/requirements.txt`; any `pyproject.toml`/secondary requirements files; crawler adapters; Dockerfile; Compose.
6. **JD matching “ATS score” hardcoded-looking fallback**  
   `server/src/controllers/recruiter.controller.js` logs `tailoredResume?.atsScore || '96%'`; do not present 96% as an actual achieved ATS result. Check score generation and test fixture expectations.  
   **Manual files:** `server/src/controllers/recruiter.controller.js`; `server/src/utils/resumeGenerator*`; ATS match engine; tests.
7. **Payroll run helper reference**  
   `src/controllers/payroll.controller.js` calls `runPayrollForEmployee` inside `createRun`, while imported functions shown are `calculateEmployeePayroll` and `runPayroll`; a helper with that name exists in the same file, so confirm invocation and current branch code. More importantly automated test is placeholder.  
   **Manual files:** `ShriMarutinandan/payroll.smtl.backend/src/controllers/payroll.controller.js`; `src/services/payroll.service.js`; `package.json`.
8. **YogLiving AI claim**  
   Chat route imports `controllers/chatController`; the route exists but source implementation was not retrieved. Cannot confirm LLM, memory, or guardrails from the route alone.  
   **Manual files:** `controllers/chatController.js`; associated service/config; `package.json`.
9. **Personal profile experience/latency claims**  
   Profile README states job titles/dates and a 40% latency reduction. Repository source cannot substantiate employment history or that metric.  
   **Manual verification:** employment records, before/after measurement methodology, target system and source of measurements; do not copy without confirmation.
10. **CRM backend security/AI**  
    Provider SDK dependencies exist, but inspected controller source does not establish AI use; request logging includes request body/header logs and should be checked for credential/PII exposure.  
    **Manual files:** `CRM-backend/index.js`; AI-related controllers/services; auth middleware; `.env` handling and logging.

---

## 8. Suggested final project selection and resume wording

### Software Engineer / SDE (choose 3)

1. **JD Matching / Standard Recruiter** — ATS parsing/matching platform with deterministic + model-assisted resume/JD pipelines, document processing, backend APIs and PDF/DOCX export.
2. **Job Intelligence Engine (HuntScraper; consolidate the two scraper repos)** — asynchronous ATS crawling, normalization, persisted queues, job APIs and operational monitoring.
3. **Payroll SMTL** — payroll and attendance business rules, monthly run lifecycle, access-scoped APIs and payslips.

### Backend / Node.js (choose 3)

1. **Payroll SMTL Backend** — Node/Express, Mongoose, layered routes/controllers/services, payroll calculations, attendance and leave integration, role-scoped run/payslip flows.
2. **AutomationOwl SaaS** — Node/Express/Mongo/Redis/BullMQ; separate HTTP and worker services; describe only integrations verified in code/demo.
3. **JD Matching API** — Express, file parsing, model provider abstraction/fallbacks, deterministic ATS match logic, document export.

### Full Stack / MERN (choose 3)

1. **AutomationOwl SaaS** — best complete product architecture on available docs; verify end-to-end user flows.
2. **JD Matching** — React/Vite + Express/Mongo and file-to-match-to-export workflow.
3. **CRM** — combine frontend/backend repositories into a single CRM product entry; emphasize auth, inquiry/project workflows and analytics.

### AI Engineer / GenAI (choose 2–3)

1. **JD Matching** — top AI project; describe hybrid resume/JD parsing, Groq structured extraction, local ONNX classification and deterministic fallback. Do not claim fine-tuning or benchmark performance.
2. **AOH Resume Parsing / Recruiting Platform** — describe deterministic extraction with selective Groq fallback/augmentation, only after confirming active routes and model provider code on the current branch.
3. **AutomationOwl SaaS AI workflow** — include only one or two source-verified model-backed workflows (e.g., blog generation or voice); avoid listing every SDK as a capability.

### Safe resume bullet templates (add no unverified numbers)

- **JD Matching:** “Built a full-stack ATS resume/JD analysis application with multi-format document ingestion, structured parsing, deterministic skill/experience matching, model-assisted extraction with local fallbacks, and PDF/DOCX export.”
- **Job Intelligence Engine:** “Implemented FastAPI endpoints and asynchronous crawler controls for job discovery, queue management, normalized job retrieval, administrative run logs, and live status reporting backed by MongoDB.”
- **Payroll SMTL:** “Developed Node.js/Express payroll APIs that combine attendance, approved leave, holidays, configured pay splits and statutory deductions to produce payroll previews, runs and payslips.”
- **AutomationOwl SaaS:** “Designed a containerized SaaS architecture separating API and background workers, with MongoDB persistence and Redis/BullMQ job processing; integrated only the services confirmed in the shipped workflow.”
- **CRM:** “Delivered a React admin portal and Express/MongoDB APIs for project and inquiry workflows, user authentication, contact-message management and sales analytics.”

Replace “built/designed” with the exact level of your personal contribution. Add metrics only after reproducing them and documenting the measurement method.

---

## 9. Recommended next actions

1. Run the exact test/build commands for JD Matching, AOH, both scraper repos, SaaS, Payroll and CRM; record pass/fail and commit SHA.
2. Verify the contradiction list, especially rate limits/CORS, AI provider calls, resume parser active routes and Payroll `createRun` behavior.
3. Create one-page architecture diagrams for JD Matching, scraper and SaaS; include data flow, failure/retry behavior and trust boundaries.
4. Add or repair automated backend tests for Payroll and CRM; remove placeholder test scripts that exit nonzero.
5. Capture real, reproducible project metrics and link test datasets/demo deployments.
6. Share the actual current resume files if you want a direct missing-technology/project comparison; this report cannot determine what your resumes already contain because no resume document was in the audited repository set.
