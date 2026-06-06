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
