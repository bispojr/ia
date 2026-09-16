# Guia de Estudos — Como vencer o Walls!

## Parte 1 — Conhecendo o inimigo

**Objetivo da sessão:** sair de um robô que simplesmente se movimenta para um agente capaz de **perceber o ambiente, formular uma estratégia e controlar seu movimento**.

### Desafio 1 — O que o Walls realmente faz?

Execute o `Walls` várias vezes e observe seu comportamento.

**Perguntas para investigação:**

* O robô parece tomar decisões diferentes durante a batalha?
* Que comportamento ele repete?
* Ele precisa “saber” onde está?
* O que aconteceria se ele simplesmente mantivesse uma mesma velocidade e direção?

**Missão:** descreva o comportamento do Walls em suas próprias palavras, sem olhar seu código.

---

### Desafio 2 — Como saber onde estou?

Seu robô precisa descobrir sua posição na arena.

Investigue os eventos e métodos disponíveis no Robocode.

**Perguntas:**

* Como descobrir a coordenada `X`?
* Como descobrir a coordenada `Y`?
* O que representa `(0,0)`?
* A posição muda continuamente ou apenas quando algum evento acontece?

**Missão:** faça seu robô informar sua posição durante a batalha.

---

### Desafio 3 — Onde estão as paredes?

Agora pense como um agente:

> **Como o robô poderia saber que está se aproximando de uma parede?**

Não implemente ainda uma solução sofisticada.

**Perguntas:**

* Sabendo `X`, `Y`, largura e altura da arena, é possível calcular a distância até cada parede?
* Qual parede está mais próxima?
* O tamanho do próprio robô interfere?

**Missão:** faça o robô identificar qual das quatro paredes está mais próxima.

---

### Desafio 4 — Não bata na parede!

Crie um comportamento que faça o robô mudar sua trajetória antes de atingir uma parede.

**Perguntas:**

* É melhor reagir depois da colisão ou antes?
* Qual distância de segurança seria razoável?
* Uma distância fixa funciona em todos os lados?
* O que acontece se o robô estiver indo na direção contrária à parede?

**Missão:** seu robô deve conseguir permanecer na arena sem depender de `onHitWall`.

---

### Desafio 5 — O problema da direção

Agora observe atentamente:

> Saber qual parede está próxima é suficiente para saber **para onde virar**?

Considere a direção atual do robô.

**Perguntas:**

* Se estou perto da parede norte, devo sempre virar para o mesmo lado?
* O que muda se estiver me deslocando para leste?
* Como transformar “quero ir para lá” em uma rotação?

**Missão:** faça o robô escolher uma nova direção de movimento de maneira coerente com sua posição atual.

---

### Desafio 6 — O robô está realmente patrulhando?

Faça seu robô percorrer a arena tentando acompanhar as paredes.

Agora observe.

**Perguntas:**

* Ele realmente acompanha uma parede?
* Ele fica preso em algum canto?
* Ele faz curvas muito fechadas?
* Ele consegue completar uma volta?

**Missão:** conseguir realizar uma trajetória aproximadamente retangular pela arena.

---

### Desafio 7 — Quanto devo virar?

Seu robô provavelmente consegue acompanhar as paredes, mas talvez esteja fazendo curvas ruins.

Investigue a relação entre:

* direção atual;
* direção desejada;
* ângulo de giro.

**Pergunta central:**

> Como calcular o menor giro necessário para transformar uma direção em outra?

**Missão:** criar uma função ou mecanismo que permita ao robô escolher um giro adequado em diferentes situações.

---

### Desafio 8 — E se eu mudar a velocidade?

Experimente velocidades diferentes.

**Perguntas:**

* Um robô mais rápido faz curvas melhores ou piores?
* O que acontece quando ele chega muito rápido a uma parede?
* Existe uma velocidade que torna o controle mais previsível?
* É possível reduzir a velocidade durante uma curva e aumentá-la depois?

**Missão:** encontre uma política de velocidade que torne o movimento do robô mais estável.

---

### Desafio 9 — O Walls está tentando fazer o quê?

Agora volte a observar o `Walls`.

Compare:

> **seu robô × Walls**

**Perguntas:**

* O que o seu robô ainda faz pior?
* O que parece ser essencial para o comportamento do Walls?
* O Walls precisa saber onde está o inimigo para realizar seu movimento?
* Qual é a estratégia de movimento dele?

**Missão:** escreva uma hipótese sobre **por que o Walls consegue sobreviver**.

---

### Desafio 10 — Primeiro confronto

Coloque seu robô contra:

```text
SeuRobô × Walls
```

Faça várias batalhas.

Não tente simplesmente “consertar até ganhar”.

Registre:

* Como seu robô morreu?
* Em que situação?
* O comportamento foi determinístico?
* Qual foi a principal falha?

### Fechamento da Parte 1

Ao final da primeira sessão, você deverá ter um agente que:

**percebe → calcula → decide → movimenta**

E a pergunta para levar para a segunda sessão será:

> **“Se o Walls consegue sobreviver sem necessariamente procurar o inimigo, como posso transformar meu robô em um agente que, além de sobreviver, consiga derrotá-lo?”**
