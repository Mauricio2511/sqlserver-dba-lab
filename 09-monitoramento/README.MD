# Monitoramento SQL Server

## Ambiente

Este laboratório está sendo realizado em uma instância separada do SQL Server, dedicada aos estudos de monitoramento e troubleshooting.

- **SQL Server 2025:** Enterprise Developer Edition (64-bit)
- **SSMS:** SQL Server Management Studio 22

## Objetivo

Realizar práticas de monitoramento e troubleshooting do SQL Server, analisando diferentes cenários para identificar, investigar e tratar problemas no ambiente.

## Referência

Os alertas utilizados nos laboratórios fazem parte do curso **“Criando 40 alertas para monitorar o SQL Server”**, disponibilizado pelo [**Fabrício Lima**](https://www.linkedin.com/in/fabriciofrancalima/), CEO da [**Power Tuning**](https://powertuning.com.br/).

## Configuração do alerta

Para este laboratório, utilizei os seguintes parâmetros no alerta de utilização do TempDB:

- **Alerta:** Tempdb MDF File Utilization
- **Limite de utilização:** 70%
- **Tamanho mínimo do arquivo:** 100 MB

## Monitoramentos

| Status | Monitoramento | Descrição |
|---|---|---|
| ✅ | [Monitoramento do TempDB](tempdb/) | Monitoramento da utilização do TempDB, identificação de sessões, análise de consumo, alertas e troubleshooting. |
