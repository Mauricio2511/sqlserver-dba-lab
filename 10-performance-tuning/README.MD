# Indice

## Objetivo

Realizar uma prática de **Index Tuning** no SQL Server, analisando o comportamento de uma consulta antes e depois da criação de um índice.

Durante o exercício foram utilizados:

- SQL Server
- SSMS (SQL Server Management Studio)
- Banco de dados `StackOverflow2010`
- Tabela `dbo.Badges`
- `SET STATISTICS IO ON`
- Plano de execução
- Análise dos índices existentes
- Criação de índice não clusterizado

O objetivo foi identificar uma consulta que pudesse se beneficiar de um índice e validar, na prática, a redução da quantidade de páginas lidas pelo SQL Server.

---

## Etapa 1 - Validação dos índices existentes

Antes da criação de um novo índice, foi realizada a análise dos índices existentes na tabela `dbo.Badges`.

### Comando utilizado

    USE StackOverflow2010;
    GO

    SELECT
        i.name AS index_name,
        i.type_desc,
        i.is_unique,
        c.name AS column_name
    FROM sys.indexes AS i
    INNER JOIN sys.index_columns AS ic
        ON i.object_id = ic.object_id
        AND i.index_id = ic.index_id
    INNER JOIN sys.columns AS c
        ON ic.object_id = c.object_id
        AND ic.column_id = c.column_id
    WHERE i.object_id = OBJECT_ID('dbo.Badges')
    ORDER BY
        i.index_id,
        ic.key_ordinal;
    GO

A análise permitiu verificar quais índices já estavam disponíveis antes da criação do novo índice.

### Evidência

**Screenshot para o GitHub:**

`images/10-performance-tuning/validacao-indices-atuais.png`

![Validação dos índices atuais](../images/10-performance-tuning/validacao-indices-atuais.png)

---

## Etapa 2 - Análise da consulta

Foi criada uma consulta utilizando filtros sobre as colunas `Name`, `Date` e `UserId`.

### Consulta utilizada

    USE StackOverflow2010;
    GO

    SET STATISTICS IO ON;
    SET STATISTICS TIME ON;
    GO

    SELECT
        Id,
        UserId,
        Name,
        Date
    FROM dbo.Badges
    WHERE Name = 'Enlightened'
      AND Date >= '2010-01-01'
      AND Date < '2011-01-01'
      AND UserId > 10000;
    GO

Antes da criação do índice, foram analisadas as condições utilizadas no `WHERE`, buscando entender quais colunas poderiam contribuir para uma melhor filtragem dos registros.

A tabela `dbo.Badges` possui aproximadamente **1,1 milhão de registros**, permitindo observar com clareza o impacto da criação do índice.

---

## Etapa 3 - Execução inicial e análise de desempenho

A consulta foi executada antes da criação do índice para obter um cenário inicial e posteriormente comparar os resultados após a otimização.

O plano de execução inicial apresentou um:

**Clustered Index Scan**

A consulta realizou:

**6.547 páginas lidas**

Esse resultado foi utilizado como referência para a comparação.

### Evidências

**Screenshot para o GitHub:**

`images/10-performance-tuning/plano-de-execucao-previsto-atual.png`

![Plano de execução previsto antes do índice](../images/10-performance-tuning/plano-de-execucao-previsto-atual.png)

**Screenshot para o GitHub:**

`images/10-performance-tuning/plano-de-execucao-usado-atual.png`

![Plano de execução utilizado antes do índice](../images/10-performance-tuning/plano-de-execucao-usado-atual.png)

**Screenshot para o GitHub:**

`images/10-performance-tuning/detalhes-execucao-previsto-atual.png`

![Detalhes da execução prevista antes do índice](../images/10-performance-tuning/detalhes-execucao-previsto-atual.png)

**Screenshot para o GitHub:**

`images/10-performance-tuning/detalhes-execucao-usado-atual.png`

![Detalhes da execução utilizada antes do índice](../images/10-performance-tuning/detalhes-execucao-usado-atual.png)

**Screenshot para o GitHub:**

`images/10-performance-tuning/qtd-paginas-lidas-atual.png`

