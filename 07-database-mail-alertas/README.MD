# Database Mail e Alertas

## Objetivo

O Database Mail permite ao SQL Server enviar notificações por e-mail utilizando um servidor SMTP. Esse recurso é amplamente utilizado para alertar administradores sobre falhas em Jobs, eventos críticos e outras rotinas automatizadas executadas pelo SQL Server Agent.

Neste laboratório foi configurado o Database Mail utilizando uma conta SMTP dedicada, criado um Profile para gerenciamento das configurações de envio e realizada a integração com um Operator do SQL Server Agent para notificação automática de falhas no Job **DatabaseCheck**.

---

## Procedimentos realizados

Durante esta prática foram executadas as seguintes atividades:

- Configuração de uma conta SMTP para envio de e-mails;
- Criação do Profile **AlertasDB**;
- Associação da conta SMTP ao Profile;
- Envio de e-mail de teste para validação da configuração;
- Confirmação do recebimento da mensagem;
- Criação do Operator **dbaalertas**;
- Configuração do Job **DatabaseCheck** para envio de notificações por e-mail em caso de falha;
- Execução de um teste de falha controlada para validação do processo de notificação.

---

# Evidências

## Configuração da conta SMTP

Foi criada uma conta do Database Mail responsável pelo envio das notificações utilizando um servidor SMTP externo.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/configuracao-conta-database-mail.png) |

---

## Configuração do Profile

Foi criado o Profile **AlertasDB**, responsável por centralizar as configurações de envio de e-mails e disponibilizar a conta SMTP para utilização pelo SQL Server.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/perfil-database-mail.png) |

---

## Envio do e-mail de teste

Após a configuração do Database Mail, foi realizado um envio de teste para validar a comunicação com o servidor SMTP.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/envio-teste-database-mail.png) |

---

## Validação do envio

O recebimento da mensagem confirmou o funcionamento correto da configuração do Database Mail.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/email-recebido-database-mail.png) |

---

## Criação do Operator

Foi criado o Operator **dbaalertas**, responsável por receber notificações enviadas pelo SQL Server Agent.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/operator-database-mail.png) |

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/operator-criado.png) |

---

## Configuração das notificações do Job

O Job **DatabaseCheck** foi configurado para enviar notificações por e-mail ao Operator sempre que ocorrer uma falha durante sua execução.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/notificacao-job-databasecheck.png) |

---

## Validação da notificação automática

Para validar a configuração, foi provocada uma falha controlada durante a execução do Job **DatabaseCheck**. O SQL Server Agent enviou automaticamente uma notificação por e-mail ao Operator configurado, confirmando a integração entre Job, Operator e Database Mail.

| Evidência |
|-----------|
| ![](../images/07-database-mail-alertas/email-falha-job-databasecheck.png) |

---

# Boas práticas

Durante este laboratório foram aplicadas algumas práticas recomendadas para administração do SQL Server:

- Utilizar uma conta dedicada para envio de notificações por e-mail;
- Centralizar as configurações de envio utilizando Profiles;
- Configurar Operators para recebimento de notificações administrativas;
- Configurar notificações de falha para Jobs críticos executados pelo SQL Server Agent;
- Validar as configurações por meio de testes controlados antes da utilização em ambiente de produção;
- Priorizar notificações em caso de falha, reduzindo o volume de alertas desnecessários e facilitando o monitoramento operacional.
