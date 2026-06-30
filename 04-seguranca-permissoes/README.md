# Conceitos de Segurança no SQL Server

Nesta etapa estudei conceitos iniciais de segurança no SQL Server, incluindo modos de autenticação, nível de servidor, de banco de dados, permissões em objetos e utilização de schemas.

A segurança no SQL Server pode ser aplicada em diferentes níveis, permitindo controlar o acesso de usuários e grupos conforme a necessidade do ambiente. Esse controle pode ocorrer na instância, no banco de dados, nos objetos do banco ou por meio da organização lógica utilizando schemas.

---

#### Modos de Autenticação

O SQL Server permite dois principais modos de autenticação:

| Modo de Autenticação | Descrição |
|---|---|
| Windows Authentication | Utiliza usuários ou grupos do Windows/Active Directory para autenticação. O acesso é criado no Windows e vinculado ao SQL Server por meio de permissões. |
| SQL Server Authentication | Utiliza logins criados diretamente no SQL Server, como o usuário `sa` ou outros logins SQL. O acesso é gerenciado dentro da própria instância SQL Server. |
| Mixed Mode | Permite utilizar tanto autenticação do Windows quanto autenticação SQL Server. |

---

#### Segurança em Nível de Servidor

As roles em nível de servidor controlam permissões aplicadas à instância SQL Server como um todo.

| Role | Permissões |
|---|---|
| `sysadmin` | Realiza qualquer atividade no SQL Server. A permissão deste papel compreende as permissões de todos os outros papéis. |
| `securityadmin` | Gerencia os logins. Esta role pode inclusive conseguir se adicionar à role `sysadmin` ou adicionar outro usuário à role `sysadmin`. |
| `serveradmin` | Pode alterar configurações da instância e executar shutdown na instância. |
| `diskadmin` | Utilizada basicamente para criar e excluir dispositivos de backup. |
| `dbcreator` | Cria e altera bancos de dados. |
| `processadmin` | Tem permissão para executar `KILL` em processos da instância. |
| `setupadmin` | Tem permissão para gerenciar Linked Servers. |
| `bulkadmin` | Executa o comando `BULK INSERT`. |
| `public` | Todo login do SQL Server pertence à role `public`. Essa role permite conectar na instância, visualizar os bancos de dados da instância e executar instruções de `SELECT` no banco `master`. Deve ser utilizada com cuidado, pois permissões indevidas nessa role podem gerar riscos de segurança. |

---

#### Segurança em Nível de Banco de Dados

As roles em nível de banco de dados controlam permissões dentro de um banco específico.

| Role | Permissões |
|---|---|
| `db_owner` | Tem poderes totais sobre o banco de dados. |
| `db_accessadmin` | Pode adicionar e remover usuários no banco de dados. |
| `db_datareader` | Pode ler dados em todas as tabelas dos bancos de dados dos usuários. |
| `db_datawriter` | Pode adicionar, alterar ou excluir dados em todas as tabelas de usuário do banco de dados. |
| `db_ddladmin` | Pode adicionar, modificar ou excluir objetos do banco de dados, como tabelas, por exemplo. |
| `db_securityadmin` | Pode gerenciar roles e adicionar ou excluir usuários às roles do banco de dados. Também pode gerenciar permissões para objetos do banco de dados. |
| `db_backupoperator` | Pode realizar backup do banco de dados. |
| `db_denydatareader` | Não pode consultar dados em nenhuma das tabelas do banco de dados. |
| `db_denydatawriter` | Não pode alterar dados no banco de dados. |

---

#### Segurança em Nível de Objetos

A segurança em nível de objetos permite controlar permissões específicas em objetos do banco de dados, como:

- Tabelas;
- Views;
- Stored Procedures;
- Functions.

Os principais comandos utilizados para controle de permissões em objetos são:

| Privilégio | Descrição |
|---|---|
| `GRANT` | Atribui privilégios de acesso do usuário a objetos do banco de dados. |
| `REVOKE` | Remove os privilégios de acesso aos objetos obtidos com o comando `GRANT`. |
| `DENY` | Nega permissão a um usuário ou grupo para realizar operação em um objeto. |

---

#### Schemas no SQL Server

Schemas são coleções de objetos dentro de um banco de dados. Eles ajudam a organizar tabelas, views, procedures, functions e outros objetos de forma lógica.

Exemplo de organização por schema:

```sql
dbo.Clientes
financeiro.Pagamentos
rh.Funcionarios
```
---

## Evidências

As evidências abaixo documentam a criação do login `dbasql`, o vínculo com o banco `CLIENTES` e a validação por meio de views de sistema.

