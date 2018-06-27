Title: Select multiplas-escolhas.
Date: 2018-07-01 18:00
Category: Outros
Tags: ELM
Slug: select-multiplas-escolhas
Authors: Bernardo Gomes
Status: draft

Eu estava criando uma tela utilizando Elm-Lang e me deparei com problema de pegar as multiplos itens selecionados em um input select.

No elm pegar o input único do select é realativamente fácil, utilizando o seguinte código.
```
import Html.Events exposing (targetValue, on)

onChangeDefault : (String -> msg) -> Html.Attribute msg
onChangeDefault tagger =
    on "change" (Json.Decode.map tagger targetValue)


select [ onChangeDefault SelectSelected, multiple True ]
        [ option [ value "1" ]
            [ text "Um" ]
        , option
            [ value "2" ]
            [ text "Dois" ]
        ]

type Msg
    = SelectSelected String
```