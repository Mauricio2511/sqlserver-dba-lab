# Monitoramento do TempDB

## Objetivo

Realizar um laboratório prático de monitoramento e troubleshooting do TempDB, simulando um cenário de crescimento, identificando a sessão responsável pelo consumo e realizando o tratamento do incidente.

## Cenário

Foi simulado um crescimento controlado do TempDB através da criação e preenchimento de uma tabela temporária.

O cenário permitiu acompanhar o ciclo completo de um incidente:

**Crescimento → Alerta → Investigação → Tratamento → Validação**

## Estado inicial

Antes da simulação, foi realizada a análise do tamanho dos arquivos e da utilização do TempDB.

### Evidência

![Estado inicial do TempDB](../../../../images/09-monitoramento/tempdb/tempdb-estado-inicial.png)

![Utilização inicial do TempDB](../../../../images/09-monitoramento/tempdb/tempdb-utilizacao-inicial.png)

## Simulação de crescimento

Foi criada e preenchida uma tabela temporária com grande volume de dados para provocar o crescimento controlado do TempDB.

### Evidência

![Crescimento do TempDB](../../../../images/09-monitoramento/tempdb/tempdb-crescimento.png)

## Alerta

O crescimento do TempDB ultrapassou o limite configurado para o alerta de utilização dos arquivos MDF.

### Evidência

![Alerta de crescimento do TempDB](../../../../images/09-monitoramento/tempdb/tempdb-alerta-crescimento.png)

## Investigação

Após o disparo do alerta, foi identificada a sessão responsável pelo consumo de espaço no TempDB.

A investigação foi realizada utilizando as DMVs de utilização do TempDB, informações da sessão e o último comando executado.

### Evidência

![Sessão responsável pelo consumo](../../../../images/09-monitoramento/tempdb/tempdb-sessao-responsavel.png)

## Tratamento

Como o consumo foi provocado pela própria sessão de laboratório, a tabela temporária utilizada no teste foi removida, liberando o espaço utilizado no TempDB.

### Evidência

![TempDB após tratamento](../../../../images/09-monitoramento/tempdb/tempdb-apos-tratamento.png)

## Validação

Após o tratamento, foi validada a utilização do TempDB e confirmado que os arquivos permaneceram com o tamanho físico adquirido durante o crescimento.

### Evidência

![Utilização após tratamento](../../../../images/09-monitoramento/tempdb/tempdb-apos-tratamento.png)

![Tamanho dos arquivos após tratamento](../../../../images/09-monitoramento/tempdb/tempdb-tamanho-apos-tratamento.png)

## Alerta CLEAR

Após a normalização da utilização, o alerta foi executado novamente para validar a recuperação do ambiente.

### Evidência

![Alerta CLEAR do TempDB](../../../../images/09-monitoramento/tempdb/tempdb-alerta-clear.png)

## Boas práticas

- Monitorar o crescimento e a utilização do TempDB.
- Investigar a sessão responsável antes de realizar qualquer intervenção.
- Evitar `KILL` ou redução dos arquivos sem compreender a causa do consumo.
- Não realizar shrink do TempDB automaticamente após todo crescimento.
- Dimensionar o TempDB de acordo com a carga esperada do ambiente.
