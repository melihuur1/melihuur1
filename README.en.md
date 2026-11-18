<div align="center">

[🇹🇷 Türkçe](README.md) | [🇬🇧 English](README.en.md)

<img src="assets/header-en.svg" width="100%" alt="Melih — Software Developer: backend, real-time systems and developer tools" />

<p>
  <b>I build backend systems, real-time applications and developer tools.</b><br />
  <sub>Currently looking for software development internship opportunities.</sub>
</p>

</div>

## About Me

The part of software development I enjoy most is the part users never see but everything depends on: data that stays consistent under concurrent requests, sessions that pick up where they left off after a dropped connection, and permissions that are actually enforced on the server.

- **Backend systems:** REST APIs with NestJS, Spring Boot and FastAPI; transactions, constraints and locking strategies in PostgreSQL
- **Real-time applications:** multi-user, reconnect-safe experiences with WebSockets, Socket.IO and Yjs (CRDT)
- **Developer tools:** a local tool for searching codebases and an Obsidian plugin that automates a learning workflow
- **How I work:** testing behavior against real dependencies (Testcontainers, Playwright) and documenting decisions and limitations openly

I also use AI-assisted tools in my development process; before I call a project done, I make a point of verifying it with tests and writing down its limitations openly.

## Featured Projects

### [SyncNotes](https://github.com/melihuur1/syncnotes)

**Real-time collaborative notes — Google-Docs-style co-editing built on a CRDT**

Several people edit the same note at once, see each other's cursors and keep working offline; when they reconnect, their changes merge cleanly.

- Lossless concurrent editing with **Yjs CRDT + Hocuspocus**; two collaboration servers behind nginx, kept in sync through Redis fan-out
- Permissions are enforced on the server **even on WebSocket sessions that are already open**: a revoked user's connection is closed instantly, and even if that event is lost, access ends within 5 seconds
- Offline editing with IndexedDB, version history with one-click restore; refresh-token rotation, CSRF protection and Argon2id
- 119 tests with Jest, Vitest and Playwright — including Redis outages, restarts and offline merges

`TypeScript` `NestJS` `Hocuspocus` `Yjs` `React` `Tiptap` `Prisma` `PostgreSQL` `Redis` `Docker`

