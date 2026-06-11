# 🛒 Pipeline de Dados E-commerce: Arquitetura Medallion com Databricks e PySpark

![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/Apache,_Spark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)
![Delta Lake](https://img.shields.io/badge/Delta_Lake-00ADD8?style=for-the-badge&logo=delta-lake&logoColor=white)

## 📝 Descrição do Projeto
Este projeto demonstra a implementação de um pipeline de engenharia de dados ponta a ponta utilizando a arquitetura **Medallion (Bronze, Silver e Gold)**. O objetivo principal é extrair inteligência de negócio a partir de um conjunto de dados reais de e-commerce, identificando os produtos líderes em faturamento e volume de vendas.

O desenvolvimento foi realizado inteiramente no ambiente **Databricks Community Edition**, utilizando **PySpark** para o processamento distribuído e o formato **Delta Lake** para garantir a confiabilidade e performance no armazenamento das tabelas.

---

## 📊 O Banco de Dados
Os dados utilizados pertencem ao **Brazilian E-Commerce Public Dataset by Olist**, um conjunto de dados públicos e anonimizados de vendas reais em marketplaces brasileiros.

---

## 🏗️ Arquitetura do Pipeline (Medallion)

O ecossistema de dados foi estruturado em três camadas lógicas dentro do Unity Catalog (`workspace.default`):

### 1. 🥉 Camada Bronze (Ingestão)
* **Objetivo:** Armazenar os dados brutos exatamente como vieram da origem, garantindo a rastreabilidade do histórico.
* **Processamento:** Leitura do catálogo original carregado via arquivo, adição de metadados operacionais (`data_ingestao` com o timestamp atual) e gravação no formato Delta.
* **Tabela Gerada:** `bronze_olist_pedidos`

### 2. 🥈 Camada Silver (Limpeza e Qualidade)
* **Objetivo:** Limpar, padronizar e enriquecer os dados para consumo analítico confiável.
* **Processamento:** 
  * Filtragem de registros inconsistentes (remoção de linhas com valores nulos em chaves primárias como `product_id` e `price`).
  * Eliminação de registros duplicados com base na regra de negócio do pedido (`dropDuplicates`).
* **Tabela Gerada:** `silver_olist_pedidos`

### 3. 🥇 Camada Gold (Negócio / Agregações)
* **Objetivo:** Disponibilizar visões agregadas e prontas para consumo por ferramentas de BI (como Power BI ou Tableau) ou analistas de negócios.
* **Processamento:** Agrupamento dos dados limpos da camada Silver por produto (`product_id`), realizando cálculos de agregação de faturamento total acumulado (`sum`) e volume total de pedidos atendidos (`count`). Os dados são ordenados de forma decrescente para destacar os maiores geradores de receita.
* **Tabela Gerada:** `gold_produtos_mais_vendidos`

---

## 📈 Resultados e Insights Visualizados
<img width="1366" height="768" alt="img" src="https://github.com/user-attachments/assets/b4cf6d5a-30d1-4a2b-a170-174c4aa341f1" />
O pipeline consolida os indicadores cruciais de performance de produto, permitindo que a gestão identifique imediatamente quais itens geram maior receita para a operação do e-commerce, conforme ilustrado no gráfico de barras gerado de forma nativa no ambiente Databricks.

---

## 🛠️ Tecnologias Utilizadas

* **Ambiente Cloud:** Databricks (Serverless Compute)
* **Linguagem:** Python / PySpark
* **Mecanismo de Armazenamento:** Delta Lake (Tabelas Delta)
* **Visualização de Dados:** Databricks Native Charts

---

## 🚀 Como Executar este Projeto
1. Crie uma conta gratuita no **Databricks Community Edition**.
2. Faça o download do dataset da Olist no Kaggle.
3. Importe os dados utilizando a interface "Create or modify table from file upload" no menu Catalog.
4. Importe o notebook `Pipeline_Medallion_Olist_Ecommerce.ipynb` disponibilizado neste repositório para o seu workspace.
5. Execute as células sequencialmente utilizando o atalho `Shift + Enter`.
