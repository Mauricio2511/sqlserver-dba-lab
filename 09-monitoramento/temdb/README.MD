# Monitoramento do TempDB

## Cenário

Realizei um laboratório prático de monitoramento e troubleshooting do TempDB, simulando um cenário de crescimento, identificando a sessão responsável pelo consumo e realizando o tratamento do incidente.

Para isso, simulei um crescimento controlado do TempDB através da criação e preenchimento de uma tabela temporária.

O laboratório me permitiu acompanhar o ciclo completo de um incidente:

**Crescimento → Alerta → Investigação → Tratamento → Validação**

## Estado inicial

Antes de iniciar a simulação, analisei o tamanho dos arquivos e a utilização do TempDB.

### Evidência

![Estado inicial do TempDB](../../images/09-monitoramento/tempdb/tempdb-estado-inicial.png)

![Utilização inicial do TempDB](../../images/09-monitoramento/tempdb/tempdb-utilizacao-inicial.png)

## Simulação de crescimento

Criei e preenchi uma tabela temporária com grande volume de dados para provocar o crescimento controlado do TempDB.

### Evidência

![Crescimento do TempDB](../../images/09-monitoramento/tempdb/tempdb-crescimento.png)

## Alerta

Após o crescimento do TempDB, recebi o alerta informando que a utilização dos arquivos MDF havia ultrapassado o limite configurado.

### Evidência

![Alerta de crescimento do TempDB](../../images/09-monitoramento/tempdb/tempdb-alerta-crescimento.png)

## Investigação

Após receber o alerta, identifiquei a sessão responsável pelo consumo de espaço no TempDB.

Analisei as informações da sessão e consultei o último comando executado para confirmar a origem do consumo.

### Evidência

![Sessão responsável pelo consumo](../../images/09-monitoramento/tempdb/tempdb-sessao-responsavel.png)

Também analisei a utilização do TempDB durante o incidente para verificar o impacto do consumo.

### Evidência

![Utilização do TempDB durante o incidente](../../images/09-monitoramento/tempdb/tempdb-utilizacao-incidente.png)

## Tratamento

Como o consumo foi provocado pela minha própria sessão de laboratório, removi a tabela temporária utilizada no teste, liberando o espaço ocupado no TempDB.

### Evidência

![TempDB após tratamento](../../images/09-monitoramento/tempdb/tempdb-apos-tratamento.png)

## Validação

Após o tratamento, verifiquei o tamanho físico dos arquivos e confirmei que eles permaneceram com o tamanho adquirido durante o crescimento.

### Evidência

![Tamanho dos arquivos após tratamento](../../images/09-monitoramento/tempdb/tempdb-tamanho-apos-tratamento.png)

## Alerta CLEAR

Após a normalização da utilização, executei novamente o alerta para validar a recuperação do ambiente e recebi a mensagem de **CLEAR**, confirmando que a condição do alerta havia sido normalizada.

### Evidência

![Alerta CLEAR do TempDB](../../images/09-monitoramento/tempdb/tempdb-alerta-clear.png)

## Boas práticas

- Monitorar o crescimento e a utilização do TempDB.
- Investigar a sessão responsável antes de realizar qualquer intervenção.
- Evitar `KILL` ou redução dos arquivos sem compreender a causa do consumo.
- Não realizar shrink do TempDB automaticamente após todo crescimento.
- Dimensionar o TempDB de acordo com a carga esperada do ambiente.
