# Computer Networks Mastery

Advanced, practical networking notes for software developers and backend engineers. The material connects protocol theory to browsers, Linux, routers, firewalls, Nginx, Docker, VPS deployments, Node.js/Express and databases.

## Outcomes
- Explain encapsulation, addressing, routing, transport and application protocols.
- Diagnose DNS, TCP, TLS, HTTP, proxy, Docker and database connectivity failures.
- Understand TCP reliability, flow control, congestion control, MTU and performance.
- Design a deployment with public ingress separated from private application and database services.

## Contents
- `docs/lesson-index.md` — map of the 30 lessons to the detailed chapters.
- `docs/` — advanced theory, examples, caveats and review questions.
- `labs/` — Windows, Linux and packet-capture exercises.
- `diagrams/` — Mermaid architecture and protocol diagrams.
- `interview/` — conceptual and scenario-based interview preparation.

## Study method
For every topic, ask: What problem does it solve? At which layer? What messages/state does it use? How does it fail? Which command, log or packet proves the hypothesis?

## Safety
Only capture or test systems you own or are authorized to assess. Packet captures can contain credentials, cookies and tokens. Do not expose database ports publicly merely to test connectivity. Be careful changing firewall rules over SSH; keep a recovery path.

Start with `docs/lesson-index.md`.
