# 🚀 Desafio DIO: [Seu Nome do Projeto/Tema Principal, ex: API de Produtos Serverless para E-commerce] na Cloud com Azure ☁️

Este projeto foi desenvolvido como parte do Bootcamp "[Nome do Bootcamp na DIO, ex: Microsoft Azure AI Fundamentals]" da Digital Innovation One (DIO). O objetivo principal é demonstrar a aplicação prática dos conceitos de plataforma de aplicações Microsoft Azure, com foco em contêineres, arquiteturas serverless, gerenciamento de APIs e persistência de dados em nuvem.

---

## 🎯 Objetivo do Projeto

O desafio consistiu em [Descreva brevemente o que você fez. Ex: simular uma arquitetura para o gerenciamento de produtos de um e-commerce, desenvolvendo uma API RESTful e utilizando os serviços do Azure para orquestração de contêineres, gerenciamento de API e armazenamento de dados.].

---

## 💻 Tecnologias Utilizadas

Para a construção desta solução, foram utilizadas as seguintes tecnologias e serviços do Azure:

* **Azure Services:**
    * [**Azure Container Apps**](https://azure.microsoft.com/services/container-apps/) - Para hospedar a aplicação conteinerizada/microsserviço.
    * [**Azure Container Registry (ACR)**](https://azure.microsoft.com/services/container-registry/) - Para armazenar e gerenciar as imagens Docker da aplicação.
    * [**Azure SQL Database**](https://azure.microsoft.com/services/sql-database/) - Para o armazenamento persistente dos dados relacionais [ou substitua por Azure Cosmos DB, Azure Storage Account - Blob/Table, dependendo da sua escolha].
    * [**Azure API Management (APIM)**](https://azure.microsoft.com/services/api-management/) - Para criar um gateway seguro e unificado para a API [inclua se usou, caso contrário, remova].
    * [**Azure Functions**](https://azure.microsoft.com/services/functions/) - Para lógica serverless e/ou processamento de eventos [inclua se usou, caso contrário, remova].
    * Azure Resource Groups - Para organizar os recursos do Azure.
    * [Opcional: Azure Monitor / Application Insights] - Para monitoramento e telemetria da aplicação.

* **Ferramentas & Linguagens:**
    * [**Docker**](https://www.docker.com/) - Para conteinerização da aplicação.
    * [**Azure CLI**](https://docs.microsoft.com/cli/azure/install-azure-cli) - Para interagir com os serviços Azure via linha de comando.
    * [**Python 3.x**](https://www.python.org/) - Linguagem de programação utilizada para a API/Função [ou Node.js, C#, Java, etc.].
    * [**Flask**](https://flask.palletsprojects.com/en/2.0.x/) - Framework web Python para a construção da API [ou Express, ASP.NET Core, etc.].
    * [**Git & GitHub**](https://github.com/) - Para controle de versão e hospedagem do repositório.
    * [**Postman**](https://www.postman.com/) / Insomnia / `curl` - Para testar as chamadas da API.

---

## 🏛️ Arquitetura da Solução

A solução proposta consiste em [descreva brevemente o fluxo e os componentes]. A API, desenvolvida em [Linguagem/Framework], é conteinerizada e implantada no **Azure Container Apps** (ou como uma **Azure Function**). Sua imagem é gerenciada no **Azure Container Registry**. Os dados são persistidos no **Azure SQL Database** [ou seu DB escolhido]. Opcionalmente, o **Azure API Management** atua como um gateway seguro para a API, aplicando políticas e expondo-a aos consumidores.

![Diagrama da Arquitetura](assets/diagrama-solucao.png)
_Descreva seu diagrama: Ex: O diagrama acima ilustra o fluxo onde requisições de clientes podem passar pelo Azure API Management (se usado) antes de interagir com a API hospedada no Azure Container Apps (ou Azure Function), que por sua vez se conecta ao Azure SQL Database para persistência de dados. O Azure Container Registry é o repositório central para a imagem da API._

---

## 🚀 Passos para a Implementação

Aqui estão os principais passos seguidos para implementar esta solução, utilizando a Azure CLI:

1.  **Criação do Grupo de Recursos:**
    ```bash
    az group create --name [seu-resource-group-name] --location [sua-localizacao-azure, ex: eastus]
    ```

2.  **Configuração do Banco de Dados [Ex: Azure SQL Database]:**
    * Criação do servidor SQL e do banco de dados.
    * Configuração da regra de firewall para permitir acesso dos serviços Azure e, opcionalmente, do seu IP local para desenvolvimento/teste.
    ```bash
    # Exemplo: Criar servidor SQL
    az sql server create --name [seu-sql-server-name] --resource-group [seu-resource-group-name] --location [sua-localizacao] --admin-user [seu-admin-user] --admin-password [sua-senha-segura]
    # Exemplo: Criar banco de dados
    az sql db create --resource-group [seu-resource-group-name] --server [seu-sql-server-name] --name [seu-database-name] --edition GeneralPurpose --family Gen5 --capacity 2
    # Exemplo: Configurar firewall para permitir acesso de serviços Azure (essencial para Container Apps/Functions)
    az sql server firewall-rule create --resource-group [seu-resource-group-name] --server [seu-sql-server-name] --name AllowAzureServices --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
    # Opcional: Se precisar acessar do seu IP local para teste
    az sql server firewall-rule create --resource-group [seu-resource-group-name] --server [seu-sql-server-name] --name AllowMyIP --start-ip-address [seu-ip-publico] --end-ip-address [seu-ip-publico]
    ```
    * [Opcional: Detalhe sobre a criação de tabelas, ex: Criada uma tabela `Products` com colunas `Id`, `Name`, `Price` e populada com dados de exemplo.]

3.  **Criação do Azure Container Registry (ACR):**
    ```bash
    az acr create --resource-group [seu-resource-group-name] --name [seu-acr-name] --sku Basic
    ```

4.  **Desenvolvimento e Conteinerização da Aplicação/API [ou Desenvolvimento da Azure Function]:**
    * Um `Dockerfile` foi criado para definir o ambiente do contêiner e suas dependências.
    * O código da API ([ex: `app.py`]) processa requisições [ex: POST para `/products`] e interage com o banco de dados.
    * [Se usou Azure Functions: Descreva a criação da sua função, os gatilhos e bindings configurados, e a lógica do código.]

5.  **Construção e Push da Imagem Docker para o ACR:**
    ```bash
    # Logar no ACR
    az acr login --name [seu-acr-name]
    # Construir e taggear a imagem
    docker build -t [seu-acr-name].azurecr.io/[nome-da-sua-imagem]:v1 .
    # Fazer push da imagem para o ACR
    docker push [seu-acr-name].azurecr.io/[nome-da-sua-imagem]:v1
    ```

6.  **Implantação no Azure Container Apps [ou Azure Functions]:**
    * **Para Azure Container Apps:**
        ```bash
        # Exemplo: Criar ambiente de Container Apps (se já tiver, pule)
        az containerapp env create --name [seu-containerapp-env-name] --resource-group [seu-resource-group-name] --location [sua-localizacao]
        # Exemplo: Criar Container App com segredo
        az containerapp create --name [seu-containerapp-name] --resource-group [seu-resource-group-name] --environment [seu-containerapp-env-name] \
          --image [seu-acr-name].azurecr.io/[nome-da-sua-imagem]:v1 \
          --target-port [porta-da-sua-api, ex: 80] --ingress external --query properties.latestRevisionFqdn \
          --secrets "db-connection-string=[Sua_Connection_String_do_SQL_Database]" \
          --env-vars "DB_CONNECTION_STRING=secretref:db-connection-string" # Isso referencia o segredo
        ```
    * **Para Azure Functions (se você não usou contêiner e sim apenas Functions):**
        ```bash
        # Exemplo: Criar Function App (Plano de Consumo)
        az functionapp create --resource-group [seu-resource-group-name] --consumption-plan-location [sua-localizacao] \
            --name [seu-function-app-name] --runtime [dotnet|node|python|java] --runtime-version [versao-runtime] \
            --storage-account [nome-da-sua-storage-account-para-functions]
        # Exemplo: Deploy da função
        # Se você está usando func core tools: func azure functionapp publish [seu-function-app-name]
        # Se está fazendo deploy de zip: az functionapp deployment source config-zip -g [seu-resource-group-name] -n [seu-function-app-name] --src [caminho-para-seu-zip]
        ```
    * [Opcional: Detalhe como você configurou a **identidade gerenciada** para o seu Function App/Container App e atribuiu a **função RBAC** ao banco de dados.]

7.  **Configuração do Azure API Management (Opcional):**
    * Criação da instância do APIM.
    * Importação da sua API do Container App/Function App.
    * Configuração de **políticas** (ex: Rate Limit, validação de Subscription Key, transformação de requisição/resposta).
    ```bash
    # Exemplo: Criar instância do APIM (Tier Consumption é uma boa opção para testes)
    az apim create --name [seu-apim-name] --resource-group [seu-resource-group-name] --location [sua-localizacao] --publisher-name "Seu Nome" --publisher-email "seu.email@example.com" --sku-name Consumption
    # Exemplo: Importar API
    az apim api import --path "/api" --service-name [seu-apim-name] --resource-group [seu-resource-group-name] \
        --description "API de E-commerce" --display-name "E-commerce API" \
        --service-url "https://[url-do-seu-container-app-ou-function]" --api-id "ecommerce-api"
    # Exemplo: Adicionar política (você precisará do conteúdo XML da política em um arquivo, ex: policy.xml)
    # az apim api policy create --service-name [seu-apim-name] --resource-group [seu-resource-group-name] --api-id "ecommerce-api" --xml-path "policy.xml"
    ```

8.  **Teste da Aplicação/API:**
    * Utilizado [ex: Postman] para enviar uma requisição [ex: POST] para a URL do Container App ou API Management (ex: `https://[seu-apim-name].azure-api.net/[endpoint-da-api]`).
    * [Opcional: Adicione um snippet de como você testou, ex: `curl -X POST -H "Content-Type: application/json" -d '{"name": "Produto X", "price": 123.45}' https://[sua-url]/products`]
    * [Opcional: Mencione como você obteve a Subscription Key do Developer Portal do APIM para incluir nas requisições.]

---

## ⚙️ Códigos e Arquivos Relevantes

* [`Dockerfile`](Dockerfile) - [Se aplicável] Define o ambiente e as dependências do contêiner.
* [`app.py`](app.py) - O código-fonte da API em Python [ou o nome do seu arquivo principal da aplicação].
* [`requirements.txt`](requirements.txt) - Lista de dependências Python [ou `package.json`, `.csproj`, etc.].
* [Se usou Azure Functions: `HttpTrigger1/function.json` e o arquivo de código da função, ex: `HttpTrigger1/__init__.py`]
* [Se usou API Management: `policy.xml` (exemplo de um arquivo de política XML que você aplicou, ex: para Rate Limit ou JWT Validation).]

---

## 📸 Screenshots

Aqui estão algumas capturas de tela dos recursos implantados no Azure, comprovando a execução e configuração da solução. Certifique-se de salvar suas imagens na pasta `assets/` do seu repositório e atualizar os links.

* **Visão Geral do Azure Container App [ou Azure Function App]:**
    ![Visão Geral Container App](assets/containerapp_overview.png)
    _Descrição: Tela de visão geral do Container App (ou Function App), mostrando o status de execução, a URL de acesso e o ambiente._

* **Configuração de Segredos [ou Variáveis de Ambiente no Function App]:**
    ![Configuração de Segredos](assets/containerapp_secrets.png)
    _Descrição: Demonstração da configuração segura da string de conexão do banco de dados como um segredo/variável de ambiente no Container App/Function App._

* **Visão Geral do Azure SQL Database [ou seu DB escolhido]:**
    ![Visão Geral SQL Database](assets/sqldb_overview.png)
    _Descrição: Detalhes do Azure SQL Database, incluindo o nome do servidor, o status e as informações de conexão._

* **Azure Container Registry - Repositórios:**
    ![ACR Repositórios](assets/acr_repositories.png)
    _Descrição: Exibição da imagem Docker da API (ou Function App conteinerizada) armazenada no Azure Container Registry._

* **[Se usou API Management] Visão Geral do Azure API Management:**
    ![Visão Geral APIM](assets/apim_overview.png)
    _Descrição: Tela de visão geral da instância do Azure API Management, mostrando o status e a URL do Gateway._

* **[Se usou API Management] Configuração de Políticas no APIM:**
    ![Políticas APIM](assets/apim_policies.png)
    _Descrição: Exemplo de uma política configurada no API Management, como Rate Limit ou validação de Subscription Key, aplicada à API._

* **Teste da API com [Ferramenta de Teste, ex: Postman]:**
    ![Teste API Postman](assets/api_postman_test.png)
    _Descrição: Resultado de uma requisição HTTP bem-sucedida à API (via Container App ou APIM), mostrando a resposta esperada e os cabeçalhos da requisição._

---

## 🧠 Insights e Aprendizados

Durante a execução deste desafio, pude aprofundar meu conhecimento e praticar os seguintes conceitos:

* **Poder da Plataforma como Serviço (PaaS):** Experimentei como serviços como **Azure Container Apps** e **Azure Functions** simplificam o desenvolvimento e a implantação de aplicações, abstraindo a complexidade da infraestrutura e oferecendo escalabilidade automática.
* **Gerenciamento de Imagens com ACR:** O **Azure Container Registry (ACR)** se mostrou essencial para um fluxo de trabalho eficiente com contêineres, permitindo armazenar, versionar e gerenciar imagens Docker de forma segura.
* **Segurança em Nuvem:** A prática crucial de utilizar o gerenciamento de **segredos** (Container Apps) ou **identidades gerenciadas** (Functions/Container Apps) para proteger informações sensíveis, como credenciais de banco de dados, evitando exposição no código ou configurações públicas.
* **Integração de Serviços Azure:** Compreendi como diferentes serviços Azure (ex: Contêineres/Functions, ACR, Bancos de Dados, API Management) podem ser integrados de forma fluida para construir uma solução completa e robusta.
* [Se usou Container Apps] **Conceito de Revisões e Escalabilidade:** Pratiquei o gerenciamento de revisões para implantações seguras (ex: Blue/Green deployment) e a escalabilidade dinâmica do Container Apps com base na demanda.
* [Se usou Azure Functions] **Gatilhos e Associações (Bindings):** Reforcei o entendimento de como os gatilhos iniciam a execução de funções e como as associações simplificam a interação com outros serviços de forma declarativa, reduzindo o código boilerplate.
* [Se usou API Management] **Gerenciamento de APIs Abrangente:** Aprendi sobre o papel vital do **Azure API Management** como um gateway central, a aplicação de **políticas (XML)** para controlar o comportamento da API (como segurança e limitação de taxa), e como o **Portal do Desenvolvedor** melhora a experiência dos consumidores da API.

---

## 💡 Possibilidades de Melhoria e Próximos Passos

Este projeto básico pode ser expandido de várias maneiras para simular um cenário mais completo e robusto:

* **CI/CD Automatizado:** Implementar um pipeline de CI/CD (ex: **GitHub Actions** ou Azure DevOps Pipelines) para automatizar o build da imagem Docker (ou o deploy da função), o push para o ACR e a implantação no Azure em cada alteração de código.
* **Autenticação e Autorização:** Adicionar mecanismos de segurança robustos para a API (ex: Azure Active Directory B2C, JWT).
* **Mensageria:** Integrar com serviços de mensageria como **Azure Service Bus** ou **Azure Event Hubs** para processamento assíncrono de eventos e desacoplamento de serviços (ex: processar um pedido após a compra).
* **Outros Bancos de Dados:** Explorar o uso de **Azure Cosmos DB** para um catálogo de produtos flexível ou **Azure Cache for Redis** para cache de sessão e dados de alta performance.
* **Monitoramento Avançado:** Configurar o **Application Insights** para telemetria detalhada da API, incluindo métricas, logs e rastreamento distribuído.
* **Front-end:** Conectar uma aplicação front-end (ex: React, Angular, Vue.js) hospedada em **Azure Static Web Apps** ou **Azure Storage (static website)** à API.

---
