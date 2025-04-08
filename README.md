# Analisador Sintático para Expressões Matemáticas

## Descrição da Gramática
A gramática GRUPO 5 é uma gramática livre de contexto que define expressões combinando operadores aritméticos (+, -, *, /) e relacionais (>, <), organizados em uma hierarquia de precedência onde parênteses têm a maior precedência, seguidos por operadores relacionais, multiplicativos e, finalmente, aditivos. As produções utilizam recursão à esquerda (E → E + T | T, T → T * F | F, etc.) para garantir associatividade à esquerda, sendo adequada para análise sintática de expressões em compiladores ou interpretadores, embora sua forma recursiva a torne inerentemente ambígua, exigindo ajustes para uso prático em analisadores descendentes. A gramática aceita identificadores (id) e expressões entre parênteses como átomos básicos, permitindo construções como (a + b) * c > d, mas não inclui operadores de igualdade, unários ou literais numéricos sem extensões adicionais.

## Conjunto First e Follow

| Não-Terminal | FIRST          | FOLLOW                     |
|--------------|----------------|----------------------------|
| **E**        | `{ id, ( }`    | `{ $, +, -, ), >, < }`     |
| **T**        | `{ id, ( }`    | `{ $, +, -, *, /, ), >, < }`|
| **F**        | `{ id, ( }`    | `{ $, +, -, *, /, ), >, < }`|
| **G**        | `{ id, ( }`    | `{ $, +, -, *, /, ), >, < }`|



## Implementação
Visão Geral
Este pacote implementa um analisador sintático descendente recursivo para uma linguagem de expressões simples. O parser constrói uma Árvore Sintática Abstrata (AST) a partir de expressões de entrada válidas e fornece relatórios de erro detalhados para sintaxe inválida.

Funcionalidades:
- Tokenização de strings de entrada em componentes significativos
- Análise de expressões aritméticas com operadores +, -, *, /, <, >
- Suporte a parênteses para agrupamento de expressões
- Reconhecimento de identificadores alfanuméricos
- Construção de representação em Árvore Sintática Abstrata (AST)
- Inclui casos de teste

Componentes Principais
Tokenizador: Divide a string de entrada em tokens usando padrões regex

Analisador Sintático: Implementa análise descendente recursiva com métodos para cada regra gramatical

Nó da AST: Classe interna que representa nós na árvore sintática abstrata

Métodos Principais
tokenize(): Converte a string de entrada em tokens

parse(): Ponto de entrada para análise, retorna o nó raiz da AST

E(), EPrime(), T(), TPrime(), F(), FPrime(), G(): Métodos recursivos que implementam as regras gramaticais

peek(), consume(), match(): Métodos auxiliares para manipulação de tokens

## Casos de Testes:

##  Casos de Teste Válidos 
| Expressão                            | Resultado  | Observações                                                                 |
|-------------------------------------|------------|------------------------------------------------------------------------------|
| `a + b`                             | Válido  | Soma simples                                                                 |
| `a * b < c`                         | Válido  | Comparação após operação de multiplicação                                    |
| `(a + b) * c - d / e > f`           | Válido  | Expressão complexa com parênteses e múltiplos operadores                     |
| `a < b + c * d`                     | Válido  | Precedência correta: `*`, depois `+`, depois `<`                             |
| `a * (b + c)`                       | Válido  | Parênteses alterando precedência                                             |
| `x + y * z - w / v`                 | Válido  | Expressão aritmética extensa                                                 |
| `(a < b) + (c > d)`                 | Válido  | Expressões relacionais agrupadas                                             |
| `a`                                 | Válido  | Identificador isolado                                                        |
| `(a)`                               | Válido  | Identificador dentro de parênteses                                           |
| `a + b * c - d / e < f + g`         | Válido  | Expressão completa com operadores relacionais e aritméticos                  |

---

## Casos de Teste Inválidos 

| Expressão                            | Resultado   | Motivo do Erro                                                               |
|-------------------------------------|-------------|------------------------------------------------------------------------------|
| `*a/b`                              | Inválido | Início com operador `*` sem operando à esquerda                              |
| `a +`                               | Inválido | Operador `+` sem operando à direita                                          |
| `a b`                               | Inválido | Dois identificadores sem operador entre eles                                |
| `(a + b`                            | Inválido | Parêntese de fechamento ausente                                              |
| `a + b)`                            | Inválido | Parêntese de fechamento sem correspondente de abertura                       |
| `a + * b`                           | Inválido | Dois operadores seguidos sem operando intermediário                          |
| `a + b ( * c)`                      | Inválido | Uso inválido de parênteses                                                   |
| `123`                               | Inválido | Números não são suportados, apenas identificadores                           |
| `a + b > c < d`                     | Inválido | Dois operadores relacionais encadeados sem parênteses                        |
| *(vazio)*                           | Inválido | Expressão vazia                                                              |
| `a ++ b`                            | Inválido | `++` não é operador suportado; interpretado como dois `+` consecutivos       |
| `a + b * (c - d`                    | Inválido | Parêntese de fechamento ausente                                              |
