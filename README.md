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

