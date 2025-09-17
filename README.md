# SQLC com Python

Este projeto demonstra a utilização do sqlc para gerar código Python a partir de arquivos SQL. A ferramenta automatiza a criação de classes de modelo e funções para interagir com o banco de dados, o que ajuda a garantir a segurança e a tipagem das queries SQL.

## Sobre o sqlc

sqlc é um gerador de código que converte queries SQL em código Go, Kotlin, Python, TypeScript e Rust com segurança de tipo. Ele lê seu schema e suas queries SQL, e em seguida, gera um código limpo e seguro para interagir com seu banco de dados.

## Estrutura do Projeto

A estrutura do diretório é organizada da seguinte forma:
```text
.
├── db/
│   ├── __init__.py
│   ├── models.py
│   └── query.py
├── query.sql
├── schema.sql
└── sqlc.yaml
```

    schema.sql: Contém a definição da tabela authors.

    query.sql: Contém as queries SQL definidas para o projeto, como GetAuthor, ListAuthors, CreateAuthor, UpdateAuthor e DeleteAuthor.

    sqlc.yaml: É o arquivo de configuração do sqlc. Ele especifica o schema, as queries, o motor do banco de dados (postgresql) e as opções de geração de código para Python, incluindo o pacote de saída (db) e a emissão de queriers síncronos e assíncronos.

    db/: Este diretório é o destino do código gerado pelo sqlc.

        models.py: Contém a classe Author, que representa a estrutura da tabela authors.

        query.py: Contém as classes Querier e AsyncQuerier, que expõem os métodos para executar as queries definidas em query.sql.

### Como Executar

Para rodar este projeto, você precisará ter o sqlc e o Docker (para o PostgreSQL) instalados em sua máquina.

Clone o repositório:
```bash
git clone <url_do_repositorio>
cd <nome_do_diretorio>
```

Configure o banco de dados:
Você pode usar um contêiner Docker para o PostgreSQL.


# Exemplo de comando docker-compose ou docker run
```bash
docker run --name some-postgres-image -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
```

Instale as dependências Python:
```bash
pip install sqlalchemy
```

Execute o sqlc para gerar o código:
```bash
sqlc generate
```

Este comando lerá o arquivo sqlc.yaml e gerará o código Python necessário no diretório db/.

Utilize o código gerado:
Agora você pode usar as classes Querier ou AsyncQuerier em seu código Python para interagir com o banco de dados.

```python
### Exemplo de uso
import sqlalchemy
from db.query import Querier

### Supondo que você tenha uma URL de conexão
DATABASE_URL = "postgresql://postgres:mysecretpassword@localhost:5432/postgres"
engine = sqlalchemy.create_engine(DATABASE_URL)

with engine.connect() as conn:
    querier = Querier(conn)

    # Exemplo de criação de um autor
    new_author = querier.create_author(name="Erick Duarte", bio="Um desenvolvedor de software.")
    print(f"Autor criado: {new_author.name}")

    # Exemplo de listagem de autores
    authors = querier.list_authors()
    for author in authors:
        print(f"ID: {author.id}, Nome: {author.name}")
```
