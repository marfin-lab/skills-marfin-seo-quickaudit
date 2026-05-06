# marfin-seo-quickaudit

Auditoria SEO completa de qualquer URL em 60 segundos, dentro do Claude.

## O que essa Skill faz

- Roda análise dos 7 pilares de SEO (técnica, conteúdo, on-page, autoridade, social, segurança, UX)
- Gera score 0-100 por pilar e score geral
- Prioriza as 3 ações de maior impacto vs esforço
- Entrega card visual pronto pra compartilhar (Markdown ou HTML 1080x1080)

## Demo

Pergunta no Claude: *"audita wordz.com.br"*

Resposta em ~60s: [veja exemplo](examples/example-output-pt.md)

## Como instalar

### Opção 1: Claude.ai (web/desktop)

1. Conecte o MCP do Rankz: https://rankz.marfin.co/docs/mcp
2. Suba esse repo como skill personalizada na sua conta
3. Pronto. Pergunte "audita meu site" pro Claude

### Opção 2: Claude Code

```bash
git clone https://github.com/marfin-co/skills-marfin-seo-quickaudit.git ~/.claude/skills/marfin-seo-quickaudit
```

E configure o MCP do Rankz no seu `~/.claude/mcp.json`.

## Pré-requisitos

- Conta Rankz (free funciona pra 3 audits/mês): https://rankz.marfin.co
- MCP do Rankz conectado no Claude

## Roadmap

- [x] V1: audit + 3 ações + card
- [ ] V2: comparação com 2 concorrentes na mesma execução
- [ ] V3: integração direta com `marfin-monthly-seo-report` pra agendar follow-up

## Licença

MIT. Use, modifique, redistribua. Cite a Marfin se publicar derivada.

## Outras Skills da Marfin

| Skill | O que faz |
|---|---|
| [marfin-linkedin-postmortem](#) | Diagnóstico dos seus últimos 30 posts no LinkedIn |
| [marfin-cnpj-dossie](#) | Dossiê completo de qualquer CNPJ brasileiro |
| [marfin-keyword-opportunity](#) | Encontra keywords em posição 8-20 (quick wins) |

Catálogo completo: https://marfin.co/skills

---

Feito por [Marfin Co.](https://marfin.co) — venture builder de produtos com IA.
