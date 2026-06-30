# Infraestrutura

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

### Infraestrutura Virtual

- Criada uma máquina virtual dedicada para atuar como Domain Controller.
- Criada uma máquina virtual dedicada para instalação do SQL Server.
- Configurados recursos de hardware virtual, como memória, disco, rede e armazenamento.
- Configurada rede entre as VMs para comunicação em ambiente de domínio.
- Configurados discos virtuais separados para sistema operacional, dados e logs do SQL Server.

---

| Etapa | Evidência |
|---|---|
| VM do Domain Controller criada no VirtualBox | ![VM Domain Controller](../images/01-infraestrutura/vm-domain-controller-virtualbox.png) |
| VM do SQL Server criada no VirtualBox | ![VM SQL Server](../images/01-infraestrutura/vm-sqlserver-virtualbox.png) |
