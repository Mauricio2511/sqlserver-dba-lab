# Active Directory

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

## Procedimentos Realizados

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

## Evidências

As evidências abaixo documentam as principais etapas realizadas na configuração inicial do laboratório.

| Etapa | Evidência |
|---|---|
| Domain Controller configurado no domínio | ![Domain Controller](../images/02-active-directory/domain-controller-dominio.png) |
| Contas de domínio criadas para o ambiente SQL Server | ![Contas de Serviço](../images/02-active-directory/contas-servico-sqlserver.png) |
