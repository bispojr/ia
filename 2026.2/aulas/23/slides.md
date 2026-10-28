---
theme: default
title: 'Capítulo 1 — Iniciando com Prolog'
info: |
  Capítulo 1 — Iniciando com Prolog
  Prof. Esdras Lins Bispo Jr. | bispojr@ufj.edu.br
  Inteligência Artificial
  Bacharelado em Ciência da Computação (BCC)
  28 de outubro de 2026
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
---

<style>
:global(.slidev-layout) {
  --accent: #1f5aa6;
  --accent-soft: #dbeafe;
}

:global(.slidev-layout h1),
:global(.slidev-layout h2) {
  color: var(--accent);
}

:global(.title-accent) {
  color: var(--accent);
}

:global(.title-panel) {
  border-left: 6px solid var(--accent);
  background: linear-gradient(90deg, var(--accent-soft) 0%, rgba(255, 255, 255, 0) 72%);
  padding-left: 1.5rem;
}

:global(.title-logo) {
  background: white;
  border: 1px solid var(--accent-soft);
  border-radius: 1rem;
  box-shadow: 0 12px 30px rgba(31, 90, 166, 0.12);
  padding: 1rem;
}

:global(.title-meta) {
  color: #24508b;
}
</style>

<div class="grid grid-cols-[1fr_auto] items-center gap-8 py-10 title-panel">

<div>

# Capítulo 1
## Iniciando com Prolog

**<span class="title-accent">Prof. Esdras Lins Bispo Jr.</span>**  
<span class="title-meta">bispojr@ufj.edu.br</span>

<span class="title-meta">Inteligência Artificial</span>  
<span class="title-meta">Bacharelado em Ciência da Computação (BCC)</span>

<span class="title-meta">28 de outubro de 2026</span>

</div>

<div class="flex items-center justify-center title-logo">

<img
  src="/images/ufjJataiLogo.png"
  alt="Logotipo da UFJ"
  style="width: 180px; max-width: 22vw; height: auto;"
/>

</div>

</div>

---

# Objetivos do Capítulo

Após este capítulo, você será capaz de:

- Escrever e carregar um programa simples em Prolog
- Inserir objetivos no prompt do sistema Prolog (`?-`)
- Compreender a terminologia básica da linguagem
- Distinguir os diferentes tipos de dado (*termos*)

---
layout: section
---

# 1.1 Iniciando o Prolog

---

# O Prompt do Sistema

Ao iniciar o Prolog, o sistema exibe o **prompt**:

```
?-
```

O prompt indica que o Prolog está **pronto para receber objetivos** (*goals*).

- Cada sequência de objetivos deve ser encerrada com um **ponto final** (`.`)
- A linha só é enviada ao sistema após pressionar **Enter**

> Em diferentes versões do Prolog, o prompt pode variar ligeiramente.

---

# A Primeira Consulta

O predicado `write/1` exibe um termo na saída. O predicado `nl/0` imprime uma nova linha.

<PrologREPL initialCode="write('Olá mundo'),nl,write('Bem-vindo ao Prolog'),nl." />

> Edite o código acima e clique **Executar** para experimentar.

---

# Por Dentro da Consulta

A sequência de objetivos abaixo contém **quatro objetivos** separados por vírgulas (que significam **"e"**):

```prolog
?- write('Olá mundo'), nl, write('Bem-vindo ao Prolog'), nl.
```

| Objetivo | Efeito |
|---|---|
| `write('Olá mundo')` | Exibe `Olá mundo` na tela |
| `nl` | Imprime uma nova linha |
| `write('Bem-vindo ao Prolog')` | Exibe `Bem-vindo ao Prolog` na tela |
| `nl` | Imprime uma nova linha |

Para a sequência **ser bem-sucedida**, cada objetivo é satisfeito em ordem. O sistema responde `true`.

---

# Objetivos e Efeitos Colaterais

Do ponto de vista do sistema Prolog, o que importa é se a sequência de objetivos **é satisfeita ou não**.

- A geração de texto na tela (`write`, `nl`) é um **efeito colateral** (*side effect*)
- O `true` ao final indica que a sequência **teve sucesso**

> Os predicados `write` e `nl` têm significados **pré-definidos** pelo sistema. Eles são chamados de **predicados embutidos** (*built-in predicates — BIPs*).

> Uma sequência de objetivos inserida no prompt é frequentemente chamada de **consulta** (*query*).

---
layout: section
---

# 1.2 Programas Prolog

---

# Criando um Programa Prolog

A forma usual de trabalhar é:

1. **Criar** o programa em um editor de texto e salvar como `.pl`
2. **Carregar** o programa com o predicado `consult`
3. **Consultar** a base de dados com sequências de objetivos