**[Source code](https://github.com/melihuur1/syncnotes)** · [Architecture notes](https://github.com/melihuur1/syncnotes/blob/main/docs/architecture.md)

### [StockGuard](https://github.com/melihuur1/stockguard)

**Inventory management and a concurrent reservation engine that cannot oversell**

If one unit is left and 100 customers reserve it at the same moment, exactly one must succeed — on any number of application instances, even if an instance crashes halfway through.

- All coordination lives in PostgreSQL: a conditional `UPDATE ... WHERE available >= q`, `CHECK` constraints and triggers; no JVM locks or shared memory
- Idempotency keys (`INSERT ... ON CONFLICT DO NOTHING`) and safe expiry sweeps with `FOR UPDATE SKIP LOCKED`
- A simulated race of 1,000 concurrent requests for 100 units across two instances: **100 successful reservations, 0 oversold**
- Integration tests on real PostgreSQL via Testcontainers, plus `kill -9` crash scenarios; when each safeguard was removed in turn, the tests caught all 16 removals

`Java 21` `Spring Boot` `Spring Security` `JPA / Hibernate` `PostgreSQL` `Flyway` `Testcontainers` `Docker`

**[Source code](https://github.com/melihuur1/stockguard)** · [Concurrency design](https://github.com/melihuur1/stockguard/blob/main/docs/concurrency.md)

### [RepoMind AI](https://github.com/melihuur1/repomind-ai)

**A local tool that finds where and how something is done in a codebase, and shows the exact lines that do it**

It indexes a GitHub repository locally and lets you browse, search and question it in Turkish or English. Every result is a real range of lines, pinned to the Git commit it came from. It is a local web application for one person on one machine.

- Syntax-aware chunking with Tree-sitter (Java, TypeScript, JavaScript), plus structural splitting for SQL and YAML
- Hybrid search: BM25 keyword search and pgvector semantic search, fused with Reciprocal Rank Fusion; Turkish questions are translated locally
- Analyzed repositories are treated as untrusted input: code is never executed, and pattern-based filters keep secrets out of the index
- **Experimental AI analysis:** a local LLM (`qwen2.5:7b` on Ollama) chooses among the lines found. It is not reliable — in the last measurement it pointed at the right source for 8 of 16 questions, and at a wrong one for 4

`Python` `FastAPI` `Tree-sitter` `PostgreSQL + pgvector` `ONNX Runtime` `Ollama` `React` `pytest` `Playwright`

**[Source code](https://github.com/melihuur1/repomind-ai)** · [Evaluation results](https://github.com/melihuur1/repomind-ai/blob/main/docs/evaluation.md)

### [WatchVenn](https://github.com/melihuur1/watchvenn)

**A real-time group movie picker that ends the "what should we watch?" argument in about a minute**

Someone creates a temporary room and shares the link; everyone votes *Evet* (yes), *Fark etmez* (don't mind) or *Veto* on a few movies, and the group gets the top three movies nobody objects to, each with a reason. No accounts, no install.

- The server owns all state and broadcasts a full room snapshot over Socket.IO after every accepted change
- Reconnect-safe client: nothing is sent until the rejoin is acknowledged and a fresh snapshot has arrived
- The voting algorithm is one pure function (`computeResults`): veto elimination, deterministic ranking and tie rules
- 146 Vitest tests (including integration through real socket.io-client connections); keyboard support, `aria-live` and WCAG AA contrast checks

`TypeScript` `React` `Vite` `Tailwind CSS` `Node.js` `Express` `Socket.IO` `Vitest` `Render`

**[Source code](https://github.com/melihuur1/watchvenn)** · **[Live demo](https://watchvenn.onrender.com)**

<sub>The demo runs on Render's free tier: after a quiet period, the first visit can take up to about a minute while the service wakes up. The interface is in Turkish.</sub>

### [PhraseVault](https://github.com/melihuur1/phrasevault)

**An Obsidian plugin that turns screenshots of English text into numbered, AI-annotated phrase notes**

You paste a screenshot of a subtitle or an article; Claude suggests the expressions worth learning, with Turkish explanations, and after you review and edit them, each one is saved as a numbered note.

- Structured JSON output from the Anthropic API, with schema validation and a check that each quoted expression really appears in the source sentence
- Safe vault writes: notes are only ever created, `INDEX.md` is only appended to, and numbers are never reused
- Estimated cost per analysis from the token usage in the API response, stored as integer picodollars so totals don't drift
- A pure TypeScript core independent of Obsidian and a `VaultPort` abstraction keep it testable; network access is fully blocked in the test suite

`TypeScript` `Obsidian API` `Anthropic SDK` `esbuild` `Vitest` `GitHub Actions`

**[Source code](https://github.com/melihuur1/phrasevault)**

### [AraçKarnesi](https://github.com/melihuur1/arackarnesi)

**A service-verified digital maintenance logbook for vehicle owners and service shops**

Maintenance history lives in one place, service shops get time-limited access, and when the vehicle is sold, its history can be proven with a single link. The interface is in Turkish.

- "Service-verified" records entered by the shop cannot be modified or deleted by the vehicle owner
- Scoped, time-limited access grants for service shops; access is re-evaluated on every request, and users without access get a 404 (existence hiding)
- A time-limited, read-only share link for used-car buyers: costs and personal information are hidden and the VIN is masked
- Maintenance reminders, mileage-inconsistency detection and spending analysis; 168 tests and CI with a Docker smoke test

`TypeScript` `NestJS` `Prisma` `PostgreSQL` `React` `TanStack Query` `Tailwind CSS` `Docker` `nginx`

**[Source code](https://github.com/melihuur1/arackarnesi)** · [Architecture notes (in Turkish)](https://github.com/melihuur1/arackarnesi/blob/main/docs/architecture.md)

## Tech Stack

<table>
  <tr>
    <td><b>Languages</b></td>
    <td>
      <img src="https://img.shields.io/badge/TypeScript-1E293B?style=flat-square&logo=typescript&logoColor=22D3EE" alt="TypeScript" />
      <img src="https://img.shields.io/badge/JavaScript-1E293B?style=flat-square&logo=javascript&logoColor=22D3EE" alt="JavaScript" />
      <img src="https://img.shields.io/badge/Java-1E293B?style=flat-square&logo=openjdk&logoColor=22D3EE" alt="Java" />
      <img src="https://img.shields.io/badge/Python-1E293B?style=flat-square&logo=python&logoColor=22D3EE" alt="Python" />
      <img src="https://img.shields.io/badge/SQL-1E293B?style=flat-square&logo=postgresql&logoColor=22D3EE" alt="SQL" />
    </td>
  </tr>
  <tr>
    <td><b>Backend</b></td>
    <td>
      <img src="https://img.shields.io/badge/Node.js-1E293B?style=flat-square&logo=nodedotjs&logoColor=22D3EE" alt="Node.js" />
      <img src="https://img.shields.io/badge/NestJS-1E293B?style=flat-square&logo=nestjs&logoColor=22D3EE" alt="NestJS" />
      <img src="https://img.shields.io/badge/Express-1E293B?style=flat-square&logo=express&logoColor=22D3EE" alt="Express" />
      <img src="https://img.shields.io/badge/Spring%20Boot-1E293B?style=flat-square&logo=springboot&logoColor=22D3EE" alt="Spring Boot" />
      <img src="https://img.shields.io/badge/Spring%20Security-1E293B?style=flat-square&logo=springsecurity&logoColor=22D3EE" alt="Spring Security" />
      <img src="https://img.shields.io/badge/FastAPI-1E293B?style=flat-square&logo=fastapi&logoColor=22D3EE" alt="FastAPI" />
      <img src="https://img.shields.io/badge/OpenAPI-1E293B?style=flat-square&logo=swagger&logoColor=22D3EE" alt="OpenAPI / Swagger" />
    </td>
  </tr>
  <tr>
    <td><b>Real-time</b></td>
    <td>
      <img src="https://img.shields.io/badge/Socket.IO-1E293B?style=flat-square&logo=socketdotio&logoColor=22D3EE" alt="Socket.IO" />
      <img src="https://img.shields.io/badge/WebSocket-1E293B?style=flat-square" alt="WebSocket" />
      <img src="https://img.shields.io/badge/Yjs%20CRDT-1E293B?style=flat-square" alt="Yjs CRDT" />
      <img src="https://img.shields.io/badge/Hocuspocus-1E293B?style=flat-square" alt="Hocuspocus" />
    </td>
  </tr>
  <tr>
    <td><b>Frontend</b></td>
    <td>
      <img src="https://img.shields.io/badge/React-1E293B?style=flat-square&logo=react&logoColor=22D3EE" alt="React" />
      <img src="https://img.shields.io/badge/Vite-1E293B?style=flat-square&logo=vite&logoColor=22D3EE" alt="Vite" />
      <img src="https://img.shields.io/badge/Tailwind%20CSS-1E293B?style=flat-square&logo=tailwindcss&logoColor=22D3EE" alt="Tailwind CSS" />
      <img src="https://img.shields.io/badge/TanStack%20Query-1E293B?style=flat-square&logo=reactquery&logoColor=22D3EE" alt="TanStack Query" />
      <img src="https://img.shields.io/badge/Radix%20UI-1E293B?style=flat-square&logo=radixui&logoColor=22D3EE" alt="Radix UI" />
      <img src="https://img.shields.io/badge/Zod-1E293B?style=flat-square&logo=zod&logoColor=22D3EE" alt="Zod" />
      <img src="https://img.shields.io/badge/Tiptap-1E293B?style=flat-square" alt="Tiptap" />
    </td>
  </tr>
  <tr>
    <td><b>Data</b></td>
    <td>
      <img src="https://img.shields.io/badge/PostgreSQL-1E293B?style=flat-square&logo=postgresql&logoColor=22D3EE" alt="PostgreSQL" />
      <img src="https://img.shields.io/badge/Redis-1E293B?style=flat-square&logo=redis&logoColor=22D3EE" alt="Redis" />
      <img src="https://img.shields.io/badge/Prisma-1E293B?style=flat-square&logo=prisma&logoColor=22D3EE" alt="Prisma" />
      <img src="https://img.shields.io/badge/Hibernate-1E293B?style=flat-square&logo=hibernate&logoColor=22D3EE" alt="Hibernate" />
      <img src="https://img.shields.io/badge/Flyway-1E293B?style=flat-square&logo=flyway&logoColor=22D3EE" alt="Flyway" />
      <img src="https://img.shields.io/badge/pgvector-1E293B?style=flat-square" alt="pgvector" />
    </td>
  </tr>
  <tr>
    <td><b>AI and search</b></td>
    <td>
      <img src="https://img.shields.io/badge/Anthropic%20API-1E293B?style=flat-square&logo=anthropic&logoColor=22D3EE" alt="Anthropic API" />
      <img src="https://img.shields.io/badge/Ollama-1E293B?style=flat-square&logo=ollama&logoColor=22D3EE" alt="Ollama" />
      <img src="https://img.shields.io/badge/ONNX%20Runtime-1E293B?style=flat-square&logo=onnx&logoColor=22D3EE" alt="ONNX Runtime" />
      <img src="https://img.shields.io/badge/Tree--sitter-1E293B?style=flat-square" alt="Tree-sitter" />
    </td>
  </tr>
  <tr>
    <td><b>Testing</b></td>
    <td>
      <img src="https://img.shields.io/badge/Jest-1E293B?style=flat-square&logo=jest&logoColor=22D3EE" alt="Jest" />
      <img src="https://img.shields.io/badge/Vitest-1E293B?style=flat-square&logo=vitest&logoColor=22D3EE" alt="Vitest" />
      <img src="https://img.shields.io/badge/pytest-1E293B?style=flat-square&logo=pytest&logoColor=22D3EE" alt="pytest" />
      <img src="https://img.shields.io/badge/JUnit-1E293B?style=flat-square&logo=junit5&logoColor=22D3EE" alt="JUnit" />
      <img src="https://img.shields.io/badge/Testing%20Library-1E293B?style=flat-square&logo=testinglibrary&logoColor=22D3EE" alt="Testing Library" />
      <img src="https://img.shields.io/badge/Playwright-1E293B?style=flat-square" alt="Playwright" />
      <img src="https://img.shields.io/badge/Testcontainers-1E293B?style=flat-square" alt="Testcontainers" />
    </td>
  </tr>
  <tr>
    <td><b>DevOps and tools</b></td>
    <td>
      <img src="https://img.shields.io/badge/Docker-1E293B?style=flat-square&logo=docker&logoColor=22D3EE" alt="Docker" />
      <img src="https://img.shields.io/badge/nginx-1E293B?style=flat-square&logo=nginx&logoColor=22D3EE" alt="nginx" />
      <img src="https://img.shields.io/badge/GitHub%20Actions-1E293B?style=flat-square&logo=githubactions&logoColor=22D3EE" alt="GitHub Actions" />
      <img src="https://img.shields.io/badge/Render-1E293B?style=flat-square&logo=render&logoColor=22D3EE" alt="Render" />
      <img src="https://img.shields.io/badge/Git-1E293B?style=flat-square&logo=git&logoColor=22D3EE" alt="Git" />
      <img src="https://img.shields.io/badge/esbuild-1E293B?style=flat-square&logo=esbuild&logoColor=22D3EE" alt="esbuild" />
      <img src="https://img.shields.io/badge/Obsidian%20API-1E293B?style=flat-square&logo=obsidian&logoColor=22D3EE" alt="Obsidian API" />
    </td>
  </tr>
</table>

## LeetCode

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://leetcard.jacoblin.cool/melih411848?theme=dark&amp;ext=activity" />
    <img src="https://leetcard.jacoblin.cool/melih411848?theme=light&amp;ext=activity" alt="LeetCode stats" />
  </picture>
</div>

<br />

<img src="assets/divider.svg" width="100%" height="6" alt="" />

<div align="center">
  <sub>
    <a href="https://github.com/melihuur1/syncnotes">SyncNotes</a> ·
    <a href="https://github.com/melihuur1/stockguard">StockGuard</a> ·
    <a href="https://github.com/melihuur1/repomind-ai">RepoMind AI</a> ·
    <a href="https://github.com/melihuur1/watchvenn">WatchVenn</a> ·
    <a href="https://github.com/melihuur1/phrasevault">PhraseVault</a> ·
    <a href="https://github.com/melihuur1/arackarnesi">AraçKarnesi</a>
    <br />
    <a href="https://github.com/melihuur1?tab=repositories">All repositories</a>
  </sub>
</div>
