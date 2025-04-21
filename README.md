# Azure-Synapse-Analytcs

<h3>Definição</h3>
<p><b>Azure Synapse Analytics</b> é um serviço de análise da Microsoft que combina data warehousing empresarial e análise de Big Data num só lugar. Permite consultar dados com ou sem servidor, integrando várias ferramentas como ETL/ELT, Machine Learning e Power BI.</p>
 
<h3>Arquitetura de Consumo Modular - modelos de consumo: </h3>
<p><strong>Dedicated SQL pool:</strong>Data warehouse tradicional com desempenho e custo previsíveis, escalável ajustando as unidades de data warehouse (DWUs).</p>
<p><strong>Serverless SQL pool:</strong> Arquitetura de consulta sob demanda (pague por TB processado) para análise de dados no data lake.</p>
<p><strong>Apache Spark para Synapse:</strong> Integração profunda com o popular motor de Big Data para preparação de dados, engenharia de dados, ETL e Machine Learning. Inclui pools de Spark gerenciados.</p>
<p><strong>Synapse Pipelines:</strong> Serviço de integração de dados (ETL/ELT) baseado no Azure Data Factory, permitindo criar fluxos de trabalho para mover e transformar dados de diversas fontes (mais de 90 conectores).</p>
<p><strong>Synapse Studio:</strong> Interface de usuário unificada baseada na web que permite aos cientistas de dados, engenheiros de dados e administradores de banco de dados acessar e gerenciar todos os componentes do Synapse.</p>
<p><strong>Azure Data Lake Storage:</strong> Camada de armazenamento escalável e econômica otimizada para cargas de trabalho de Big Data, servindo como base para o Synapse.
Em resumo, a arquitetura do Azure Synapse Analytics é projetada para processamento paralelo massivo (MPP), separando computação de armazenamento para escalabilidade independente e oferecendo diferentes mecanismos de computação (SQL dedicado, SQL sem servidor e Spark) para atender a diversos casos de uso analíticos dentro de um ambiente integrado.</p>

<h3>Arquitetura de Computação distribuída</h3>
<p> O usuário tem acesso ao nó de controle(driver), quando recebe a requisição são divididas em tarefas menores(nós), após terminado o processo é devolvida ao nó de controle.No exemplo apresentado, utiliza-se o polaris, para realizar esse distribuição, conforme a imagem seguir:</p>

