# SQL Server DBA Lab

Laboratório prático de DBA utilizando SQL Server 2019 em ambiente Windows Server 2019 com Active Directory.

Este projeto tem como objetivo simular um ambiente corporativo para estudo e prática de administração de banco de dados com SQL Server.

---

## Tecnologias Utilizadas

| Tecnologia | Finalidade |
|---|---|
| Oracle VirtualBox | Virtualização do ambiente |
| Windows Server 2019 | Sistema operacional das VMs |
| Active Directory | Gerenciamento de domínio, usuários e contas de serviço |
| DNS Server | Resolução de nomes no ambiente de domínio |
| SQL Server 2019 | Sistema Gerenciador de Banco de Dados |
| SQL Server Management Studio | Administração da instância SQL Server |

---

## Arquitetura do Ambiente

O ambiente foi estruturado com duas máquinas virtuais principais:

```text
+-----------------------------+
| VM01 - DCSRV                |
| Windows Server 2019         |
| Active Directory            |
| DNS Server                  |
| Domain Controller           |
+-------------+---------------+
              |
              | Domínio: companyx.com
              |
+-------------v---------------+
| VM02 - DBSRV1               |
| Windows Server 2019         |
| SQL Server 2019             |
| SQL Server Management Studio|
+-----------------------------+

```

---

## Estrutura das Máquinas Virtuais

| VM | Nome do Servidor | Função |
|---|---|---|
| DCVM1 | DCSRV | Domain Controller |
| DBSRV1 | A definir/configurar | Servidor SQL Server |

---

## Domínio

O ambiente foi configurado com domínio próprio para simular uma estrutura corporativa.

| Item | Configuração |
|---|---|
| Domínio | companyx.com |
| Servidor Domain Controller | DCSRV |
| Sistema Operacional | Windows Server 2019 Datacenter Evaluation |
| Funções instaladas | Active Directory Domain Services e DNS |

> Por questões de segurança, endereços IP, senhas e demais informações sensíveis não são exibidos neste repositório.

---

## Organização dos Discos da VM SQL Server

A VM dedicada ao SQL Server foi configurada com discos separados para melhor organização dos arquivos do ambiente.

| Disco | Finalidade |
|---|---|
| Disco do Sistema | Instalação do Windows Server 2019 e arquivos do sistema operacional |
| Disco de Dados | Armazenamento dos arquivos de dados dos bancos de dados `.mdf` e `.ndf` |
| Disco de Logs | Armazenamento dos arquivos de log de transações `.ldf` |

Essa separação simula uma prática comum em ambientes SQL Server, permitindo melhor organização, manutenção e administração dos arquivos do banco de dados.

---

## Procedimentos Realizados

### 1. Infraestrutura Virtual

- Criada uma máquina virtual dedicada para atuar como Domain Controller.
- Criada uma máquina virtual dedicada para instalação do SQL Server.
- Configurados recursos de hardware virtual, como memória, disco, rede e armazenamento.
- Configurada rede entre as VMs para comunicação em ambiente de domínio.
- Configurados discos virtuais separados para sistema operacional, dados e logs do SQL Server.

---

### 2. Domain Controller

- Instalado o Windows Server 2019 na VM do Domain Controller.
- Configurado nome do servidor como `DCSRV`.
- Configurado endereço IP fixo.
- Instaladas as funções de Active Directory Domain Services e DNS.
- Promovido o servidor como Controlador de Domínio.
- Criado o domínio `companyx.com`.

---

### 3. Contas de Domínio para SQL Server

Foram criadas contas de domínio dedicadas para serviços e acessos relacionados ao SQL Server.

| Conta | Finalidade |
|---|---|
| SSMS | Conta de domínio para acesso administrativo ao SQL Server via SQL Server Management Studio |
| SSAS | Conta de serviço para o SQL Server Analysis Services |
| SSIS | Conta de serviço para o SQL Server Integration Services |
| SSRS | Conta de serviço para o SQL Server Reporting Services |

> Observação: o SSMS não é um serviço do SQL Server, mas sim a ferramenta gráfica utilizada para administração da instância. Neste laboratório, foi criada uma conta dedicada para acesso administrativo através do SSMS.

---

