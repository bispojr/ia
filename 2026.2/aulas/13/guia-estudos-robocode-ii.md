# Guia de Estudos — Como vencer o Walls!

## Parte 2 — De sobreviver a vencer

**Objetivo da sessão:** acrescentar **percepção do adversário, tomada de decisão e ataque**, sem abandonar o controle do movimento.

---

### Desafio 11 — Onde está o inimigo?

Até agora seu robô estava essencialmente preocupado com a arena.

Agora ele precisa perceber o adversário.

**Perguntas:**

* Qual evento informa que outro robô foi detectado?
* Que informações o Robocode fornece?
* O que significa distância?
* O que significa *bearing*?
* O que significa *heading*?

**Missão:** faça seu robô conseguir responder:

> “Onde está o inimigo em relação a mim?”

---

### Desafio 12 — Encontrar não é acertar

Faça o robô apontar o canhão para o adversário.

Agora pergunte:

> **Se eu sei onde o inimigo está agora, onde ele estará quando meu tiro chegar?**

**Perguntas:**

* O inimigo está parado?
* Ele está se movimentando?
* Em qual direção?
* A que velocidade?
* Quanto tempo o tiro leva para chegar?

**Missão:** compare tiro direto com uma estratégia simples de antecipação.

---

### Desafio 13 — O inimigo também se movimenta

Coloque seu robô contra o `Walls` e observe.

**Perguntas:**

* O Walls fica parado?
* Qual é o padrão de movimento?
* É previsível?
* Ele muda de direção aleatoriamente?
* É possível aproveitar essa previsibilidade?

**Missão:** formular uma hipótese sobre onde o Walls estará no próximo instante.

---

### Desafio 14 — Atire somente quando fizer sentido

Não queremos simplesmente:

```text
fire();
fire();
fire();
```

sempre que possível.

**Perguntas:**

* Qual é o custo de errar um tiro?
* Quando vale a pena atirar?
* O que acontece com sua energia?
* É melhor atirar sempre ou esperar uma situação favorável?

**Missão:** criar uma estratégia simples para decidir **quando disparar**.

---

### Desafio 15 — Controle de fogo

Experimente diferentes potências de tiro.

**Perguntas:**

* Um tiro mais forte é sempre melhor?
* O que acontece quando você erra?
* Como a distância influencia sua decisão?
* Existe uma relação entre sua energia e a potência do tiro?

**Missão:** criar uma política simples de escolha da potência.

---

### Desafio 16 — Movimento e ataque são dois problemas diferentes?

Agora observe seu agente como um todo.

Ele precisa simultaneamente:

1. sobreviver;
2. localizar o adversário;
3. apontar;
4. atirar.

**Pergunta central:**

> O movimento ideal para sobreviver é necessariamente o movimento ideal para atacar?

Experimente alterar o comportamento de movimento **sem alterar o ataque**.

Depois faça o contrário.

**Missão:** identificar quais decisões devem ser independentes e quais precisam interagir.

---

### Desafio 17 — E se eu não souber onde ele está?

O radar pode perder o adversário.

**Perguntas:**

* O que acontece depois de `onScannedRobot`?
* O radar continua apontado para o inimigo?
* Como encontrar novamente um adversário perdido?
* O radar deve se movimentar independentemente do corpo do robô?

**Missão:** criar uma estratégia para **não perder o inimigo por muito tempo**.

---

### Desafio 18 — Ataque previsível também é uma fraqueza

Agora pense como o adversário.

Se seu robô fizer exatamente a mesma coisa em todas as batalhas:

> **um adversário poderia aprender seu comportamento?**

Teste diferentes padrões de movimento.

**Perguntas:**

* O movimento é previsível?
* O robô sempre faz as mesmas curvas?
* Sempre passa pelos mesmos lugares?
* Existe alguma pequena variação que dificulte ser atingido?

**Missão:** introduzir uma variação controlada no comportamento de movimento.

---

### Desafio 19 — O que realmente está fazendo você perder?

Agora faça várias batalhas:

```text
SeuRobô × Walls
```

Não altere o código imediatamente.

Construa uma pequena tabela:

| Batalha | Como morreu? | Problema observado |
| ------- | ------------ | ------------------ |
| 1       |              |                    |
| 2       |              |                    |
| 3       |              |                    |
| 4       |              |                    |
| 5       |              |                    |

Depois procure padrões.

**Pergunta central:**

> Qual é a causa mais frequente das suas derrotas?

**Missão:** escolha **uma única melhoria** baseada nos dados das batalhas.

---

### Desafio 20 — Então... como vencer o Walls?

Agora retire as restrições dos desafios anteriores.

Você pode modificar:

* movimento;
* radar;
* mira;
* potência dos tiros;
* estratégia de ataque;
* velocidade;
* comportamento defensivo.

Mas existe uma regra:

> **Toda alteração precisa ter uma hipótese por trás.**

Antes de modificar:

> “Eu acho que estou perdendo porque ________.
> Minha hipótese é que ________.
> Para verificar isso, vou ________.”

Depois da batalha:

> “O resultado foi ________.
> Minha hipótese estava ________.
> A próxima coisa que vou investigar é ________.”

### Desafio final

Faça seu robô enfrentar o `Walls` em várias batalhas.

Seu objetivo não é simplesmente obter **uma vitória**.

O objetivo é construir um agente que apresente **um comportamento consistente**.

Ao final, responda:

1. **Como meu robô percebe o ambiente?**
2. **Quais informações ele utiliza para tomar decisões?**
3. **Quais decisões são reativas?**
4. **Quais decisões dependem de alguma previsão?**
5. **Como ele equilibra sobrevivência e ataque?**
6. **Qual é a principal hipótese que orienta sua estratégia?**
7. **Que evidências tenho de que minha estratégia funciona?**