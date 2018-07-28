Title: Problema com primary key datetime.
Date: 2018-07-28 18:00
Category: Python
Tags: SqlAlchemy, Sysdate, Oracle
Slug: sqlachemy-primary-key-sysdate-oracle-problema
Authors: Bernardo Gomes
Status: draft

Trabalhei em uma tarefa que possuía uma tabela legada que tinha como uma chave primária composta sendo um campo de string e o outro datetime.

```python
from sqlalchemy import Column, String, DateTime
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()


class BlogExemplo(Base):
    __tablename__ = 'blog_exemplo'
    campo_um = Column(String, primary_key=True)
    campo_dois = Column(DateTime, primary_key=True)
    campo_tres = Column(String)
```

Até esse momento nenhum problema. Possuímos um débito técnico no nosso servidor que a hora do máquina não é a mesma do banco de dados. Com isso, SEMPRE precisamos utilizar a função do banco de dados sysdate para obter a data do banco para depois atribui a uma coluna.

Nesse momento, no primeiro teste que fiz para criar um novo registro no banco de dados.
Criei um instance da classe como segue o exemplo e tentei salvar no banco de dados.

```python
exemplo = BlogExemplo()
exemplo.campo_um = "texto_1"
exemplo.campo_dois = func.sysdate()
exemplo.campo_tres = "outro texto"
db.add(exemplo)
db.commit()
```

A minha expectativa era que salvasse no banco de dados e que eu proseguiria com meu desenvolvimento, mas o que aconteceu foi um erro ser lançado com a informação que eu não tinha dados no chave primária campo_dois.

Investiguei e percebi que o ORM faz uma checagem antes de valores e como o func.sysdate() somente é executado no DB, asim para o ORM eu não tinha adicionado um valor para o campo.

Para resolver meu problema, eu segui a estratégia de criar uma função para obter data/hora e atribuir para a variável.

```python
from sqlalchemy.sql import text


def obter_hora_servidor():
    stmt = text("SELECT sysdate from DUAL;")
    result = conn.execute(stmt)
    return result.scalar()


exemplo = BlogExemplo()
exemplo.campo_um = "texto_1"
exemplo.campo_dois = obter_hora_servidor()
exemplo.campo_tres = "outro texto"
db.add(exemplo)
db.commit()

```

Assim conseguir resolver meu problema.

Obs: Esse problema somente acontecia na hora de criar um novo objeto. Se fosse para atualizar um existente poderia utilizar a func.sysdate().