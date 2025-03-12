# Análise do banco de dados Bike Store com Power BI
Projeto de análise de dados do banco de dados Bike Store com Power BI.
<br>
## 📌 Descrição
Este projeto tem como objetivo explorar e analisar os dados do banco de dados Bike Store, utilizando Power BI para criar dashboards interativos e obter insights sobre vendas, clientes e desempenho das lojas. Foram aplicadas técnicas de modelagem de dados e DAX para transformar os dados brutos em informações estratégicas.

O repositório inclui capturas dos dashboards e o arquivo .pbix para replicação da análise. <br>
<br>
Ferramenta: Power BI📊
<br>
## 🎲 O banco de dados
OS dados utilizados no projeto são do banco de dados Bike Store, da Microsoft; esse banco de dados é bastante conhecido por estudantes de análise de dados. Trata-se de um banco de dados relacional e contém informações sobre vendas, clientes e estoque.

### Conhecendo o banco de dados<br>
O banco de dados utilizado para esse estudo é facilmente encontrado para Download e uma breve busca no google. para or projeto foi baixado no site da Kaggle, uma plataforma conhecida entre os análistas de dados para compartilhamento de análises e também por conter diversos database disponiveis para estudo.##
Após o Download do dataset foi utilizado o SQL Server para conexão e análise exploratória dos dados do Bike Store. Você pode conferir as Querys que foram feitas para explorar o banco de dados em outro [repositório](https://github.com/RenanMoliveir/Portifolio_Analise_BikeStore), nesse repositório o foco é no estudo através do Power BI.<br>
Após algumas análises através de SQL observou-se que o modelo de dados podia ser melhorado para obter-se mais perfomance das análises.<br>
A seguir toda a documentação dos passos feitos para o processo de modelagem de dados através do PowerQuery<br>
#### Tabela fact_sales
A tabela fact_Sales originalmente contia algumas colunas de informações que foram removidas e também foram alterados o tipo de dados para melhorar a interação da tabela nas análises. Esse processo de modelagem dos dados no PowerQuery é necessário para se ter uma tabela fato "limpa", com o tipo de dados correto, além de melhorar o relacionamento com as outras tabela. As colunas de ID foram aletaradas para o tipo "Text", pois originalmente ela foi carregada com o tipo "number"; seguindo essa lógica, as colunas que não seriam usadas para somar seus valores foram alterados. Isso foi feito para as colunas de IDs de Orders, Store, Brand, Category, Product e Customer. O principal objetivo foi remover dados de características das vendas, concentrando apenas os dados de número da venda, ou seja, todas as características, como por exemplo, das lojas, dos produtos e clientes foram tratados em tabelas dimensão, cada um com uma tabela única, isso permite que se tenha um modelo de dados muito bem organizado e melhora as análises DAX, poupando processamento ou até mesmo fórmulas gigantescas e de difícil lógica. Todas as tabelas "dimensão" foram carregadas e modeladas separamente para melhorar o modelo e deixar apenas as colunas necessárias para análise.<br>
#### PowerQuery
= Table.TransformColumnTypes(Sales_Order,{{"OrderID", type text}, {"CustomerID", type text}})<br>
= Table.TransformColumnTypes(#"Linhas Filtradas",{{"Status", type text}, {"OrderDate", type date}, {"RequiredDate", type date}, {"StoreID", type text}, {"EmployeeID", type text}})<br>
= Table.TransformColumnTypes(#"Colunas Renomeadas",{{"ProductID", type text}})<br>
= Table.RenameColumns(#"Tipo Alterado3",{{"Item ID", "ItemID"}, {"Sales.OrderItem.Quantity", "OrderItem.Quantity"}, {"Sales.OrderItem.ListPrice", "Price"}, {"Sales.OrderItem.Discount", "Discount"}, {"Sales.OrderItem.LineTotal", "Total"}})<br>
Abaixo a tabela fact_sales:
<br>
<br>
![1-fact_sales](https://github.com/user-attachments/assets/9c1f60c4-584b-4607-a684-79a60ed6325d)