![Quantidade de páginas lidas antes do índice](../images/10-performance-tuning/qtd-paginas-lidas-atual.png)

---

## Etapa 4 - Criação do índice

Após a análise da consulta, foi criado um índice não clusterizado utilizando as colunas presentes nos filtros da consulta.

### Índice criado

    CREATE NONCLUSTERED INDEX noclus_Badges_name
    ON dbo.Badges
    (
        Name,
        UserId,
        Date
    )
    WITH (FILLFACTOR = 90);
    GO

A ordem das colunas foi definida considerando a análise realizada durante o exercício.

A coluna `Name` foi utilizada como primeira coluna do índice por apresentar maior seletividade para o cenário analisado.

### Evidência

**Screenshot para o GitHub:**

`images/10-performance-tuning/criacao-indice-name.png`

![Criação do índice](../images/10-performance-tuning/criacao-indice-name.png)

---

## Etapa 5 - Validação do novo índice

Após a criação, foi realizada uma nova consulta para confirmar que o índice estava disponível na tabela `dbo.Badges`.

A validação permitiu verificar a presença do novo índice junto aos demais índices da tabela.

### Evidência

**Screenshot para o GitHub:**

`images/10-performance-tuning/validacao-indices-novo-indice.png`

![Validação do novo índice](../images/10-performance-tuning/validacao-indices-novo-indice.png)

---

## Etapa 6 - Nova execução após a criação do índice

A mesma consulta foi executada novamente após a criação do índice para comparar o comportamento do SQL Server.

Após a criação do índice, o plano de execução passou a utilizar:

**Index Seek**

A quantidade de páginas lidas caiu de:

**6.547 → 91 páginas**

Isso representa uma redução de aproximadamente:

**98,6%**

na quantidade de páginas lidas pelo SQL Server.

### Evidências

**Screenshot para o GitHub:**

`images/10-performance-tuning/plano-de-execucao-apos-indice.png`

![Plano de execução após o índice](../images/10-performance-tuning/plano-de-execucao-apos-indice.png)

**Screenshot para o GitHub:**

`images/10-performance-tuning/detalhes-execucao-apos-indice.png`

![Detalhes da execução após o índice](../images/10-performance-tuning/detalhes-execucao-apos-indice.png)

**Screenshot para o GitHub:**

`images/10-performance-tuning/qtd-paginas-lidas-apos-indice.png`

![Quantidade de páginas lidas após o índice](../images/10-performance-tuning/qtd-paginas-lidas-apos-indice.png)

---

## Comparação dos resultados

| Métrica | Antes do índice | Após o índice |
|---|---:|---:|
| Plano de execução | Clustered Index Scan | Index Seek |
| Páginas lidas | 6.547 | 91 |
| Redução | — | ~98,6% |

A comparação demonstra que o novo índice permitiu ao SQL Server localizar os registros necessários de forma mais eficiente, evitando a leitura de uma grande quantidade de páginas da tabela.

---

## Etapa 7 - Análise do resultado

A criação do índice foi validada por meio de três pontos principais:

- mudança do plano de execução de **Clustered Index Scan** para **Index Seek**;
- redução da quantidade de páginas lidas;
- utilização do novo índice pela consulta.

O resultado mostrou, na prática, como a escolha adequada de um índice pode reduzir significativamente o trabalho realizado pelo SQL Server durante a execução de uma consulta.

Também foi possível observar a importância de analisar os índices existentes antes de criar novos índices, evitando a criação desnecessária de estruturas que poderiam não trazer benefício para a consulta.

---

## Conclusão

Neste laboratório foi realizada uma prática de **Performance & Tuning** utilizando análise de índices, plano de execução e estatísticas de I/O.

O principal resultado obtido foi a redução de **6.547 para 91 páginas lidas**, aproximadamente **98,6% de redução**, além da mudança do plano de execução de **Clustered Index Scan** para **Index Seek**.

A prática permitiu observar diretamente o impacto que um índice pode causar no desempenho de uma consulta e reforçou a importância de analisar o comportamento real da consulta antes e depois da otimização.
