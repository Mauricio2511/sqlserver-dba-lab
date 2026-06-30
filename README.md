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

## Módulos atuais do Laboratório

- ✅ [01 - Infraestrutura](01-infraestrutura/)
- ✅ [02 - Active Directory](02-active-directory/)
- ✅ [03 - Instalação do SQL Server](03-sql-server-installation/)
- ✅ [04 - Segurança e Permissões](04-seguranca-permissoes/)
- ✅ [05 - Backup e Restore](05-backup-restore/)
