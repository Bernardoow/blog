Title: Tag com mesmo nome do branch.
Date: 2019-06-25 18:00
Category: Mercurial SCM
Tags: Mercurial, hg, SCM
Slug: tag-com-mesmo-nome-do-branch
Authors: Bernardo Gomes


Ao final de uma tarefa, eu realizei o merge entre branch e logo após criei uma tag para marcar o ponto que seria a publicação daquela dia. E essa seria a primeira vez que eu criaria uma tag por que não era eu que fazia o deploy mas naquele dia seria eu.

Ao mudar de branch, digitei um comando errado ao inves de mudar de branch criei uma tag com o nome de um mesmo branch que existia e não percebi. Realizei o deploy, prosegui com outras tarefas até chegar no dia de realizar outro deploy. E aconteceu um problema no processo de merges de branch.

O processo de deploy, nessa empresa, consiste de fazer o merge do branch default no branch da nova funcionadade, resolver os conflitos, fechar o branch e fazer outro merge do branch com nova tarefa com o default.

O meu problema aconteceu na parte que realizava o merge do default no branch da nova funcionalidade e falava que não podia fazer merge no ancestral. Depois de algum tempo buscando uma solução, descobri que o problema era a tag que o merge não sabia se fazia com o branch ou com a tag.

A minha solução foi deletar a tag manualmente do arquivo e o controle de versão voltou a funcionar. Essa solução foi encontra pelo meu colega de trabalho R. que conseguiu visualizar esse cenário e me ajudou a consertar esse problema.


Fluxos do Problema: 

1. mkdir hgblog
2. cd hgblog/
3. hg init
hg branch default
4. touch readme.md
5. hg add readme.md 
6. hg commit -m "first commit"


4. hg branches
default                        0:ef1f948f2eef
5. hg branch
default
6. hg tags
tip                                0:ef1f948f2eef
7. hg branch feature-one
8. touch file01_example.txt
9. hg add file01_example.txt
10. hg commit -m "Added file01_example.txt"
hg update default
hg branch feature-two
8. touch file02_example.txt
9. hg add file02_example.txt
10. hg commit -m "Added file02_example.txt"

11. hg tag default 
10. hg up default 

hg merge default
abort: merging with a working directory ancestor has no effect

.. hg sum

parent: 2:bfb2f9ae4579 default
 Added file02_example.txt
branch: feature-two
commit: (clean)
update: 1 new changesets (update)
phases: 4 draft

hg up default

parent: 2:bfb2f9ae4579 default
 Added file02_example.txt
branch: feature-two
commit: (clean)
update: 1 new changesets (update)
phases: 4 draft

hg up feature-two 
#vim .hg/cache/tags2-visible 
#delete tag

#touch filedefault.txt
#hg add filedefault.txt 
#hg commit -m "filedefault"

#warning: tag default conflicts with existing branch name
#touch file_in_default_branch.txt
#hg add file_in_default_branch.txt
#hg commit -m "added another file in default branch"
hg up feature-one

hg merge default
hg commit -m "feature-one <- default" --close-branch


hg up default

abort: merging with a working directory ancestor has no effect





hg tags
tip                                3:0fdaec178a6e
default                            1:c0f7e024d6f4

hg tag --remove default
warning: tag default conflicts with existing branch name
vim .hgtags 
remove line with default tag.
hg commit -m "fix tags"

hg merge default
abort: merging with a working directory ancestor has no effect
hg up default
vim .hgtags 
remove line with default tag.
hg commit -m "fix tags"

hg up feature-one

1 files updated, 0 files merged, 0 files removed, 0 files unresolved
(branch merge, don't forget to commit)
hg commit -m "feature-one <- default"
hg up default
hg merge feature-one 
2 files updated, 0 files merged, 0 files removed, 0 files unresolved
(branch merge, don't forget to commit)
hg commit -m "default <- feature-one"




hg commit -m "Close branch" --close-branch