```prolog
?- consult('prog1.pl').
```

Se o arquivo existir e for sintaticamente correto, o sistema confirma:

```
# 0.00 seconds to consult prog1.pl
```

> As cláusulas carregadas ficam no **banco de dados Prolog** (*Prolog database*) até o usuário sair do sistema.

---

# Estrutura de um Programa

```prolog
cachorro(fido).
gato(felix).
animal(X) :- cachorro(X).
```

Este programa tem **três cláusulas**, cada uma terminada por ponto final.

| Linha | Tipo | Leitura natural |
|---|---|---|
| `cachorro(fido).` | **Fato** | "fido é um cachorro" |
| `gato(felix).` | **Fato** | "felix é um gato" |
| `animal(X) :- cachorro(X).` | **Regra** | "X é um animal **se** X é um cachorro" |

---

# Fatos e Regras em Detalhe

**Fatos** — afirmações incondicionalmente verdadeiras:

```prolog
cachorro(fido).    % "fido é um cachorro"
gato(felix).   % "felix é um gato"
```

**Regras** — a cabeça é verdadeira **se** o corpo for satisfeito:

```prolog
animal(X) :- cachorro(X).
```

- O símbolo `:-` lê-se **"se"**
- `X` é uma **variável** — representa qualquer valor, mas o mesmo nos dois lados

> A partir do fato `cachorro(fido)` e da regra acima, o Prolog pode deduzir que `animal(fido)` é verdadeiro.

---

# Consultando o Banco de Dados

<PrologREPL
  :program="`cachorro(fido).\ngato(felix).\nanimal(X) :- cachorro(X).`"
  initialCode="animal(fido)."
/>

> Experimente também: `animal(felix).` — e observe a diferença no resultado.

---

# Sucesso e Falha

Dizemos que um objetivo **é satisfeito** (*succeeds*) ou **falha** (*fails*):

```prolog
?- animal(fido).
true              % há evidência de que fido é um cachorro → animal

?- animal(felix).
false               % felix é gato, mas não há regra que o torne animal
```

Um objetivo pode ser interpretado como:
- **Comando**: `?- halt.` → *"Sair do Prolog"*
- **Pergunta**: `?- animal(fido).` → *"fido é um animal?"*

---

# Programa 1 - Animais

```prolog
cachorro(fido).    cachorro(rover).
cachorro(tom).     cachorro(henry).
gato(maria).    gato(harry).
gato(bill).    gato(steve).
```

Quatro cláusulas definem o predicado `cachorro` e quatro definem `gato`.

```prolog
?- cachorro(fido).
true

?- cachorro(daisy).
false
```

---

# Variáveis em Consultas

Quando uma consulta contém uma **variável**, o Prolog busca um valor que a satisfaça:

<PrologREPL
  :program="`cachorro(fido).\ncachorro(rover).\ncachorro(tom).\ncachorro(henry).\ngato(maria).\ngato(harry).\ngato(bill).\ngato(steve).`"
  initialCode="cachorro(X)."
/>

> O componente retorna a **primeira** solução. No Prolog interativo, pressionar `;` força o sistema a buscar a próxima.

---

# Retrocesso — *Backtracking*

Quando há múltiplas soluções para um objetivo, o Prolog usa **retrocesso** (*backtracking*):

```prolog
?- cachorro(Y).
Y = fido ;      ← usuário pressiona ;
Y = rover ;     ← usuário pressiona ;
Y = tom ;       ← usuário pressiona ;
Y = henry       ← sem mais soluções
```

- O nome da variável (`X` ou `Y`) é indiferente
- O Prolog sempre encontra soluções **na ordem em que as cláusulas foram carregadas**

<!--> O retrocesso será discutido em detalhe no Capítulo 3.-->

---

# Combinando Objetivos com Variáveis

<PrologREPL
  :program="`cachorro(fido).\ncachorro(rover).\ncachorro(tom).\ncachorro(henry).\ngato(maria).\ngato(harry).\ngato(bill).\ngato(steve).`"
  initialCode="gato(X), cachorro(Y)."
/>

> Esta consulta retorna a **primeira** combinação possível de gato e cachorro.  
> Experimente também: `gato(X), cachorro(X).` — por que o resultado é diferente?

---

# Mesma Variável, Mesmo Valor

```prolog
?- gato(X), cachorro(Y).   % X e Y são independentes → combinações livres
X = maria, Y = fido

?- gato(X), cachorro(X).   % X deve ser ao mesmo tempo gato E cachorro
false
```

