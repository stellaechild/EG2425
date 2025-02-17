---
Título: TPC2
Data: 15/02/2025
Autor: Maria Cunha e Tomás Campinho
UC: EG
---
# Conversão de uma Expressão para Lark

Durante a aula, os docentes apresentaram o desafio de reproduzir a seguinte gramática em Lark:

---
```txt
Terminal: { '.', ';', '[', ']', num }
Non-Terminal: { S, Is, RI, I }
Production Rules (P):

p1: Sentence: Signal Intervals '.'

p2: Signal: '+'

p3: Signal: '-'

p4: Intervals: Interval

p5: Intervals: Intervals Interval

p6: Interval: '[' num ':' num ']'

CC1: p[4] > p[2] &
CC2: p[2] >= parser.anterior
parser.anterior = p[4]
parser.erro = not (CC1) or not (CC2)
```

---
## Regras Sintáticas

Estas regras têm em atenção a estrutura da expressão que será convertida. Assim, definimos que esta expressão tem pelo menos uma estrutura 'intervalo'. Esta pode ser decomposta em cinco símbolos diferentes (correpondendo às regras lexicográficas), obtendo este aspeto : +/- [num:num].

```python
sentence: signal interval+ DOT
signal: PLUS
      | MINUS
interval: PE num COL num PD  
```

## Regras Lexicográficas

Estas regras servem para esclarecer o que os símbolos utilizados para descrever as regras sintáticas significam. Assim, definimos um número como qualquer possível combinação de números inteiros, podendo ter partes decimais. Adicionalmente, utizamos as siglas PE, PD, COL, DOT, MINUS e PLUS para substituir os símbolos de estrutura da expressão, sendo estes respetivamente '[', ']', ':', '.', '-' e '+'.

```python
num: /\d+(\.\d+)?/
PLUS: "+"
MINUS: "-"
DOT: "."
PE:"["
PD:"]"
COL:":"
```

## Output

Finalmente, passamos esta informação ao parser e apresentamos a informação de forma facilmente legível devido ao método _tree.pretty()_.
Assim, obtemos o seguinte _output_, com a expressão de exemplo '+   [ 20.0:9.0 ]  [1.5 :19.0] .' (de modo a testar, também as mensagens de erro - CC1 e CC2):

```python
sentence
  signal	+
  interval
    [
    num	20.0
    :
    num	9.0
    ]
  interval
    [
    num	1.5
    :
    num	19.0
    ]
  .

CC1 Error: interval [20.0:9.0] - end bound (9.0) must be greater than start bound (20.0).
CC2 Error: interval [1.5:19.0] - start bound (1.5) must be greater than or equal to the previous end bound (9.0).

Incorrect expression!

```
