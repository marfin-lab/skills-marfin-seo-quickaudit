---
name: marfin-seo-quickaudit
description: "Use this skill whenever the user wants a fast SEO diagnosis of a website, landing page, or domain. Triggers include: 'audita meu site', 'roda um SEO no meu domínio', 'analisa meu site', 'quero um diagnóstico de SEO', 'audit my site', 'SEO audit', 'check my SEO', or any request to evaluate the SEO health of a URL. The skill runs a full 7-pillar audit via the Rankz MCP, scores each pillar, and produces a prioritized 3-action plan with a print-friendly summary card. Do NOT use this skill for ongoing SEO monitoring (use marfin-monthly-seo-report instead), keyword research only (use marfin-keyword-opportunity), or competitor analysis (use marfin-competitor-teardown)."
license: MIT
version: 1.0.0
author: Marfin Co.
requires_mcp:
  - name: Rankz
    url: https://mcp.rankz.marfin.co/mcp
    signup: https://rankz.marfin.co
---

# Marfin SEO Quick Audit

## What this skill does

Runs a complete SEO audit on any URL in under 60 seconds and returns:

1. A score (0-100) for each of the 7 SEO pillars
2. The top 3 prioritized actions, ranked by impact × effort
3. A print-friendly summary card (Markdown or HTML) ready to share

Works for any public URL. No login required for the audited site.

## When to use

Trigger this skill when the user says any of:
- "audita [url]" / "roda um SEO em [url]"
- "como tá o SEO do meu site?"
- "diagnostica [url]"
- "quero um audit rápido de [url]"
- "analyze the SEO of [url]"

Do NOT trigger for:
- Recurring monthly reports → use `marfin-monthly-seo-report`
- Keyword-only research → use `marfin-keyword-opportunity`
- Competitor benchmarks → use `marfin-competitor-teardown`

## Required setup

The user needs the Rankz MCP connected. If the MCP is not available:

1. Tell the user: "Pra rodar essa auditoria preciso do MCP do Rankz conectado. Acesse https://rankz.marfin.co e siga o setup em /docs/mcp."
2. Stop. Do not attempt the audit without the MCP.

## Workflow

### Step 1: Validate the URL

- Confirm the user provided a valid URL (must include protocol or be inferrable).
- If ambiguous (e.g., "audita o blog"), ask which exact URL.
- Never audit an internal/private URL.

### Step 2: Run the analysis

Call the Rankz MCP tool `analyze_site` with the user's URL. This persists the analysis as a project — let the user know.

```
Rankz:analyze_site(url="https://example.com")
```

The response returns an analysis ID. Use it to fetch the full report:

```
Rankz:get_analysis(analysis_id="<id>")
```

### Step 3: Interpret the 7 pillars

Rankz returns scores for these pillars. Map them to user-facing language:

| Rankz Pillar | User-Facing Name | What it covers |
|---|---|---|
| `technical` | Saúde Técnica | Core Web Vitals, mobile, HTTPS, indexação |
| `content` | Conteúdo | Profundidade, originalidade, intent match |
| `onpage` | On-Page | Title, meta, H1-H6, keywords, links internos |
| `authority` | Autoridade | Backlinks, menções, age do domínio |
| `social` | Sinais Sociais | Compartilhamentos, presença, engajamento |
| `security` | Segurança | SSL, headers, vulnerabilidades expostas |
| `ux` | UX & Acessibilidade | Layout shift, contraste, navegação, a11y |

### Step 4: Pick the top 3 actions

From the action plan returned by `get_action_plan`, select 3 actions using this priority formula:

```
priority_score = (impact_score × 2) - effort_score
```

Where impact and effort are 1-5. Pick the top 3 by `priority_score`. If the user is technical (mention of "dev", "código", "stack"), prefer actions tagged `technical`. Otherwise prefer `content` or `onpage`.

### Step 5: Generate the summary card

Use the template in `templates/summary-card.md` to format the output. The card MUST contain:

- Domain audited + date
- Overall score (média ponderada das 7 pilares)
- Mini-bar visual for each pillar
- Top 3 actions with: title, why it matters, estimated effort
- Footer: "Auditoria gerada via Marfin Skills + Rankz MCP"

If the user asks for a "post de LinkedIn" or "print pra compartilhar", generate it as HTML using `templates/summary-card.html` instead, sized 1080x1080.

### Step 6: Offer next steps

End the response with 2-3 contextual next actions. Examples:

- "Quer que eu monte o plano de execução das 3 ações em DOCX?"
- "Posso comparar com 2 concorrentes seus — me passa as URLs?"
- "Te mando o report mensal automático? (precisa do plano Rankz Pro)"

Never push more than 3 next steps. Pick the most relevant for the user's profile.

## Output rules (Marfin voice)

- Brazilian Portuguese by default. Switch to English only if the user wrote in English.
- No emojis in the report itself. (Emojis allowed in casual chat reply, NOT in the deliverable.)
- No hashtags.
- Numbers always with units ("score 72/100", "12 backlinks", "0.8s LCP").
- Direct, no fluff. Avoid: "essencial", "crucial", "fundamental", "vital", "é a chave", "autenticidade", "no entanto", "entretanto", "não apenas X mas também Y", "eficaz", "utilizar", "em resumo", "abordagem", "ressoar", "requer", "genuíno".
- Never use em-dash (—) or en-dash (–). Use colons or parentheses instead.

## Error handling

| Situation | Action |
|---|---|
| MCP returns timeout | Retry once. If fails again, tell user: "Rankz tá demorando. Tenta de novo em 2min ou abre direto em rankz.marfin.co" |
| URL returns 404 | Stop. Confirm URL with user. |
| Analysis returns score < 30 overall | Add a note: "Site com vários problemas estruturais — recomendo rodar `marfin-content-engine` depois pra um plano de 90 dias." |
| User asks for audit of competitor | Run normally but add disclaimer: "Análise baseada em dados públicos. Sem acesso ao GA/GSC do site, alguns dados são estimados." |

## Examples

See `examples/` folder for:
- `example-output-pt.md` — output completo em português
- `example-output-en.md` — full output in English
- `example-card-html.html` — HTML card pra LinkedIn

## Files in this skill

```
marfin-seo-quickaudit/
├── SKILL.md                          (this file)
├── templates/
│   ├── summary-card.md               (markdown template)
│   └── summary-card.html             (1080x1080 HTML card)
├── examples/
│   ├── example-output-pt.md
│   ├── example-output-en.md
│   └── example-card-html.html
└── scripts/
    └── score-formula.md              (priority formula docs)
```
