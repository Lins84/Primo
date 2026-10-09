# Python na HP Prime: notas da comunidade

A HP não publicou documentação oficial de Python para a Prime. O manual de 2017 (3a edição) e o de 2015 não mencionam Python. Este resumo foi escrito em palavras próprias, a partir de duas fontes da comunidade. Ele não substitui testar no aparelho: as fontes são de 2023 a 2025 e o firmware muda.

## Fontes

- Mike Markowski, "HP Prime Programming": https://udel.edu/~mm/hp/primePython/ (firmware 2.1.14730, abril de 2023)
- Fórum do Museu HP, "Introduction to Python on the HP Prime" (julho de 2025): https://www.hpmuseum.org/forum/thread-23774.html
  - O primeiro post anexa um guia em PDF, "HP Prime Python Activities.pdf", que não foi baixado para este repositório.

## O que se sabe

- O interpretador é MicroPython. A lista de módulos e funções aparece no próprio aparelho, no menu CMDS do app Python.
- O módulo `hpprime` liga o Python ao PPL. `hpprime.eval('<comando PPL>')` executa comandos PPL que não têm equivalente em Python, como menus e caixas de diálogo.
- NumPy não existe. Matrizes precisam usar o `linalg.matrix` do próprio aparelho.

## Três formas de usar Python

1. **Programa PPL com bloco Python.** O código Python fica entre `#PYTHON nome` e `#END`, e uma função PPL exportada chama `PYTHON(nome);`.

   ```
   #PYTHON meuBloco
     print('Python aqui')
   #END

   EXPORT minhaFuncao()
   BEGIN
     PYTHON(meuBloco);
   END;
   ```

2. **App derivado do app Python.** Salva-se o app Python com outro nome e adicionam-se os arquivos pelo Connectivity Kit. Desvantagem: é preciso tocar em "Clear" antes de rodar, e as funções START, RESET, Info, Symb, Plot e Num do PPL não funcionam do mesmo jeito.

3. **App do tipo "None" com `#PYTHON START()`.** Segundo o usuário komame, no fórum, dá um app que roda só em Python, sem o toque em "Clear", e permite importar módulos próprios. Desvantagem: o `#PYTHON` sempre reserva 1 MB de memória.

## Interface com o usuário (via PPL)

- Menu: `h.eval('X:=0;choose(X,"Titulo","Opcao 1","Opcao 2")')`. O resultado começa em 1.
- Mensagem: `h.eval('msgbox("texto")')`.
- Caixa de entrada: `input()` do PPL. Os valores voltam em variáveis do Python. No teste do autor, o valor `[-1]` do manual (aceita qualquer tipo) deu erro, e `[0]` (só reais) funcionou.
- Menu de botões: `drawmenu(...)` do PPL. A tela tem 320x240 pixels e cada botão de rodapé tem 53x20 pixels.
- Toques e teclas: `h.eval('mouse')` devolve os dois dedos, e `h.eval('getkey')` devolve a tecla (-1 se nenhuma). Use `wait(0.1)` em laços de leitura para limitar a frequência.

## Trocar dados entre PPL e Python

- Variável global do PPL vista pelo Python: `eval('Programa.variavel')`. Quebra se o programa for renomeado.
- Variável local: converter para texto e passar no código. Prefira `str()` do Python em vez de `STRING()` do PPL, porque os formatos numéricos do HOME podem causar erro de sintaxe. Alternativa citada: `STRING(valor,1,0,{3,"."})`.
- Em apps, `AVars("nome")` leva resultados para fora do app (HOME e CAS).
- Bug citado: no HOME, `2e+1` dá erro e `2e1` funciona. Contorno: `import cas` e `cas.caseval('AVars("nome"):=valor')`.
- Em PPL, a variável `Apps` devolve a lista de apps, e o primeiro item é o app ativo.

## Quando usar cada linguagem

- Um usuário do fórum afirma que Python é bem mais rápido que PPL. Isso não foi medido aqui.
- O PPL continua sendo a base documentada nos manuais deste repositório. Python entra se houver necessidade de velocidade ou de lógica mais complexa.
