## Lab 1

### Cenário

Neste laboratório, simulei um cenário de crescimento do TempDB para praticar o processo de monitoramento, identificação da sessão responsável pelo consumo, análise da utilização e tratamento do incidente.

### Estado inicial

Inicialmente, analisei o tamanho físico dos arquivos do TempDB.

A instância possuía oito arquivos de dados com 8 MB cada e um arquivo de log com 8 MB.

![Estado inicial do TempDB](../../images/09-monitoramento/tempdb/lab1/tempdb-estado-inicial.png)

Também consultei a utilização interna do TempDB para estabelecer uma linha de base antes da simulação.

![Utilização inicial do TempDB](../../images/09-monitoramento/tempdb/lab1/tempdb-utilizacao-inicial.png)

### Simulação de crescimento

Criei uma tabela temporária e realizei uma inserção controlada de dados para provocar o crescimento do TempDB.

Após a execução, os arquivos de dados cresceram de 8 MB para aproximadamente 136 MB.

![Crescimento do TempDB](../../images/09-monitoramento/tempdb/lab1/tempdb-crescimento.png)

### Alerta

Para o laboratório, configurei o alerta de utilização do arquivo MDF do TempDB com limite de 70% de utilização e tamanho mínimo de 100 MB.

Após o crescimento, o alerta foi disparado indicando que os arquivos de dados haviam ultrapassado o limite configurado.

![Alerta de crescimento do TempDB](../../images/09-monitoramento/tempdb/lab1/tempdb-alerta-crescimento.png)

### Investigação

Para identificar a sessão responsável pelo consumo do TempDB, consultei a utilização de espaço por sessão.

A análise identificou a sessão responsável pela maior parte do consumo de objetos de usuário no TempDB.

![Sessão responsável pelo consumo](../../images/09-monitoramento/tempdb/lab1/tempdb-sessao-responsavel.png)

Em seguida, consultei informações da sessão para identificar o login, computador de origem, aplicação e estado da conexão.

Também utilizei o `DBCC INPUTBUFFER` para verificar o último comando executado pela sessão e relacioná-lo ao teste realizado.

### Análise durante o incidente

Depois de identificar a sessão responsável, consultei novamente a utilização interna do TempDB para analisar o consumo durante o incidente.

![Utilização do TempDB durante o incidente](../../images/09-monitoramento/tempdb/lab1/tempdb-utilizacao-incidente.png)

### Tratamento

Como o consumo estava relacionado à tabela temporária utilizada no teste, realizei a remoção do objeto temporário.

```sql
DROP TABLE #TempDB_Test;
```

Após a remoção, o espaço utilizado pelos objetos temporários foi liberado.

![TempDB após o tratamento](../../images/09-monitoramento/tempdb/lab1/tempdb-apos-tratamento.png)

### Tamanho físico após o tratamento

Mesmo após a liberação do espaço interno, os arquivos físicos do TempDB permaneceram com o tamanho que haviam atingido durante o crescimento.

![Tamanho físico após o tratamento](../../images/09-monitoramento/tempdb/lab1/tempdb-tamanho-apos-tratamento.png)

Esse comportamento demonstra a diferença entre liberar espaço utilizado dentro do arquivo e reduzir fisicamente o tamanho do arquivo.

### Alerta CLEAR

Após o tratamento, executei novamente a rotina de monitoramento.

Como a condição que havia provocado o alerta deixou de existir, o sistema gerou uma mensagem de CLEAR indicando que a utilização dos arquivos do TempDB estava novamente abaixo do limite configurado.

![Alerta CLEAR do TempDB](../../images/09-monitoramento/tempdb/lab1/tempdb-alerta-clear.png)

---

## Lab 2

### Cenário

Neste segundo laboratório, simulei um cenário em que uma sessão provoca o crescimento natural do TempDB durante uma operação controlada.

O objetivo foi acompanhar o crescimento dos arquivos, identificar a sessão responsável pelo consumo, realizar o tratamento da sessão e analisar o comportamento do TempDB após a liberação do espaço.

### Configuração inicial

Para este laboratório, os oito arquivos de dados do TempDB estavam configurados com 1 GB cada.

O crescimento automático dos arquivos estava configurado em 64 MB.

O alerta `Tempdb MDF File Utilization` permaneceu habilitado, com limite de utilização de 70% e tamanho mínimo de arquivo de 100 MB.

![Estado inicial do TempDB](../../images/09-monitoramento/tempdb/lab2/tempdb-estado-inicial-kill%282%29.png)

### Crescimento natural

Criei uma tabela temporária e iniciei uma transação com inserções sucessivas.

A carga foi mantida em execução para permitir o acompanhamento do consumo do TempDB e do crescimento automático dos arquivos.

Para acompanhar a evolução, utilizei uma consulta baseada no material de Fabrício Lima.

