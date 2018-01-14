Title: Meld exibindo código na lingua japonesa-chinesa durante o merge. 
Date: 2017-12-24 18:00
Category: Outros
Tags: Mercurial, Linux, SCM, Merge
Slug: meld-merge-encoding
Authors: Bernardo Gomes

A minha equipe estava trabalhando em um projeto antigo e precisei fazer um merge. Uma situação relativamente simples de ser resolvida. Durante o merge foi aberto o programa Meld[1] e a comparação do texto não foi o que eu esperava pois o código comparado estava em uma lingua parecida com o japonês/chinês.

![alt "meld exibindo comparações com outra lingua."]({filename}/images/merge-unicode.png)

Pesquisei a solução e descobri que o problema estava na codificação do software para merge. No projeto que estava trabalhando, a codificação era latin-1 e por padrão a codificação do Meld era utf-8.

A solução utilizada foi primeiro checar o encoding suportado através do comando
```
$ gsettings get org.gnome.meld detect-encodings
['utf8']
```

Assim confirmei que estava somente aceitando utf-8. Para suportar o latin-1 executei o seguinte comando:

```
gsettings set org.gnome.meld detect-encodings "['utf8','latin1']"
```

Com isso, conseguir realizar o merge.

O caso apresentado é voltado para latin1. Se seu projeto for de outro encoding faça o mesmo processo alterando o latin1 para o encoding que utilizar.

Fontes:

[askubuntu.com](https://askubuntu.com/questions/839755/meld-shows-output-in-unreadable-alphabet-japanese-chinese)

[1] [Meld](http://meldmerge.org/)

