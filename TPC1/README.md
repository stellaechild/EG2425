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
sentence: signal intervals DOT
intervals: interval+
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
Assim, obtemos o seguinte _output_, com a expressão de exemplo "- [1.0:9.0] [15.0:3.0].":

```txt
sentence
  signal	-
  intervals
    interval
      [
      num	1.0
      :
      num	9.0
      ]
    interval
      [
      num	15.0
      :
      num	3.0
      ]
  .

CC1 Error: interval [1.0:9.0] is in the wrong order according to the sentence signal '-'
CC2 Error: interval [15.0:3.0] starts with 15.0 but previous interval ends with 9.0

IsValid: False 
Number of intervals: 2 
Largest interval width: 12.0
```
