Title: Criando Hooks no Mercurial.
Date: 2019-06-25 18:00
Category: Mercurial SCM
Tags: Mercurial, hg, SCM, Hook, Hooks
Slug: hooks-no-mercurial
Authors: Bernardo Gomes

Para criar hook no mercurial é simples. Basta você editar o arquivo .hg/hgrc, criar uma sessão [hooks] e adicionar o comando que deverá ser executado em um determinado momento.


## Exemplo de verificação antes de comitar.

Nesse exemplo quero checar que não existe comando pdb no código no momento do commit.

1. vim .hg/hgrc
2. adicionar a sessão [hooks]
3. adicionar o código pretxncommit.checkpdb = hg export tip | (! grep -q 'pdb.set_trace()') 

![alt "Exemplo de um hook"]({filename}/images/mercurial_hook.png)


