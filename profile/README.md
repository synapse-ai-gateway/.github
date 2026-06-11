<div align="center">

<img src="https://synapse-gateway.com/og.png" alt="Synapse AI Gateway" width="600" />

# Synapse AI Gateway

**Governance-first AI gateway for regulated organisations.**

[Website](https://synapse-gateway.com) · [Quick start](https://github.com/synapse-ai-gateway/synapse-ai-gateway#quick-start) · [Docs](https://github.com/synapse-ai-gateway/synapse-ai-gateway/tree/main/docs)

</div>

---

## The gap in the middle

Teams in finance, healthcare, and the public sector face the same tension: pressure to adopt AI quickly, alongside real obligations around data residency, audit trails, and responsible use. The available tooling falls into three camps — too complex (LiteLLM, Kong, Azure APIM), too expensive (six-figure enterprise platforms), or too cloud-dependent (sending data to a third party, which is a non-starter under data residency rules).

Synapse AI Gateway lives between those three. It is the governance layer a small team can deploy in an afternoon, before they earn the right to a larger platform. Apache 2.0, single `docker compose up`, no enterprise tier.

## Governance bound to the credential

Every API key the gateway issues is bound at creation to a system prompt, a model allowlist, a team identity, and rate limits. Applications cannot override that binding from the request body. Use-case approval becomes a technical control instead of a wiki page — a team approved for a customer support assistant cannot quietly use the same key for something else, because the system prompt and the model allowlist will refuse.

## Every request passes through five layers

1. **Authentication and use-case scoping** — system prompt injection, model allowlist, rate limits
2. **Prompt-layer DLP** — built-in regex engine, three outcomes per category: block, redact, alert
3. **Hybrid model routing** — on-premises (Ollama, vLLM) or cloud (OpenAI, Anthropic, Azure, Google) — data classification on the key decides
4. **Immutable audit logging** — append-only PostgreSQL, SHA-256 hashes only, never plaintext
5. **Output filtering and anomaly detection** — response-side DLP, webhook alerts on usage spikes and repeated DLP blocks

## What's in the box

- **Three containers, one compose command** — postgres + backend + admin console
- **Backend image:** 195 MB · **idle memory:** under 50 MB
- **69 tests · ~88% coverage** · Bandit + Trivy in CI
- **Append-only PostgreSQL audit schema** — designed for regulatory examination
- **OpenAI-compatible REST API** — drop-in at the API layer; backend choice is transparent to consuming applications

## Get started

```bash
git clone https://github.com/synapse-ai-gateway/synapse-ai-gateway
cd synapse-ai-gateway
docker compose up -d
```

Admin console at `http://localhost:5173`, gateway at `http://localhost:8080`. Every setting has a working default for a laptop trial — set real secrets via `.env` before exposing the stack beyond localhost. Full quick start in the [main repo README](https://github.com/synapse-ai-gateway/synapse-ai-gateway#quick-start).

## Documentation

Each subsystem has its own deep-dive:

- [**Governance model**](https://github.com/synapse-ai-gateway/synapse-ai-gateway/blob/main/docs/governance-model.md) — per-key system prompt enforcement, model allowlist, use-case scoping
- [**DLP configuration**](https://github.com/synapse-ai-gateway/synapse-ai-gateway/blob/main/docs/dlp-configuration.md) — regex engine, jurisdiction-specific patterns, block/redact/alert actions
- [**Hybrid routing**](https://github.com/synapse-ai-gateway/synapse-ai-gateway/blob/main/docs/hybrid-routing.md) — data classification, on-premises vs cloud routing decisions
- [**Audit logging**](https://github.com/synapse-ai-gateway/synapse-ai-gateway/blob/main/docs/audit-logging.md) — hash-only schema, regulatory examination, retention

## When you should use something else

For high-scale deployments — millions of requests per day, dedicated platform team, enterprise tooling budget — [LiteLLM](https://github.com/BerriAI/litellm) and Kong are better fits. They solve a different problem at a different scale. Synapse is for the 5-100-person band that needs governance and audit today, not in eighteen months.

## Contributing

Pull requests welcome. Read [CONTRIBUTING.md](https://github.com/synapse-ai-gateway/synapse-ai-gateway/blob/main/CONTRIBUTING.md) for workflow, branch policy, and what makes a good PR. Security issues: please follow the [private disclosure process](https://github.com/synapse-ai-gateway/synapse-ai-gateway/blob/main/SECURITY.md).

---

Built and maintained by **Zaka Ul Haque** — [LinkedIn](https://www.linkedin.com/in/zakaulhaque/) · [GitHub](https://github.com/zaddy88).

Apache 2.0 · No enterprise tier · No feature gating · `synapse-gateway.com`