> A **mesma variável** em dois objetivos deve ser o **mesmo valor**. Não há animal que satisfaça `gato(X)` e `cachorro(X)` simultaneamente nessa base.

---

# O Predicado `listing`

```prolog
?- listing(cachorro/1).
```

Lista todas as cláusulas que definem o predicado `cachorro`, na ordem em que foram carregadas:

```prolog
cachorro(fido).
cachorro(rover).
cachorro(tom).
cachorro(henry).

true
```

---
layout: section
---

# 1.3 Objetos de Dados: Termos

---

# Tipos de Termo em Prolog

Os objetos de dados em Prolog são chamados de **termos** (*terms*):

| Tipo | Exemplos |
|---|---|
| **Inteiro** | `42`, `-7`, `0`, `+5` |
| **Real** | `3.14`, `-.5`, `+256.0` |
| **Átomo** | `fido`, `'Olá mundo'`, `>=` |
| **Variável** | `X`, `Nome`, `_Temp` |
| **Termo composto** | `cachorro(fido)`, `gosta(paul, prolog)` |
| **Lista** | `[1, 2, 3]`, `[]`, `[a, [b, c]]` |

---

# (1) Números

**Inteiros** — sequência de dígitos com sinal opcional:

```prolog
623    -47    +5    025
```

**Reais** — contêm um único ponto decimal:

```prolog
6.43    -.245    +256.0
```

---

# (2) Átomos

Átomos são **constantes sem valor numérico**. Três formas de escrita:

**(a) Sequência de letras/dígitos/_underscore_** começando com **minúscula**:

```prolog
joao    today_is_Tuesday    fred_jones    a32_BCD
```

**(b) Qualquer sequência entre **aspas simples**** (permite espaços e maiúsculas):

```prolog
'Today is Tuesday'    'today-is-Tuesday'    'Olá mundo'
```

**(c) Sequência de **caracteres especiais**:**

```prolog
+++    >=    >    +--    :-    =..
```

---

# (3) Variáveis

Em uma consulta, uma variável representa um **termo a ser determinado** pelo sistema.

O nome de uma variável começa com **letra maiúscula** ou **_underscore_**:

```prolog
X          Author
Pessoa_A   _123A
```

**Não são** variáveis (começam com minúscula ou dígito):

```prolog
author    45_ABC    pessoa-A
```

> A variável `_` (_underscore_ sozinho) é chamada de **variável anônima** e tem propósito especial <!--(Capítulo 2)-->.

---

# (4) Termos Compostos

Um termo composto começa com um **funtor** (átomo), seguido de **argumentos** entre parênteses:

```
functor(t1, t2, …, tn)    n ≥ 1
```

O número de argumentos é a **aridade** (*arity*).

```prolog
gosta(paul, prolog)                          % aridade 2
cachorro(henry)                                   % aridade 1
pessoa('joao smith', 32, doutor, londres)     % aridade 4
gosta(cachorro(henry), Y)                         % argumento composto
```

> Cada argumento pode ser **qualquer tipo de termo**, inclusive outro termo composto.

---

# (5) Listas

Uma lista é uma sequência de **elementos** (qualquer tipo de termo) entre colchetes:

```prolog
[cachorro, gato, peixe, homem]
[cachorro, gato, y, meupred(A,b,c), [p,q,R], z]
[[joao, 28], [maria, 56, professor], roberto]
```

A **lista vazia** é `[]`.

- Ao contrário da aridade de termos compostos, o número de elementos **não precisa ser definido antecipadamente**
- Um elemento pode ser qualquer termo, inclusive outra lista

<!--> As listas serão exploradas em detalhe no Capítulo 9.-->

---

# Átomos e Termos Compostos: *Call Terms*

**Átomos** e **termos compostos** têm importância especial nas cláusulas Prolog — são coletivamente chamados de **termos de chamada** (*call terms*).

```prolog
% Átomos usados como predicados (aridade 0):
true.
fail.

% Termos compostos como cabeças de cláusula:
cachorro(fido).
animal(X) :- cachorro(X).
gosta(paul, prolog).
```

> Voltaremos a este conceito quando estudarmos a execução de programas.

---

# Resumo do Capítulo 1

- O Prolog usa o prompt `?-` para receber **consultas** (sequências de objetivos)
- Programas são compostos de **fatos** e **regras**, carregados com `consult`
- Um objetivo **é satisfeito** (*true*) ou **falha** (*false*)
- **Variáveis** permitem buscar valores — o Prolog usa **retrocesso** para encontrar todas as soluções
- Os objetos de dados são chamados de **termos**: números, átomos, variáveis, termos compostos e listas

<!--**Próximo:** Capítulo 2 — Unificação e Variáveis em Detalhe-->