### 4. VM do SQL Server

- Criada uma VM dedicada para o servidor de banco de dados.
- Instalado o Windows Server 2019.
- Configurados discos separados para sistema operacional, dados e logs.
- Preparado o ambiente para ingresso no domínio e instalação do SQL Server 2019.

- ---

## Evidências

As evidências abaixo documentam as principais etapas realizadas na configuração inicial do laboratório.

| Etapa | Evidência |
|---|---|
| VM do Domain Controller criada no VirtualBox | ![VM Domain Controller](images/01-infraestrutura/vm-domain-controller-virtualbox.png) |
| VM do SQL Server criada no VirtualBox | ![VM SQL Server](images/01-infraestrutura/vm-sqlserver-virtualbox.png) |
| Domain Controller configurado no domínio | ![Domain Controller](images/02-active-directory/domain-controller-dominio.png) |
| Contas de domínio criadas para o ambiente SQL Server | ![Contas de Serviço](images/02-active-directory/contas-servico-sqlserver.png) |

---

### 5. Instalação do SQL Server 2019

- Realizada a instalação do SQL Server 2019 na VM dedicada ao servidor de banco de dados.
- Configurado o SQL Server Database Engine durante o processo de instalação.
- Configurado o SQL Server Agent para inicialização automática.
- Configuradas contas de domínio para execução dos serviços do SQL Server.
- Alterada a collation do Database Engine para `SQL_Latin1_General_CP1_CI_AI`.
- Configurado o modo de autenticação do SQL Server.
- Configurado acesso administrativo à instância.
- Definidos diretórios separados para arquivos de dados e arquivos de log.
- Mantido o diretório de backup no mesmo disco da instalação para fins de prática em laboratório.
- Configurado o TempDB com múltiplos arquivos de dados, utilizando 6 arquivos conforme a quantidade máxima de núcleos disponíveis no ambiente.
- Instalado o SQL Server Management Studio.
- Validada a conexão com a instância SQL Server utilizando autenticação do Windows.

> Observação: o diretório de backup foi mantido no mesmo disco da instalação apenas para fins de estudo. Em ambientes produtivos, o ideal é utilizar um disco separado para backups ou uma solução externa, como armazenamento em nuvem.

---

## Evidências

As evidências abaixo documentam as principais etapas realizadas durante a instalação e configuração inicial do SQL Server 2019.

| Etapa | Evidência |
|---|---|
| Configuração das contas de serviço do SQL Server | ![Configuração das contas de serviço do SQL Server](images/03-sql-server/config-contas-servicos.png) |
| Configuração da collation do Database Engine | ![Configuração da collation do Database Engine](images/03-sql-server/collation.png) |
| Configuração de acesso à instância SQL Server | ![Configuração de acesso à instância SQL Server](images/03-sql-server/configuracao-acesso.png) |
| Configuração dos diretórios de dados e logs | ![Configuração dos diretórios de dados e logs](images/03-sql-server/conf-discos.png) |
| Configuração do TempDB | ![Configuração do TempDB](images/03-sql-server/tempdb.png) |
| Conexão ao SQL Server via SSMS com autenticação do Windows | ![Conexão ao SQL Server via SSMS](images/03-sql-server/banco-conectado.png) |

> Senhas, endereços IP e demais informações sensíveis foram omitidos ou mascarados por boas práticas de segurança.


---

### 6. Conceitos de Segurança no SQL Server

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

## 7. Backup e Restore

Backup e restore são atividades essenciais na rotina de um DBA, pois permitem proteger os dados e recuperar bancos em cenários de falha, exclusão acidental, corrupção ou necessidade de retorno para um ponto anterior.

Neste bloco, criei o banco `BKORES` para ser utilizado ao longo das práticas de backup e restore. A partir dele, realizei testes de alteração de recovery model, geração de backups e simulações de recuperação do banco de dados.

---

### Recovery Model

O recovery model define como o SQL Server registra as transações no log e influencia diretamente as possibilidades de backup e restore.

Durante os estudos, trabalhei com os principais modelos de recuperação:

