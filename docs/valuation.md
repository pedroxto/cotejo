# Contexto de valuation (DCF + múltiplos)

Base de conhecimento do Cotejo. O simulador do `index.html` é uma versão simplificada (FCF direto, 5 anos, Gordon puro); este documento descreve o método completo que o produto deve seguir.

## Estrutura

1. **FCFF** = EBIT·(1−t) + D&A − CAPEX − ΔNWC
2. VP do período explícito + **VT** (valor terminal), ambos descontados ao **WACC**
3. **EV → Equity:** EV − dívida líquida − minoritários (+ caixa líquido) → ÷ nº de ações
4. **Múltiplos:** EV/EBITDA (passa de EV a equity pela mesma ponte) e P/L (direto no equity), sempre com comparáveis reais
5. Cruzar DCF × múltiplos (*football field*) e fazer **DCF reverso**

## WACC

- Ke(USD) = Rf + β × (ERP maduro + CRP)
- Ke(BRL) = (1 + Ke_USD) × (1 + π_BR) / (1 + π_EUA) − 1
- WACC = E/(D+E) × Ke + D/(D+E) × Kd × (1 − t)
- Dados Damodaran (jan/2026): Rf 4,18% (UST 10a), ERP maduro ~4,20%, CRP Brasil ~3,24%. Atualizar pela planilha `ctryprem`.
- **CRP ponderado:** não aplicar o CRP inteiro a uma exportadora. Usar λ = exposição da receita ao país: Ke = Rf + β·ERP + λ·CRP.

## Valor terminal

- Gordon puro: FCFF_{n+1} / (WACC − g)
- **Value driver (preferido):** NOPAT_{n+1} × (1 − g/RONIC) / (WACC − g)
- g nominal em BRL ≤ crescimento nominal da economia.
- Sempre mostrar **TV/EV** e o **múltiplo de saída implícito** (TV / EBITDA_n+1).

## Exemplo real: WEG (WEGE3), base 2025 (release 4T25)

| Item | Valor |
|---|---|
| Receita líquida | R$ 40.804,1 mi |
| EBITDA | R$ 9.000,0 mi (margem 22,1%) |
| Lucro líquido | R$ 6.376,2 mi |
| Caixa líquido | R$ 2.652 mi |
| ROIC | 32,5% |
| Ações | 4.197,3 mi |
| Preço de referência | R$ 46,82 (31/07/26) |
| Market cap / EV | ~R$ 196,5 bi / ~R$ 193,9 bi |
| EV/EBITDA · LPA · P/L | ~21,5x · ~R$ 1,52 · ~30,8x |

**WACC** (β = 1,0; π_BR 4,0%; π_EUA 2,2%; Kd 11%; t 34%; 95% E / 5% D):
- CRP cheio: Ke USD 11,62% → Ke BRL 13,59% → **WACC ≈ 13,27%**
- CRP ponderado pela receita doméstica (λ ≈ 38% no 4T25): Ke BRL 11,54% → **WACC ≈ 11,32%**

**DCF de 5 anos** (premissas ilustrativas): crescimento 9/8/7/6/5%; margem EBITDA 22%; D&A 2,8% da receita; IR 25%; capex de 6% caindo para 4%; ΔNWC = 15% do Δreceita; g = 4%; RONIC = 20%.
- EV ≈ R$ 59 bi → **~R$ 14,75/ação** (WACC 13,27%) ou **~R$ 18,59/ação** (WACC 11,32%)

**Múltiplos** (EV + caixa líquido ÷ ações):
- 15x EBITDA → ~R$ 32,80 · 18x EBITDA → ~R$ 39,23 · P/L 25x → ~R$ 37,98

### Leituras

- O DCF fica bem abaixo do mercado. O DCF reverso pede um WACC de ~6,9% em BRL nominal, o que não é plausível. O mercado está pagando por crescimento e ROIC alto por muito mais de 5 anos.
- Um TV de só ~5,9x EBITDA contra 21x hoje mostra que 5 anos é pouco para um ROIC de 32%. Usar 10 anos com *fade* de crescimento e de ROIC, ou um modelo de três estágios.
- Premissas ilustrativas não são recomendação. Substituir D&A, alíquota efetiva, capex e NWC pelos números da DFP.

## Checklist: sempre questionar

- **CRP em dobro:** o CRP pode já estar no Kd em BRL ou no β local.
- **g vs. inflação:** um g nominal de 4% com π_BR de 4% significa crescimento real zero.
- **Capex vs. D&A no terminal:** precisam ser coerentes com g e RONIC (reinvestimento = g/RONIC).
- **Alíquota:** o exemplo usa t = 34% no WACC e IR = 25% no fluxo. Escolher uma (de preferência a efetiva) ou justificar a diferença.
- **Data-base:** fazer roll-forward do valor até a data do preço (base 2025 vs. preço de 31/07/26).
- **Consistência de moeda:** fluxo em BRL nominal com taxa em BRL nominal.

## Referências

- Damodaran: *Investment Valuation*; datasets `ctryprem` e `betas`
- Koller, Goedhart & Wessels (McKinsey): *Valuation*
- Assaf Neto: *Valuation: Métricas de Valor e Avaliação de Empresas* (Atlas, 4ª ed., 2021)
- Póvoa: *Valuation: Como Precificar Ações*
- Rosenbaum & Pearl: *Investment Banking*
- Securato (FEA-USP): só foi confirmado o artigo de Oliveira, Guerreiro e Securato (RC&F/USP) sobre avaliação sob risco com o modelo de Ohlson, que gera um intervalo de valores em vez de um valor único. Nenhum livro-texto dele foi confirmado; verificar antes de citar.

## Diagnóstico de prontidão (site)

Questionário de 15 perguntas no `index.html` (seção `#diagnostico`), que roda no navegador e mede a **preparação da empresa para venda ou captação**. Não é avaliação da empresa nem recomendação de investimento, e não usa o `dcf()`.

- **Pontuação:** cada pergunta tem 4 opções que valem 0 a 3 pontos. Cada uma das 5 dimensões tem 3 perguntas, e a nota da dimensão é `pontos ÷ 9 × 100`. A nota geral é a média das 5 notas de dimensão (já arredondadas), com pesos iguais.
- **Dimensões:** informações financeiras; dependência de clientes e do dono; receita recorrente e margem; governança e documentos; crescimento e posicionamento.
- **Faixas:** 85 ou mais, pronta; 65 a 84, quase pronta; 40 a 64, em preparação; abaixo de 40, preparação necessária. Dimensões abaixo de 75 entram em "os pontos que mais pesam".
- **Prêmio de risco sugerido ao simulador:** 0 p.p. (nota 85 ou mais), 1 p.p. (65 a 84), 2 p.p. (40 a 64) ou 3 p.p. (abaixo de 40), somados à taxa de desconto atual. É uma **heurística** para testar o efeito de uma empresa menos preparada, editável pelo usuário. Não foi calibrada com dados de mercado.
- **Limites:** os critérios e as faixas são de boa prática de preparação, não um modelo estatístico validado. Perguntas, pesos e faixas ficam no objeto de configuração do script (`DIM`, `Q`, `FAIXAS`, `PREMIO`, `CRITICO`).
- **Pendente:** revisar com um profissional de M&A antes de cobrar pelo relatório completo.