![Crescimento do TempDB - etapa 1](../../images/09-monitoramento/tempdb/lab2/tempdb-crescimento-01%281%29.png)

![Crescimento do TempDB - etapa 2](../../images/09-monitoramento/tempdb/lab2/tempdb-crescimento-02%281%29.png)

![Crescimento do TempDB - etapa 3](../../images/09-monitoramento/tempdb/lab2/tempdb-crescimento-03%281%29.png)

![Crescimento do TempDB - etapa 4](../../images/09-monitoramento/tempdb/lab2/tempdb-crescimento-04%281%29.png)

Durante o processo, os arquivos atingiram níveis elevados de utilização e o SQL Server realizou o crescimento automático conforme a necessidade.

### Identificação da sessão

Utilizei o `sp_whoisactive` para identificar a sessão responsável pelo consumo do TempDB.

A sessão identificada apresentava consumo de TempDB e uma transação aberta, permitindo relacionar a atividade ao crescimento observado.

![Sessão identificada pelo sp_whoisactive](../../images/09-monitoramento/tempdb/lab2/tempdb-whoisactive-kill%281%29.png)

### Tratamento da sessão

Após identificar a sessão responsável pelo consumo, utilizei o comando `KILL` para encerrá-la no ambiente de laboratório.

```sql
KILL <session_id>;
```

![KILL da sessão](../../images/09-monitoramento/tempdb/lab2/tempdb-kill-session%281%29.png)

### Liberação do espaço

Após o encerramento da sessão, consultei novamente a utilização do TempDB.

O resultado demonstrou que o espaço utilizado pela sessão foi liberado.

![Utilização do TempDB após o KILL](../../images/09-monitoramento/tempdb/lab2/tempdb-apos-kill.png)

### Tamanho físico dos arquivos

Mesmo após a liberação do espaço interno, os arquivos de dados permaneceram fisicamente maiores que o tamanho inicial.

Os arquivos de dados estavam em aproximadamente 1,28 GB.

![Tamanho físico após o KILL](../../images/09-monitoramento/tempdb/lab2/tempdb-tamanho-apos-kil.png)

Esse comportamento demonstra a diferença entre liberar espaço dentro do arquivo e reduzir fisicamente o tamanho do arquivo.

### SHRINKFILE

Como parte do experimento, utilizei o `DBCC SHRINKFILE` no arquivo `tempdev`, com o objetivo de demonstrar a redução física de um arquivo após a liberação do espaço utilizado.

```sql
USE tempdb;
GO

DBCC SHRINKFILE
(
    tempdev,
    1024
);
GO
```

O arquivo `tempdev` foi reduzido de aproximadamente 1,28 GB para 1 GB.

![Resultado do SHRINKFILE](../../images/09-monitoramento/tempdb/lab2/tempdb-shrink-tempdev.png)

Após a execução, validei novamente o tamanho físico dos arquivos.

![TempDB após o SHRINKFILE](../../images/09-monitoramento/tempdb/lab2/tempdb-apos-shrink.png)

### Restart do SQL Server

Por fim, reiniciei a instância do SQL Server para observar o comportamento do TempDB após sua recriação.

![TempDB após o restart](../../images/09-monitoramento/tempdb/lab2/tempdb-apos-restart.png)

Após o restart, os arquivos de dados foram recriados com 1 GB cada e o arquivo de log voltou para 8 MB.

Também validei novamente a utilização do TempDB.

![Utilização do TempDB após o restart](../../images/09-monitoramento/tempdb/lab2/tempdb-utilizacao-apos-restart.png)

### Observações

As operações de `KILL`, `SHRINKFILE` e restart foram realizadas exclusivamente em ambiente de laboratório, com o objetivo de compreender o comportamento do TempDB durante cenários controlados.

O `KILL` deve ser utilizado somente após a investigação da sessão e avaliação do impacto de seu encerramento. Em ambiente produtivo, o encerramento de uma sessão pode provocar rollback e impactos para a aplicação.

O `SHRINKFILE` não deve ser utilizado como rotina de manutenção para tratar o crescimento recorrente do TempDB. Antes de considerar um shrink, é necessário investigar a causa do crescimento e avaliar se o tamanho atual é necessário para a carga de trabalho.

O fato de o SHRINK ter funcionado neste laboratório não significa que ele será apropriado ou conseguirá reduzir um arquivo em todos os cenários.

Da mesma forma, o restart do SQL Server não deve ser utilizado como mecanismo de tratamento de incidentes relacionados ao crescimento do TempDB em ambiente produtivo, pois provoca indisponibilidade da instância e das aplicações dependentes dela.

Neste laboratório, essas operações foram utilizadas exclusivamente para demonstrar tecnicamente a diferença entre consumo de espaço, liberação de espaço, crescimento automático e redução física dos arquivos.
