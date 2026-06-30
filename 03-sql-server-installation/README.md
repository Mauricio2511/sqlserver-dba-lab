# VM do SQL Server

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

# Instalação do SQL Server 2019

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