| Etapa | Evidência |
|---|---|
| Criação do login `dbasql` com autenticação SQL Server | ![Criação do login dbasql](images/04-seguranca/criacao-login-dbasql.png) |
| Login `dbasql` criado na instância SQL Server | ![Login dbasql criado na instância](images/04-seguranca/login-dbasql-criado-instancia.png) |
| Criação do user `dbasql` no banco CLIENTES | ![Criação do user dbasql no banco CLIENTES](images/04-seguranca/criacao-user-dbasql-clientes.png) |
| Validação do login `dbasql` na instância | ![Validação do login dbasql](images/04-seguranca/validacao-login-dbasql-instancia.png) |
| Validação do user `dbasql` no banco CLIENTES | ![Validação do user dbasql](images/04-seguranca/validacao-user-dbasql-clientes.png) |

Nesta etapa foi validada a concessão de permissão mínima em nível de objeto. Inicialmente, o usuário `dbasql` não possuía permissão para consultar a tabela `dbo.Customer2`. Após a execução do comando `GRANT SELECT`, o acesso à tabela foi permitido.

| Etapa | Evidência |
|---|---|
| Teste de acesso à tabela `Customer2` antes da permissão | ![Teste de acesso antes do GRANT](images/04-seguranca/teste-acesso-customer2-antes-grant.png) |
| Concessão da permissão `SELECT` na tabela `Customer2` para o user `dbasql` | ![GRANT SELECT em Customer2 para dbasql](images/04-seguranca/grant-select-customer2-dbasql.png) |
| Teste de acesso à tabela `Customer2` após a permissão | ![Teste de acesso após o GRANT](images/04-seguranca/teste-acesso-customer2-apos-grant.png) |

Nesta etapa demosntro a diferença entre a remoção de uma permissão e a negação explícita de acesso. Após remover a permissão `SELECT` do usuário `dbasql`, o acesso à tabela `dbo.Customer2` deixou de ser permitido. Em seguida, apliquei uma negação explícita, reforçando o bloqueio de acesso ao objeto.

| Etapa | Evidência |
|---|---|
| Remoção da permissão `SELECT` na tabela `Customer2` para o user `dbasql` | ![REVOKE SELECT em Customer2 para dbasql](images/04-seguranca/revoke-select-customer2-dbasql.png) |
| Teste de acesso à tabela `Customer2` após o `REVOKE` | ![Teste de acesso após o REVOKE](images/04-seguranca/teste-acesso-customer2-apos-revoke.png) |
| Negação explícita da permissão `SELECT` na tabela `Customer2` para o user `dbasql` | ![DENY SELECT em Customer2 para dbasql](images/04-seguranca/deny-select-customer2-dbasql.png) |

Nesta etapa apliquei uma permissão de leitura em nível de coluna. O usuário `dbasql` recebeu acesso apenas às colunas `Id`, `FirstName` e `LastName` da tabela `dbo.Customer2`. Ao tentar consultar todas as colunas da tabela, o acesso foi negado para as colunas não autorizadas.

| Etapa | Evidência |
|---|---|
| Concessão da permissão `SELECT` apenas para colunas específicas da tabela `Customer2` | ![GRANT SELECT em colunas específicas da Customer2](images/04-seguranca/grant-select-colunas-customer2-dbasql.png) |
| Teste de acesso às colunas permitidas da tabela `Customer2` | ![Teste de acesso às colunas permitidas](images/04-seguranca/teste-acesso-colunas-permitidas-customer2.png) |
| Teste de acesso a todas as colunas da tabela `Customer2` com permissão negada | ![Teste de acesso negado a todas as colunas](images/04-seguranca/teste-acesso-todas-colunas-customer2-negado.png) |

Nesta etapa mostro o uso de uma role personalizada para administrar permissões no banco de dados. A role `ALLCustomer` recebeu permissões na tabela `dbo.Customer3` e o usuário `dbasql` passou a herdar esses acessos ao ser adicionado como membro da role. Após a remoção do usuário da role, o acesso à tabela deixou de ser permitido.

| Etapa | Evidência |
|---|---|
| Criação da role `ALLCustomer` e concessão de permissões na tabela `Customer3` | ![Criação da role e concessão de permissões](images/04-seguranca/criacao-role-allcustomer-permissoes-customer3.png) |
| Adição do user `dbasql` como membro da role `ALLCustomer` | ![User dbasql adicionado à role](images/04-seguranca/dbasql-adicionado-role-allcustomer.png) |
| Teste de acesso à tabela `Customer3` através da role | ![Teste de acesso via role](images/04-seguranca/teste-acesso-customer3-via-role.png) |
| Remoção do user `dbasql` da role `ALLCustomer` | ![User dbasql removido da role](images/04-seguranca/dbasql-removido-role-allcustomer.png) |
| Teste de acesso à tabela `Customer3` após remoção da role | ![Teste de acesso após remoção da role](images/04-seguranca/teste-acesso-customer3-apos-remocao-role.png) |