| Recovery Model | Descrição                                                                                                                                                        |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SIMPLE`       | O log de transações é reutilizado automaticamente, não permitindo backup de log. É mais simples, porém oferece menor possibilidade de recuperação ponto a ponto. |
| `FULL`         | Mantém o histórico completo das transações no log, permitindo backups de log e maior controle na recuperação do banco.                                           |
| `BULK_LOGGED`  | Modelo intermediário, usado em operações em massa para reduzir o volume de log gerado, mantendo suporte a backup de log em cenários específicos.                 |

---

### Tipos de Backup

Também pratiquei os principais tipos de backup utilizados no SQL Server:

| Tipo de Backup | Descrição                                                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `FULL`         | Realiza uma cópia completa do banco de dados no momento do backup.                                                              |
| `DIFERENCIAL`  | Armazena somente as alterações realizadas desde o último backup Full.                                                           |
| `LOG`          | Registra as transações realizadas no banco, permitindo restaurações mais granulares quando o banco está no recovery model Full. |

---

## Evidências

As evidências abaixo documentam a criação do ambiente de testes, a validação dos recovery models e as primeiras configurações utilizadas para as práticas de backup e restore.

| Etapa | Evidência |
|---|---|
| Validação inicial dos recovery models das bases da instância | ![Recovery model inicial](images/05-backup-restore/recovery-model-inicial.png) |
| Alteração do banco `BKORES` para recovery model `SIMPLE` | ![Recovery model SIMPLE no banco BKORES](images/05-backup-restore/recovery-model-bkores-simple.png) |
| Alteração do banco `BKORES` para recovery model `FULL` | ![Recovery model FULL no banco BKORES](images/05-backup-restore/recovery-model-bkores-full.png) |

Nesta etapa realizei o primeiro backup Full do banco `BKORES`. O arquivo foi armazenado em um disco dedicado para backups, seguindo a boa prática de separar arquivos de backup dos arquivos de dados e logs do banco de dados.

| Etapa | Evidência |
|---|---|
| Execução do backup Full do banco `BKORES` com sucesso | ![Backup Full do banco BKORES](images/05-backup-restore/backup-full-bkores-sucesso.png) |
| Arquivo `.bak` gerado na pasta de backup | ![Arquivo de backup Full gerado](images/05-backup-restore/arquivo-backup-full-bkores.png) |
| Validação do banco `BKORES` disponível após o backup | ![Validação do banco BKORES após backup Full](images/05-backup-restore/validacao-bkores-apos-backup-full.png) |

Nesta etapa simulei um cenário de perda do banco `BKORES`, removendo-o da instância e realizando a restauração a partir do backup Full criado anteriormente. Após o restore, foi validado que o banco voltou a ficar online e que os dados da tabela de teste foram recuperados com sucesso.

| Etapa | Evidência |
|---|---|
| Remoção do banco `BKORES` para simular perda da base | ![Banco BKORES removido da instância](images/05-backup-restore/bkores-removido-instancia.png) |
| Restore Full do banco `BKORES` executado com sucesso | ![Restore Full do banco BKORES](images/05-backup-restore/restore-full-bkores-sucesso.png) |
| Validação do banco `BKORES` online e dados recuperados após o restore | ![Validação dos dados após Restore Full](images/05-backup-restore/validacao-dados-bkores-apos-restore-full.png) |

Nesta etapa realizei um backup Full do banco `BKORES` utilizando compressão. A prática permitiu comparar o tamanho do arquivo gerado com o backup Full anterior, demonstrando como a compressão pode reduzir o espaço utilizado para armazenamento dos backups.

IMPORTANTE: Apesar de ser uma prática positiva para economia de espaço em disco, é essencial observar que backups com compressão podem gerar maior consumo de CPU durante a execução. Por isso, em ambientes produtivos, essa configuração deve ser avaliada considerando a capacidade do servidor, o horário de execução dos backups e o impacto na carga de trabalho do banco de dados.

| Etapa | Evidência |
|---|---|
| Execução do backup Full com compressão do banco `BKORES` | ![Backup Full com compressão do banco BKORES](images/05-backup-restore/backup-full-compressao-bkores-sucesso.png) |
| Comparação entre o backup Full normal e o backup Full com compressão | ![Comparação entre backup normal e comprimido](images/05-backup-restore/comparacao-backup-full-normal-compressao.png) |
