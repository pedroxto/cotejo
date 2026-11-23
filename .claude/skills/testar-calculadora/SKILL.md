---
name: testar-calculadora
description: Testa a calculadora de valuation do Cotejo (index.html) com números públicos de empresas reais e compara com o que as empresas, o mercado ou o docs/valuation.md mostram, para checar se as contas estão certas. Use quando pedirem para validar, conferir ou auditar a calculadora, o DCF, o WACC ou o exemplo da WEG, ou depois de mudar qualquer lógica de finanças.
---

# Testar a calculadora com números públicos

Objetivo: provar que as **contas** estão certas e mostrar **por que** o resultado difere do que o mercado ou a empresa mostra. Empresas não publicam DCF; o que elas publicam (receita, EBITDA, lucro, caixa, ações) e o que o mercado mostra (preço, EV/EBITDA, P/L, preço-alvo) são os pontos de comparação.

Leia antes `docs/valuation.md` (método, WEG e checklist). Copy em pt-BR; nada de cor fixa; mantenha o aviso "não é recomendação de investimento".

## Passos

1. **Escolha a empresa e a base.** Padrão: WEG (WEGE3), base 2025. Aceite outra empresa se o usuário pedir. Use só dados públicos: release de resultados (RI da empresa), DFP/ITR na CVM, preço de fechamento com data.
2. **Levante os dados reportados** e anote fonte e data de cada um: receita líquida, EBITDA, lucro líquido, caixa líquido (ou dívida líquida), nº de ações, preço. Se não conseguir confirmar um número em fonte, marque como "não verificado" e não o use como verdade.
3. **Teste 1, dados que fecham sozinhos** (sem premissas): margem EBITDA = EBITDA ÷ receita; market cap = preço × ações; EV = market cap − caixa líquido; EV/EBITDA; LPA = lucro ÷ ações; P/L = preço ÷ LPA. Compare com o que a empresa e os portais mostram. Tolerância: arredondamento (±0,1 na margem, ±0,05x nos múltiplos).
4. **Teste 2, WACC** com as fórmulas do doc (Ke USD → Ke BRL → WACC), com CRP cheio e ponderado. Compare com os valores do doc (13,27% e 11,32%). Tolerância: ±0,05 p.p.
5. **Teste 3, DCF.** Rode o mesmo DCF de duas formas:
   - **Completa** (FCFF, 5 anos, valor terminal por value driver e por Gordon), como no doc.
   - **Simulador** (`dcf(fcf,g,r,gp)` do `index.html`, FCF direto e Gordon puro). Use o FCFF do ano 0 e o crescimento médio como entrada. Extraia a função do `index.html` em vez de reescrevê-la, para testar o código real.
   Rode em Node, em arquivo no scratchpad, não na pasta do projeto.
6. **Teste 4, múltiplos e ponte EV → ações:** 15x e 18x EBITDA e P/L 25x, somando o caixa líquido e dividindo pelas ações.
7. **Teste 5, DCF reverso:** ache por bissecção o WACC (e o g) que iguala o valor ao preço de mercado. Um WACC irreal indica que o mercado precifica mais anos de crescimento do que o modelo.
8. **Compare com o que o mercado mostra** (preço, preço-alvo de analistas, múltiplo do setor). Quando houver diferença, **explique a premissa que a causa**, não esconda:
   - CRP cheio × ponderado pela receita (λ)
   - g nominal × inflação; RONIC; capex × D&A no terminal
   - alíquota do WACC (34%) × do fluxo (25%)
   - data-base do fluxo × data do preço
   - 5 anos × 10 anos com fade; Gordon puro × value driver
   - base do beta (emergentes × EUA) e moeda
9. **Teste de sanidade do simulador:** valores iniciais fictícios (FCF 100, g 8%, r 12%, gp 3%, dívida 300, 39 ações). Confira à mão e com `r ≤ gp`, `fcf ≤ 0`, dívida > EV (devem dar erro ou "—").

## Resultado esperado

Entregue uma tabela por teste: **item, valor da fonte, valor calculado, diferença, veredito**.

Vereditos:
- **Igual** (dentro da tolerância)
- **Semelhante se a premissa for outra** (diferença explicada por uma premissa; diga qual e mostre o valor ao trocá-la)
- **Divergente** (conta errada ou dado errado: aponte a linha do código ou do doc e proponha a correção)

Termine com: o que foi verificado em fonte, o que ficou "não verificado", e o aviso de que os exemplos são ilustrativos e não recomendação.

## Cuidados

- Não corrija o doc ou o código sem avisar; relate a divergência primeiro.
- Não cite fonte sem abrir ou ter confirmado a página.
- O simulador é simplificado de propósito (FCF direto, 5 anos, Gordon). Diferença para o DCF completo é esperada; só é erro se a conta do próprio `dcf()` não bater com o cálculo à mão.
