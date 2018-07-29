Title: Pattern Matching List.
Date: 2018-07-29 18:00
Category: ELM-Lang
Tags: ELM
Slug: pattern-matching-list
Authors: Bernardo Gomes

Durante um desenvolvimento, me deparei com necessidade de transformar uma data que estava em forma de uma lista: ["2018", "05", "10", "10", "20", "15"] em uma string formatada "2018/05/10".

Meu primeiro pensamento foi fazer uma indexedMap e depois criar uma função com vários ifs para utilizar dentro de um fold para obter a data.
Depois algum tempo de análise, encontrei uma alternativa com pattern matching. Eu estava observando a documentação da função head da lista
que fuciona assim:

```elm
head : List a -> Maybe a
head list =
  case list of
    x :: xs ->
      Just x

    [] ->
      Nothing
```

Nesse momento tive a idéia de fazer pattern-matching com os valores que eu precisava:

```elm
transformaEmData : List String -> String
transformaEmData list =
  case list of
    ano :: mes: dia:: hora :: minuto: segundo :: [] ->
      ano ++ "/" ++ mes ++ "/" ++ dia

    _ ->
      "Data inválida."
```

Com essa implementaçao eu consegui resolver meu problema.
