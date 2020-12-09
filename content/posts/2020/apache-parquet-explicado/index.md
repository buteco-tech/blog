---
title: "Apache Parquet explicado"
summary: "Apache Parquet é um formato de armazenamento colunar disponível em todos os projetos que pertencem ao ecossistema Hadoop, independente do modelo de processamento, framework ou linguagem usada."
tagline: "Com exemplos em Pandas e Spark"
date: 2020-11-25 08:00:00
slug: apache-parquet-explicado
authors:
  - alexandrevicenzi
categories:
  - data-science
tags:
  - spark
  - apache
  - parquet
  - pandas
  - python
resources:
  - name: thumbnail
    src: apache-parquet.png
    title: Apache Parquet
  - name: thumbnail-wide
    src: apache-parquet-wide.png
    title: Apache Parquet
  - name: parquet-structure
    src: apache-parquet-structure.png
    title: Apache Parquet
  - name: nb-pandas
    src: nb-pandas.ipynb
    title: Notebook Pandas
  - name: nb-spark
    src: nb-spark.ipynb
    title: Notebook Spark
---

{{<figure-res name="thumbnail-wide" alt="Apache Parquet" quality="100" filter="box">}}

[Apache Parquet][pq-doc] é um formato de armazenamento colunar disponível em todos os projetos que pertencem ao ecossistema Hadoop, independente do modelo de processamento, framework ou linguagem usada.

O projeto iniciou em uma parceria entre o Twitter e a Cloudera, e a primeira versão foi liberada em 2013. Desde 2015 o projeto faz parte da Apache Software Foundation.

Diferente dos modelos tradicionais de armazenamento que usam abordagem orientada a linhas, o Parquet armazena os dados de forma colunar plana, onde os valores das colunas são armazenadas de forma adjacente as outras, esse modelo possui alguns benefícios:

- Compressão por colunas é mais eficiente
- Algorítimo de compressão pode ser especificado por coluna
- Queries "wide" (que usam várias colunas) são mais eficientes

Parquet foi criado para suportar compressão e codificação de forma eficiente, sendo possível especificar compressão por coluna, além de ser otimizado para trabalhar com estruturas de dados complexas em massa. Todos esses benefícios são em virtude do algorítimo *record shredding and assembly algorithm* descrito no *[Dremel paper][dremel]* da Google e implementado como parte do *core* do Parquet.

Podemos dividir um arquivo Parquet em duas partes basicamente:

- Dados
- Metadados

Na parte de dados ainda podemos descrever os seguintes itens:

- Row group — Particionamento lógica dos dados em linhas
- Column chunk — Um *chunk* de dados em uma coluna em particular
- Page — Column chunk também são divididos em páginas

Já na parte de metadados temos dados como versão, informações sobre as colunhs, entre outras informações que são utilizadas durante a leitura do arquivo.

{{<figure src="apache-parquet-structure.png" caption="Estrutura de um arquivo Parquet">}}

Em um [comparativo disponibilizado pela Databricks][databricks-comp] em relação ao uso de CSV ou Parquet podemos observar:

| Formato | Espaço utilizado | Tempo de execução | Escaneado |
|:-------:|:-------:|:--------:|:-------:|
| CSV     | 1 TB    | 236 seg  | 1.15 TB |
| Parquet | 130 GB  | 6.78 seg | 2.51 GB |

O uso de Parquet reduziu o espaço de armazenamento em 87%, escaneou 99% menos dados e executou 34x mais rápido em determinadas operações.

É possível utilizar Parquet em diversas ferramentas do ecossistema Hadoop como, por exemplo:

- [Spark][spark]
- [Hive][hive]
- [Impala][impala]
- [Drill][drill]
- [Pig][pig]

Mas chega de teoria, vamos colocar a mão na massa.

No exemplo de hoje vou explicar como utilizar Parquet com [Pandas][pandas] e [Spark][spark].

## Pandas

O [Pandas][pandas] é uma biblioteca para análise e manipulação de dados em Python.

Neste exemplo vamos converter uma lista de dicionários, salvar no formato Parquet e após vamos carregar o arquivo gerado para conferência.

{{<jupyter src="content/posts/2020/apache-parquet-explicado/nb-pandas.ipynb" >}}

## Spark

O [Spark][spark] é uma *engine* para processamento e análise de dados, e o exemplo é bem simples.

Vamos usar o Spark para salvar um DataFrame no formato Parquet, após salvo vamos carregar o arquivo gerado e executar queries SQL em cima dos dados.

{{<jupyter src="content/posts/2020/apache-parquet-explicado/nb-spark.ipynb" >}}

## Conclusão

O artigo de hoje foi mais para explicar o que é Parquet e porque ele pode ser uma melhor alternativa de armazenamento em comparação a CSV e JSON.

Se você já faz uso de ferramentas como Spark ou outras da Apache, será inevitável uma migração no futuro para um ganho em desempenho.

A única desvantagem que eu vejo em comparação a outros formatos é que Parquet é um formato binário, sendo assim, é necessária alguma ferramenta externa para visualizar os dados, no caso de CSV ou JSON basta abrir em algum editor de texto, ou planilha, mas tecnicamente isso irá funcionar apenas para arquivos pequenos.

Dúvidas ou sugestões? Deixe um comentário.

**Notebooks**

- <a href="nb-pandas.ipynb" download>Exemplo com Pandas</a>
- <a href="nb-spark.ipynb" download>Exemplo com Spark</a>

**Referências**

- [Documentação do Apache Parquet][pq-doc]
- [Documentação do Spark - Parquet Files][spark-pq]

[pq-doc]: https://parquet.apache.org/documentation/latest/
[dremel]: https://research.google/pubs/pub36632/
[databricks-comp]: https://databricks.com/glossary/what-is-parquet
[spark]: https://spark.apache.org/
[hive]: https://hive.apache.org/
[impala]: https://impala.apache.org/
[drill]: http://drill.apache.org/
[pig]: https://pig.apache.org/
[pandas]: https://pandas.pydata.org/
[spark-pq]: https://spark.apache.org/docs/latest/sql-data-sources-parquet.html
