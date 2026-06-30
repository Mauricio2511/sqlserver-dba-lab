# Estrutura das Máquinas Virtuais

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

Infraestrutura Virtual

- Criada uma máquina virtual dedicada para atuar como Domain Controller.
- Criada uma máquina virtual dedicada para instalação do SQL Server.
- Configurados recursos de hardware virtual, como memória, disco, rede e armazenamento.
- Configurada rede entre as VMs para comunicação em ambiente de domínio.
- Configurados discos virtuais separados para sistema operacional, dados e logs do SQL Server.