![image](https://github.com/user-attachments/assets/ecfc4f6c-d808-40a7-b53f-0f243a66ea6a)

<strong>Pontos</strong>
- Serveless e Servidor <br>
- Mecanismo de consulta distribuída <br>
- Robusto <br>
- Query utiliza T-SQL <br>
- Pay-per-Query <br>
- Não armazena dados, os consome <br>
- Synapse Link <br>

<hr>
<h3>OPENROWSET Function Overview</h3>

<p>O comando OPENROWSET no Azure Synapse Analytics (especificamente no pool de SQL sem servidor) é uma função Transact-SQL (T-SQL) que permite ler o conteúdo de arquivos diretamente de uma fonte de dados externa e retornar esses dados como um conjunto de linhas (rowset), como se fosse uma tabela.</p>

<p>Em termos simples, o OPENROWSET te permite consultar dados em arquivos (como CSV, Parquet, Delta Lake) armazenados no Azure Storage (Azure Data Lake Storage ou Azure Blob Storage) sem a necessidade de primeiro carregar esses dados em uma tabela do Synapse Analytics.</p>

<strong>Principais pontos sobre o OPENROWSET:</strong>

<p>-Acesso Ad-Hoc a Arquivos: É ideal para explorar rapidamente o conteúdo de arquivos, realizar transformações básicas ou executar consultas únicas sem a sobrecarga de criar tabelas externas.</p>

<p>-Suporte a Diversos Formatos: Funciona com formatos de arquivo comuns para análise de dados, como CSV, Parquet e Delta Lake.
Especificação da Fonte de Dados: Você pode especificar explicitamente a localização dos arquivos diretamente na função OPENROWSET ou referenciar uma fonte de dados externa já configurada no Synapse Analytics.</p>

<p>-Definição de Schema (Opcional): Você pode definir o schema dos dados no arquivo usando a cláusula WITH, o que é útil para garantir os tipos de dados corretos e melhorar o desempenho da consulta. Se omitido, o Synapse tentará inferir o schema. </p>

<p>-Autenticação: O OPENROWSET lida com a autenticação para acessar o armazenamento subjacente, suportando diferentes métodos como Microsoft Entra pass-through (para logins do Microsoft Entra), tokens SAS (para logins SQL) e identidade gerenciada do workspace. </p>

<p>-Uso na Cláusula FROM: A função OPENROWSET é usada na cláusula FROM de uma instrução SELECT como se fosse o nome de uma tabela. </p>

<strong>Sintaxe</strong>

> OPENROWSET( <br>
    BULK 'caminho/para/o/arquivo(s)', <br>
    FORMAT = 'formato_do_arquivo' <br>
    [, DATA_SOURCE = 'nome_da_fonte_de_dados'] <br>
    [, PARSER_VERSION = 'versao_do_parser'] <br>
    [, <opções_específicas_do_formato>] <br>
) [AS] nome_da_tabela_virtual (coluna1 tipo1, coluna2 tipo2, ...)

 <p>CSV-Exemplo prático</p>
 
 >SELECT TOP 10 * <br>
FROM OPENROWSET( <br>
    BULK 'https://<sua_conta>.blob.core.windows.net/<seu_container>/dados.csv', <br>
    FORMAT = 'CSV', <br>
    --FIRSTROW = 2, -- Ignorar a primeira linha (cabeçalho) <br>
    FIELDTERMINATOR = ',', <br>
    HEADER_ROW = TRUE, <br>
    ROWTERMINATOR ='\n' <br>
) AS dados;

<p>HEAD_ROW=TRUE - Possui cabeçalho</p>
<p>FIEDTERMINATOR - Delimitador </p>
<p>ROWTERMINATOR - Ultimo caractere de considera um linha</p>

<strong>Atribuindo nome e tipo as colunas</strong>
<p>Exemplo 1 - Atribuindo nome a coluna e tipo</p>

![image](https://github.com/user-attachments/assets/96ef6259-ae76-4b75-9a91-f655766e3432)
<p>Collate, utiliza-se para converter o tipo do dado por região.</p>

<p>Exemplo 2 - Atribuindo nome e tipagem por posição</p>

![image](https://github.com/user-attachments/assets/4f723bc9-210d-4e05-bc63-8a0b1b367df7)

<STRONG>Criando um Schema com base externa</strong>

> CREATE EXTERNAL DATA SOURCE nome_schema <br>
  WITH(<br>
      LOCATION = 'abfss://#'
  )

<Strong>Cenários em que temos virgulas como separador e no conteúdo da colunas</strong>

<p>Existem duas formas de trabalhar com o cenário em que existem virgulas, no conteúdo da coluna.</p>
<p>Exemplo 1 - ESCAPECHAR </p>

![image](https://github.com/user-attachments/assets/a48a4e6f-4fb0-4c8f-87f2-746da4d378de)


<p>Exemplo 2 - FIELDQUOTE</p> 

![image](https://github.com/user-attachments/assets/fd59d922-994e-4245-9399-3ab898e7b5be)

<strong>Cenário que o espaçamento como delimitador</strong>
<p>No exemplo a seguir, basta adicionar FIELDTERMINATOR '\t', conforme a imagem a seguir:</p>

![image](https://github.com/user-attachments/assets/b3ae457a-01cb-458a-b826-ef0f0ee812ae)

<hr>

<h3>Trabalhando com o Formato JSON - Serveless SQL POOL</h3>
<Strong>JSON In-line</Strong>
<p>Sintaxe</p>

> {"name":"Nome1","Idade":"14"}
<p> Exemplo 1 </p>
<p>Nota: Alterar o tipo FIELDTERMINATOR = '0x0b' | FIELDQUOTE = '0x0b' | ROWTERMINATOR = '0x0a' | PARSE_VERSION='1.0' e adicionar no WTIH jsonDoc nvarchar(max) </p>
<p>Resultado</p>

> {"name":"Nome1","Idade":"14"}
<p>Exemplo 2</p>
<Strong>Separando as colunas</Strong>
<p>Adiciona o JSON_VALUE e adiciona os campos individualmente por coluna.</p>

![image](https://github.com/user-attachments/assets/1457d84e-90e0-4ad6-872c-b8df0c8460c8)

<p>Exemplo 3 - OPENJSON </p>
<p> Abrindo arquivo direto com o comando openjson, seguindo o mesmo exemplo anterior adiciona no final após o WITH.</p>

> CROSSAPPLY OPENJSON(jsonDoc)

<p>Exemplo 4 - JSON ARRAY</p>

EM Construção ...
