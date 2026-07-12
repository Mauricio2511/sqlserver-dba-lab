# SQL Server DBA Lab

Laboratório prático de DBA utilizando SQL Server 2019 em ambiente Windows Server 2019.

Este projeto tem como objetivo simular um ambiente corporativo para estudo e prática de administração de banco de dados com SQL Server.

---

## Módulos do Laboratório

| Status | Módulo | Descrição |
|---|---|---|
| ✅ | [01 - Infraestrutura](./01-infraestrutura/) | Construção da infraestrutura virtual do laboratório utilizando VirtualBox, criação das máquinas virtuais e organização dos discos. |
| ✅ | [02 - Active Directory](./02-active-directory/) | Configuração do domínio, Active Directory, DNS e criação das contas utilizadas no ambiente SQL Server. |
| ✅ | [03 - Instalação do SQL Server](./03-sql-server-installation/) | Preparação da VM do banco de dados, instalação e configuração inicial do SQL Server 2019. |
| ✅ | [04 - Segurança e Permissões](./04-seguranca-permissoes/) | Conceitos de autenticação, roles, permissões, schemas e boas práticas de segurança. |
| ✅ | [05 - Backup e Restore](./05-backup-restore/) | Recovery Models, backups Full, Diferenciais, Log, Restore, Compressão e Criptografia. |
| ✅ | [06 - SQL Server Agent e Jobs](./06-sql-server-agent-jobs/) | Automação de tarefas administrativas utilizando SQL Server Agent, criação e agendamento de Jobs, Steps e monitoramento das execuções. |
| ✅ | [07 - Database Mail e Alertas](./07-database-mail-alertas/) | Configuração do Database Mail, criação de Operators e envio automático de notificações por e-mail para Jobs do SQL Server Agent. |
| ✅ | [08 - Maintenance Plans](./08-maintenance-plans/) | Criação de plano de manutenção com verificação de integridade, backups Full, Diferencial e de Log, agendamentos independentes e validação das rotinas pelo SQL Server Agent. |
