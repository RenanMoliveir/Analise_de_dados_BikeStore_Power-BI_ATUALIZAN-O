# Análise do banco de dados Bike Store com Power BI

**Projeto de análise de dados do banco de dados Bike Store com Power BI.**

## 📌 Descrição

Este projeto tem como objetivo explorar e analisar os dados do banco de dados Bike Store, utilizando Power BI para criar dashboards interativos e obter insights sobre vendas, clientes e desempenho das lojas. Foram aplicadas técnicas de modelagem de dados e DAX para transformar os dados brutos em informações estratégicas.

O repositório inclui capturas de tela dos dashboards e o arquivo .pbix para que você possa replicar a análise.

**Ferramenta:** Power BI 📊

---

## 🎲 O Banco de Dados

Os dados utilizados neste projeto são provenientes do banco de dados Bike Store da Microsoft. Este banco de dados relacional é bastante utilizado em estudos de análise de dados e contém informações sobre vendas, clientes e estoque.

### Conhecendo o Banco de Dados

O banco de dados pode ser facilmente encontrado para download com uma busca rápida no Google. Para este projeto, o dataset foi baixado no site Kaggle, uma plataforma amplamente conhecida entre analistas de dados, onde são compartilhadas análises e diversos bancos de dados disponíveis para estudo.

Após o download, o SQL Server foi utilizado para conectar e realizar a análise exploratória dos dados do Bike Store. Você pode conferir as queries feitas para explorar o banco de dados em outro [repositório](https://github.com/RenanMoliveir/Portifolio_Analise_BikeStore). Este repositório tem como foco o estudo através do Power BI.

Durante a análise em SQL, percebeu-se que o modelo de dados poderia ser otimizado para melhorar a performance das análises. A seguir, estão documentados os passos realizados para o processo de modelagem de dados utilizando o Power Query.

---

## 🔧 Modelagem de Dados

### Tabela fact_sales

A tabela `fact_sales` originalmente continha algumas colunas de informações que foram removidas, além de ter sido feito um ajuste no tipo de dados para melhorar a interação dessa tabela nas análises. Esse processo de modelagem de dados no Power Query é fundamental para garantir que a tabela fato esteja "limpa", com os tipos de dados corretos, e para melhorar o relacionamento com as outras tabelas.

As colunas de ID foram alteradas para o tipo **"Text"**, pois inicialmente foram carregadas como **"Number"**. Além disso, outras colunas que não seriam utilizadas para agregações de valores foram alteradas, como as colunas de IDs de `Order`, `Store`, `Brand`, `Category`, `Product` e `Customer`.

O objetivo principal foi remover dados desnecessários sobre as características das vendas, deixando apenas os dados essenciais, como o número da venda. Para isso, as características das lojas, produtos e clientes foram tratadas em tabelas separadas de **dimensão**. Esse modelo de dados bem estruturado facilita as análises DAX, além de reduzir o processamento e evitar fórmulas complexas e de difícil compreensão.

### Power Query

Aqui estão alguns exemplos de transformações aplicadas no Power Query:

```powerquery
= Table.TransformColumnTypes(Sales_Order,{{"OrderID", type text}, {"CustomerID", type text}})
= Table.TransformColumnTypes(#"Linhas Filtradas",{{"Status", type text}, {"OrderDate", type date}, {"RequiredDate", type date}, {"StoreID", type text}, {"EmployeeID", type text}})
= Table.TransformColumnTypes(#"Colunas Renomeadas",{{"ProductID", type text}})
= Table.RenameColumns(#"Tipo Alterado3",{{"Item ID", "ItemID"}, {"Sales.OrderItem.Quantity", "OrderItem.Quantity"}, {"Sales.OrderItem.ListPrice", "Price"}, {"Sales.OrderItem.Discount", "Discount"}, {"Sales.OrderItem.LineTotal", "Total"}})
```

### Tabela fact_sales (Após o Tratamento)

Abaixo está a tabela `fact_sales` após o tratamento dos dados, contendo apenas as informações necessárias sobre as vendas, com datas e IDs:

![1-fact_sales](https://github.com/user-attachments/assets/9c1f60c4-584b-4607-a684-79a60ed6325d)

---
## 📌 Tabela `dim_product`

Seguindo a mesma lógica, foram selecionados apenas os campos necessários para otimizar a **performance**.  

### 🔹 **1. Alteração de Tipo de Dados**
O primeiro passo foi garantir que cada coluna estivesse no tipo correto:  

```powerquery
= Table.TransformColumnTypes(Production_Product, {
    {"ProductID", type text}, 
    {"Name", type text}, 
    {"BrandID", type text}, 
    {"CategoryID", type text}, 
    {"ModelYear", type text}
})
```
🔹 2. Remoção de Colunas Desnecessárias
Algumas colunas não eram essenciais para a análise e foram removidas para reduzir a carga do modelo:

powerquery
```
Copiar
Editar
= Table.RemoveColumns(#"Tipo Alterado", {"Production.Category", "Sales.OrderItem"})
```
🔹 3. Junção com a Tabela dim_category
A coluna de categoria de produtos foi adicionada por meio da ferramenta Mesclar Consultas no Power Query. Isso permitiu o Merge entre as tabelas, consolidando os dados:

powerquery
```
Copiar
Editar
= Table.NestedJoin(#"Colunas Removidas1", {"CategoryID"}, dim_category, {"CategoryID"}, "dim_category", JoinKind.LeftOuter)
```
Por se tratar de uma característica dos produtos, optamos por manter essa informação diretamente na tabela dim_product, otimizando a modelagem e facilitando as análises.

