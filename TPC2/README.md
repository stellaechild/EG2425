---
Título: TPC2
Data: 15/02/2025
Autor: Maria Cunha e Tomás Campinho
UC: EG
---
# Conversão de uma Expressão para Lark

Durante a aula, os docentes apresentaram o desafio de expressar '[1.0:9.0][15.5:19.0]' em Lark.
Esta conversão pode ser dividida na definição de regras sintáticas e regras lexicográficas.

## Regras Sintáticas
Estas regras têm em atenção a estrutura da expressão que será convertida. Assim, definimos que esta expressão tem pelo menos uma estrutura 'intervalo'. Esta pode ser decomposta em quatro símbolos diferentes (correpondendo às regras lexicográficas), obtendo este aspeto : [num:num].

start: intervalo+        
intervalo: PE NUMERO ":" NUMERO PD   

## Regras Lexicográficas
Estas regras servem para esclarecer o que os símbolos utilizados para descrever as regras sintáticas significam. Assim, definimos um número como qualquer possível combinação de números inteiros, podendo ter partes decimais. Adicionalmente, utizamos as siglas PE, PD e DP para substituir os símbolos de estrutura da expressão, sendo estes respetivamente '[', ']' e ':'.

NUMERO: /\d+(\.\d+)?/   
PE: "["
PD: "]"
DP: ":"

## Output
Finalmente, passamos esta informação ao parser e apresentamos a informação de forma facilmente legível devido ao método _tree.pretty()_.
Assim, obtemos o seguinte output:

start
  intervalo
    [
    1.0
    9.0
    ]
  intervalo
    [
    15.5
    19.0
    ]

