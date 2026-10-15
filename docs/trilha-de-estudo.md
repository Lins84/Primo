# Trilha de estudo evolutiva

**Status: proposta inicial (v0.1), 2026-10-15.**

Ordem de estudo por **dependência**, não por calendário. Avance ao nível seguinte quando cumprir o critério "Pronto quando" do atual, no seu ritmo. Níveis marcados como *opcional* só entram se o projeto exigir.

Esta trilha é uma versão enxuta do roteiro full stack (HTML, CSS, JavaScript, React, Node.js, APIs, banco de dados, testes, Git e GitHub, deploy), reordenada para o que o projeto de simuladores de EQ realmente usa.

## Como usar

- Marque `[x]` nos itens que você já domina.
- Cada nível termina com uma **prática no projeto**, para o estudo virar entrega.
- Com o Claude Code: peça para **explicar o básico antes de aceitar** o código, e peça um teste para cada mudança. A barra é entender o essencial de cada parte e entregar funcionando, como orientou o professor do TCC.

## Nível 1. Git e GitHub além do básico

- [ ] Commit com mensagem clara, `.gitignore`, ver histórico (`git log`, `git diff`)
- [ ] Ramos (`branch`), `merge` e resolução de conflito
- [ ] Desfazer com segurança (`restore`, `revert`) e por que evitar `push --force`
- [ ] Pull request e revisão do próprio código antes de juntar

**Pronto quando:** você cria um ramo, faz uma mudança, abre um pull request e junta sem apoio.
**Prática no projeto:** passar a mudar o `Primo` por ramos, e não direto na `main`.
**Libera:** trabalhar com segurança, publicar com automação.

## Nível 2. Python organizado

- [ ] Ambientes e dependências (`uv`, `pyproject.toml`), o que cada pacote faz
- [ ] Módulos e pacotes, `import`, estrutura de pastas
- [ ] Funções puras (entrada → saída, sem efeito escondido)
- [ ] Tipos e docstrings curtas; erros e exceções úteis

**Pronto quando:** você separa um cálculo em módulo próprio e o chama de outro arquivo.
**Prática no projeto:** estruturar `nucleo/` com um módulo de equações de estado (bloco B do mapa do site).
**Libera:** núcleo de cálculo reaproveitável pela interface e pelos testes.

## Nível 3. Testes

- [ ] Rodar testes com `pytest`
- [ ] Comparar números com tolerância (`pytest.approx`)
- [ ] Casos-limite (extremos de composição, valores zero)
- [ ] Testes parametrizados e gabaritos externos (NIST, livros, PPL da HP)

**Pronto quando:** cada função do núcleo tem teste contra um gabarito citável e os testes rodam com um comando.
**Prática no projeto:** converter os testes do TCC e escrever os do primeiro módulo (EOS).
**Libera:** confiança nos resultados e mudanças sem medo.

## Nível 4. Números e métodos numéricos

- [ ] `numpy` (vetores, operações em bloco)
- [ ] Raízes e equações não lineares (Newton, `scipy.optimize`)
- [ ] Integração e EDOs (`scipy.integrate`)
- [ ] Ajuste e regressão (mínimos quadrados)

**Pronto quando:** você resolve uma equação cúbica de estado e uma EDO simples e confere com um gabarito.
**Prática no projeto:** solver de volume de EOS e, depois, PFR e CSTR (bloco E).
**Libera:** reatores, trocadores de calor e transferência de massa.

## Nível 5. Interface em Flet

- [ ] Controles, layout e estado da tela
- [ ] Eventos (cliques, campos, tabelas)
- [ ] Gráficos (`flet-charts`)
- [ ] Manter a interface **sem lógica de cálculo**, só chamando o núcleo
- [ ] Publicar como site estático (`flet publish`) e medir tempo de carga

**Pronto quando:** uma tela chama um módulo do núcleo, mostra o resultado e um gráfico, e roda publicada.
**Prática no projeto:** primeiro simulador de termodinâmica (bloco B) no app único.
**Libera:** os simuladores do site.

## Nível 6. HTML, CSS e Markdown (vitrine do site)

- [ ] Estrutura de uma página (HTML semântico: títulos, listas, links)
- [ ] Estilo básico e layout responsivo (CSS)
- [ ] Noções de busca: título, descrição, cabeçalhos
- [ ] Gerador de site estático a partir de Markdown

**Pronto quando:** você publica uma página de texto, legível no celular, com título e descrição próprios.
**Prática no projeto:** a pasta `site/` do mapa, com uma página por simulador.
**Libera:** ser encontrado em buscadores.

## Nível 7. Automação e publicação

- [ ] GitHub Pages e domínio próprio
- [ ] Fluxo de trabalho do GitHub Actions (rodar testes e publicar ao juntar na `main`)
- [ ] Versões e anotações de mudança

**Pronto quando:** juntar um pull request roda os testes e atualiza o site sem passo manual.
**Prática no projeto:** trocar a publicação manual do TCC (cópia temporária e `push --force`) por automação.
**Libera:** manter vários simuladores sem trabalho repetido.

## Nível 8. APIs e HTTP (conceito)

- [ ] Requisição e resposta, métodos, códigos de status, JSON
- [ ] Ler a documentação de uma API e chamá-la com `requests`

**Pronto quando:** você consulta uma API pública e usa o resultado num script.
**Libera:** buscar dados externos e entender integrações.

## Nível 9. Segurança e boas práticas

- [ ] Nunca versionar senhas e chaves; variáveis de ambiente
- [ ] Atualizar dependências com critério
- [ ] Licença do código e aviso de uso educacional
- [ ] Cuidados com dados de usuários (se um dia houver)

**Pronto quando:** você revisa um repositório antes de publicar com essa lista.

## Níveis opcionais

Só entram se o projeto exigir. Hoje o Flet atende, e o `CLAUDE.md` do TCC registra a decisão de não sair dele.

**Opcional A. JavaScript, React e Node.js**
Úteis se for preciso sair do Flet por carga ou aparência, ou criar uma interface própria.

**Opcional B. Banco de dados (SQL e SQLite)**
Úteis só com contas de usuário, projetos salvos no servidor ou histórico de uso.

## Trilha paralela: HP Prime

Independente dos níveis acima. Consulte `HP-Prime_user_guide_EN_3ed.txt` (capítulo "Programming in HP PPL").

- [ ] Primeiro programa em PPL: variáveis, entrada e saída
- [ ] Menus (`CHOOSE`, `INPUT`) e laços (`FOR`, `WHILE`)
- [ ] Solver de Newton-Raphson em PPL
- [ ] Persistência (`EXPORT`, `AFiles`)
- [ ] Python na Prime (ver `python-hp-prime.md`)

## Visão geral das dependências

```
Git → Python organizado → Testes → Métodos numéricos → Flet → Publicação
                                                         └→ HTML/CSS (vitrine)
                                                         └→ Automação
```