Nesta etapa criei um banco de dados específico para praticar permissões em nível de schema. Dentro desse banco, foram criados os schemas `SALES` e `FINANCEIRO`, além de tabelas associadas a cada schema. O user `dbasql` recebeu permissões apenas no schema `FINANCEIRO`, permitindo o acesso à tabela desse schema e mantendo o acesso negado aos objetos do schema `SALES`.

| Etapa | Evidência |
|---|---|
| Criação dos schemas `SALES` e `FINANCEIRO` no banco `TESTESCHEMA` | ![Criação dos schemas SALES e FINANCEIRO](images/04-seguranca/criacao-schemas-sales-financeiro.png) |
| Criação das tabelas nos schemas `SALES` e `FINANCEIRO` | ![Criação das tabelas nos schemas](images/04-seguranca/criacao-tabelas-sales-financeiro.png) |
| Concessão das permissões `SELECT` e `INSERT` no schema `FINANCEIRO` para o user `dbasql` | ![GRANT no schema FINANCEIRO para dbasql](images/04-seguranca/grant-select-insert-schema-financeiro-dbasql.png) |
| Teste de acesso permitido à tabela `FINANCEIRO.PAGAMENTOS` | ![Teste de acesso permitido no schema FINANCEIRO](images/04-seguranca/teste-acesso-schema-financeiro-permitido.png) |
| Teste de acesso negado à tabela `SALES.VENDAS` | ![Teste de acesso negado no schema SALES](images/04-seguranca/teste-acesso-schema-sales-negado.png) |

Nesta etapa demonstrei o bloqueio explícito de uma operação em nível de schema. Mesmo com permissão de leitura no schema `FINANCEIRO`, o user `dbasql` teve a operação de insert negada, mantendo o select permitido e bloqueando apenas o cadastro de novos registros.

| Etapa | Evidência |
|---|---|
| Negação explícita da permissão `INSERT` no schema `FINANCEIRO` para o user `dbasql` | ![DENY INSERT no schema FINANCEIRO para dbasql](images/04-seguranca/deny-insert-schema-financeiro-dbasql.png) |
| Teste de consulta permitido na tabela `FINANCEIRO.PAGAMENTOS` após o `DENY INSERT` | ![Teste de SELECT após DENY INSERT](images/04-seguranca/teste-select-schema-financeiro-apos-deny-insert.png) |
| Teste de inserção negado na tabela `FINANCEIRO.PAGAMENTOS` | ![Teste de INSERT negado no schema FINANCEIRO](images/04-seguranca/teste-insert-schema-financeiro-negado.png) |

Nesta etapa foi realizada a auditoria das permissões do user `DBASQL` no banco `TESTESCHEMA`. A validação demonstrou que o user está vinculado ao login `dbasql`, não possui associação com roles fixas amplas do banco e possui permissões específicas configuradas no schema `FINANCEIRO`.

| Etapa | Evidência |
|---|---|
| Validação do user `DBASQL` vinculado ao login `dbasql` no banco `TESTESCHEMA` | ![Validação do user DBASQL no banco TESTESCHEMA](images/04-seguranca/validacao-user-dbasql-testschema.png) |
| Auditoria das permissões do user `DBASQL` no schema `FINANCEIRO` | ![Auditoria das permissões do DBASQL no schema FINANCEIRO](images/04-seguranca/auditoria-permissoes-dbasql-schema-financeiro.png) |
| Validação de membership do user `DBASQL` sem associação a roles fixas amplas | ![Validação de membership do DBASQL](images/04-seguranca/validacao-membership-dbasql-testschema.png) |

Como boas práticas, entendo que é importante evitar o uso do login `sa` em rotinas administrativas do dia a dia, priorizando contas nomeadas e auditáveis. Quando possível, o ideal é manter o `sa` desabilitado ou renomeado, além de garantir o uso de senha forte caso ele precise permanecer ativo.

Também considero importante revisar periodicamente as permissões concedidas, auditar os acessos dos usuários e priorizar permissões específicas em vez de liberações genéricas. Evitar permissões amplas para usuários comuns, como `db_owner`, ajuda a aplicar o princípio do menor privilégio e manter o ambiente SQL Server mais seguro e controlado.
