# SQL Server Agent e Jobs

## Objetivo

O SQL Server Agent é um componente do SQL Server responsável por automatizar tarefas administrativas por meio da criação e execução de Jobs. Esse recurso permite agendar rotinas como backups, verificações de integridade, manutenção de índices e outras atividades essenciais para a administração do ambiente.

Neste laboratório foi criado um Job para executar o comando **DBCC CHECKDB** no banco de dados **CLIENTES**, permitindo compreender a criação, configuração, agendamento e monitoramento de uma rotina automatizada.

---

## Procedimentos realizados

Durante esta prática foram executadas as seguintes atividades:

- Inicialização e utilização do SQL Server Agent;
- Criação do Job **DatabaseCheck**;
- Configuração do Step **CheckDatabase** para execução do comando **DBCC CHECKDB**;
- Definição do banco de dados **CLIENTES** como contexto de execução;
- Configuração do **Owner** como `sa`, seguindo a boa prática de evitar dependência do usuário responsável pela criação do Job;
- Criação de um Schedule recorrente para execução diária às **12:00**;
- Associação do Schedule ao Job;
- Execução manual do Job para validação da configuração;
- Consulta ao histórico de execução para confirmação da conclusão com sucesso.

---

# Evidências

## SQL Server Agent

O SQL Server Agent foi iniciado e o Job criado passou a fazer parte da estrutura de gerenciamento do servidor.

| Evidência |
|-----------|
| ![](../images/sql-server-agent-expandido.png) |

---

## Propriedades do Job

Foi criado o Job **DatabaseCheck**, classificado na categoria **Database Maintenance** e configurado para ser executado sob o login **sa**, garantindo maior estabilidade e independência em relação ao usuário responsável pela criação do Job.

| Evidência |
|-----------|
| ![](../images/propriedades-job-databasecheck.png) |

---

## Configuração do Step

Foi criado o Step **CheckDatabase**, responsável por executar o comando **DBCC CHECKDB** no banco de dados **CLIENTES**, realizando a verificação da integridade lógica e física da base.

| Evidência |
|-----------|
| ![](../images/step-job-databasecheck.png) |

---

## Configuração do Schedule

Foi criado um agendamento recorrente para que o Job seja executado automaticamente todos os dias às **12:00**.

| Evidência |
|-----------|
| ![](../images/schedule-job-databasecheck.png) |

---

## Associação do Schedule

Após sua criação, o Schedule foi vinculado ao Job **DatabaseCheck**, permitindo sua execução automática conforme a periodicidade configurada.

| Evidência |
|-----------|
| ![](../images/schedule-vinculado-job-databasecheck.png) |

---

## Execução manual

Antes da primeira execução agendada, o Job foi executado manualmente para validar toda a configuração realizada.

| Evidência |
|-----------|
| ![](../images/execucao-manual-job-databasecheck.png) |

---

## Histórico de execução

Após a execução manual, foi consultado o histórico do Job, confirmando que a rotina foi concluída com sucesso e registrada pelo SQL Server Agent.

| Evidência |
|-----------|
| ![](../images/historico-execucao-job-databasecheck.png) |

---

# Boas práticas

Durante este laboratório foram aplicadas algumas práticas recomendadas para administração do SQL Server:

- Utilizar nomes descritivos para Jobs, Steps e Schedules;
- Definir um **Owner** estável, como o login `sa`, evitando dependência de contas pessoais;
- Validar manualmente um Job antes de colocá-lo em produção;
- Automatizar tarefas recorrentes utilizando Schedules;
- Acompanhar regularmente o histórico de execução para identificar possíveis falhas.
