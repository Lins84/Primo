# Mapa do site de simuladores e calculadoras de Engenharia Química

**Status: proposta inicial (v0.1), 2026-10-10.**

Documento de planejamento. **Tudo aqui é proposta**: nenhuma decisão está tomada até o autor validar. O site entra nas atualizações futuras do TCC, depois da entrega.

## 1. Ponto de partida (o que já existe)

| Peça | Estado | Observação |
|---|---|---|
| Equilíbrio de fases Gᴱ (TCC) | App Flet integrado, publicado no GitHub Pages | 7 modelos de Gᴱ escritos à mão, banco IPDB, regressão de Barker, validação contra NIST ThermoML |
| Suíte de termodinâmica (HP Prime) | 4 apps em PPL, publicados no repositório e no hpcalc.org | Termo_Titanium, Termo_Mix, Termo_Liq_Pro, Termo_Gen_Plus |
| Tempo de carga do app estático | Medido pelo autor | cerca de 18 s no PC, cerca de 40 s no celular com dados móveis; servidor Python recarrega em cerca de 2 s |

## 2. Blocos do site e módulos

Ordem dentro de cada bloco = prioridade sugerida.

### Bloco A. Equilíbrio de fases (existente)
- A1. VLE isotérmico (P-x-y) com modelos de Gᴱ. **Pronto no TCC.**
- A2. VLE isobárico (T-x-y). Adiado no TCC.
- A3. Flash isotérmico. Novo.

### Bloco B. Termodinâmica de fluidos (vem da suíte HP)
- B1. Equações de estado de fluido puro: gás ideal, van der Waals, Redlich-Kwong, SRK, Peng-Robinson, Virial. Calcula P, T ou V, fator Z, β e κ.
- B2. Misturas binárias: SRK e Peng-Robinson com regras de mistura de van der Waals (um fluido) e kij.
- B3. Densidade de líquidos: Rackett (Yamada-Gunn e com Vc real) e COSTALD, com solver inverso.
- B4. Diagramas generalizados: Lee-Kesler e regra de Kay.

### Bloco C. Trocadores de calor (Operações Unitárias 2)
- C1. LMTD: contracorrente e correntes paralelas, fator F para casco e tubos.
- C2. ε-NTU.
- C3. Dimensionamento casco-tubo pelo método de Kern: coeficientes de película, coeficiente global, área, número de tubos.
- C4. Perda de carga nos tubos e no casco, com incrustação.
- C5. Perfis de temperatura e comparação de arranjos.

### Bloco D. Transferência de massa (Operações Unitárias 3)
Depende da ementa; tópicos prováveis.
- D1. McCabe-Thiele para destilação binária. Reaproveita o VLE do bloco A.
- D2. Absorção e esgotamento (equação de Kremser).
- D3. Extração líquido-líquido (diagrama ternário).

### Bloco E. Reatores e modelagem
Depois que o núcleo estiver estável.
- E1. PFR e CSTR, isolados e em associação.
- E2. Distribuição de tempos de residência (RTD).
- E3. Resolvedor de EDOs (Runge-Kutta), base para E1 e E2.

## 3. Ordem sugerida de construção

1. **Onda 0 (agora):** concluir o TCC. Este documento fica só como planejamento.
2. **Onda 1:** núcleo de cálculo compartilhado + bloco B. É a primeira porque o código já existe em PPL e está validado, então a conversão para Python é direta.
3. **Onda 2:** bloco C. Fórmulas fechadas e fáceis de conferir com exercícios de livro.
4. **Onda 3:** bloco D, aproveitando o bloco A.
5. **Onda 4:** bloco E.

## 4. Arquitetura proposta

```
nucleo/   Python puro: equações, sem interface
testes/   gabaritos e validações
apps/     interface Flet, uma tela por simulador
site/     páginas de texto (teoria, exemplos, links)
```

- O núcleo não conhece a interface. Trocar a interface depois não exige refazer cálculo.
- Seguir o método do TCC: as equações são escritas no projeto, e a biblioteca `thermo` serve de fonte de dados e de gabarito.
- Cada módulo nasce com teste. Gabaritos possíveis:

| Bloco | Gabarito |
|---|---|
| B | resultados dos apps em PPL (validados contra Perry e NIST), `thermo` |
| C | exercícios resolvidos de Kern e Incropera |
| D | exemplos resolvidos de livro-texto de operações de transferência de massa |
| E | soluções analíticas (ordem 1 e 2) |

## 5. Pontos de atenção

- **Tempo de carga:** 15 a 40 s por abertura num app estático. Preferir um app único com vários simuladores a um app por calculadora, para pagar a carga uma vez.
- **Busca:** o conteúdo do Flet no navegador não é indexado. A pasta `site/` com texto resolve isso.
- **Atualização manual:** a publicação hoje é manual (cópia temporária e envio ao `gh-pages`). Um workflow do GitHub Actions evitaria repetir.
- **Uso educacional:** manter o aviso de que os resultados servem para estudo, como já está nos repositórios existentes.

## 6. Decisões em aberto (do autor)

1. Repositório novo ou continuar no `Primo`; nome do projeto e endereço.
2. Licença do código.
3. App único ou vários.
4. Hospedagem: GitHub Pages ou institucional (STI/UFC, RNP).
5. Idioma do site: só português ou português e inglês.
6. Ementa de Operações Unitárias 3 e de Reatores/Modelagem, para fechar os blocos D e E.
7. Quando começar: depois da entrega do TCC, conforme o registro nas atualizações futuras.
