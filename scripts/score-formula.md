# Fórmula de priorização

## Por que essa fórmula

Rankz retorna dezenas de ações no plano. Mostrar todas paralisa o usuário. Mostrar 3 bem escolhidas gera ação.

## Cálculo

```
priority_score = (impact × 2) - effort
```

Onde:
- `impact`: 1-5 (1 = ganho mínimo, 5 = ganho enorme)
- `effort`: 1-5 (1 = trivial, 5 = projeto de semanas)

## Exemplo prático

| Ação | Impacto | Esforço | Score |
|---|---|---|---|
| Adicionar HSTS header | 4 | 2 | 6 |
| Refazer arquitetura de URLs | 5 | 5 | 5 |
| Adicionar OG tags | 3 | 1 | 5 |
| Conseguir backlinks DR>40 | 5 | 4 | 6 |
| Reescrever 50 títulos | 4 | 4 | 4 |

Top 3 escolhidas: HSTS (6), Backlinks (6), OG tags (5).

## Tie-breakers

Quando há empate:
1. Prefere `technical` se usuário menciona código/dev
2. Prefere `content` ou `onpage` para outros perfis
3. Prefere ações com `effort <= 2` (quick wins) pra audit inicial

## Personalização futura

V2 vai pesar o score pelo perfil declarado do usuário (founder solo, agência, e-commerce).
