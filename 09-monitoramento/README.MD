# Monitoramento SQL Server

## Ambiente

Este laboratório está sendo realizado em uma instância separada do SQL Server, dedicada aos estudos de monitoramento e troubleshooting.

- **SQL Server:** 2025 Enterprise Developer Edition (64-bit)
- **SSMS:** SQL Server Management Studio 22

## Objetivo

Realizar práticas de monitoramento e troubleshooting do SQL Server, analisando diferentes cenários para identificar, investigar e tratar problemas no ambiente.

## Referência

Para a implementação dos alertas utilizados neste laboratório, utilizei como referência o trabalho de **Fabrício Lima**, CEO da **Power Tuning**, profissional com ampla experiência em SQL Server e reconhecido pela comunidade Microsoft como MVP em Data Platform.

Os alertas utilizados neste ambiente foram baseados nos materiais disponibilizados por Fabrício, enquanto os cenários de monitoramento, testes, investigações e análises apresentados neste laboratório foram realizados por mim em um ambiente próprio de estudos.

Como material complementar, recomendo o curso gratuito **[Criando 40 alertas para monitorar o SQL Server](https://cursos.powertuning.com.br/course/criando-40-alertas-para-monitorar-o-sql-server)**, disponibilizado pela Power Tuning, que apresenta a criação e utilização de diversos alertas para monitoramento de ambientes SQL Server.

## Monitoramentos

| Status | Monitoramento | Descrição |
|---|---|---|
| ✅ | [Monitoramento do TempDB](tempdb/) | Monitoramento da utilização do TempDB, identificação de sessões, análise de consumo, alertas e troubleshooting. |
