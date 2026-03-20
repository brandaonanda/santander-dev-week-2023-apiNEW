# DESAFIO DIO: Explorando IA Generativa em um Pipeline de ETL com Python

[Desafio proposto pela DIO/Santander](https://github.com/brandaonanda/santander-dev-week-2023-api.git)

## Resumo do desafio: Santander Dev Week 2023 Java API (ETL com Python) - CONTEXTO E PROBLEMA DADOS

  **Contexto**: Você é um cientista de dados no Santander e recebeu a tarefa de envolver seus clientes de maneira mais personalizada. Seu objetivo é usar o poder da IA Generativa para criar mensagens de marketing personalizadas que serão entregues a cada cliente.
  
  **Condições do Problema**: Você recebeu uma planilha simples, em formato CSV ('SDW2023.csv'), com uma lista de IDs de usuário do banco:

  UserID
  
  1 
  
  2
  
  3
  
  4
  
  5


  Seu trabalho é consumir o endpoint GET https://sdw-2023-prd.up.railway.app/users/{id} (API da Santander Dev Week 2023) para obter os dados de cada cliente.
  
  Depois de obter os dados dos clientes, você vai usar a API do ChatGPT (OpenAI) para gerar uma mensagem de marketing personalizada para cada cliente. Essa mensagem deve enfatizar a importância dos investimentos.
  
  Uma vez que a mensagem para cada cliente esteja pronta, você vai enviar essas informações de volta para a API, atualizando a lista de "news" de cada usuário usando o endpoint PUT https://sdw-2023-prd.up.railway.app/users/{id}.


  *RESTful API da Santander Dev Week 2023 construída em Java 17 com Spring Boot 3*

  **Principais Tecnologias**
  
  Java 17: Utilizaremos a versão LTS mais recente do Java para tirar vantagem das últimas inovações que essa linguagem robusta e amplamente utilizada oferece;
  
  Spring Boot 3: Trabalharemos com a mais nova versão do Spring Boot, que maximiza a produtividade do desenvolvedor por meio de sua poderosa premissa de autoconfiguração;
  
  Spring Data JPA: Exploraremos como essa ferramenta pode simplificar nossa camada de acesso aos dados, facilitando a integração com bancos de dados SQL;
  
  OpenAPI (Swagger): Vamos criar uma documentação de API eficaz e fácil de entender usando a OpenAPI (Swagger), perfeitamente alinhada com a alta produtividade que o Spring Boot oferece;
  
  Railway: facilita o deploy e monitoramento de nossas soluções na nuvem, além de oferecer diversos bancos de dados como serviço e pipelines de CI/CD.

  **Documentação da API (Swagger)**
  
  https://sdw-2023-prd.up.railway.app/swagger-ui.html
  
  Esta API ficará disponível no Railway por um período de tempo limitado, mas este é um código-fonte aberto. Portanto, sintam-se à vontade para cloná-lo, modificá-lo (já que é um bom projeto base para novos projetos) e executar localmente ou onde achar mais interessante! Só não esquece de marcar a gente quando divulgar a sua solução 🥰


# Adaptação do desafio

## A API original usada no projeto já estava fora do ar quando comecei esse desafio

A API utilizada neste projeto (sdw-2023-prd.up.railway.app) era um ambiente público de demonstração e **foi descontinuada**.

Com isso gerei uma nova base de dados fictícios no próprio chatGPT (OpenAI), onde solicitei uma base de dados csv de uma tabela com, inicialmente, 100 linhas (mas reduzi para 20, porque eu ia gastar meus créditos na API da openai) onde tenham as colunas `nome`, `conta`(exemplo: 00001-1), `agencia`(exemplo: 0001), `saldo`, `limite_conta`, `numero_cartao` (16 dígitos), `limite_cartao` Além disso, solicitei que criasse valores fictícios e acrescentei a seguinte restrição a ser considerada: são constraints, as colunas: `nome`, `conta` e `numero_cartao`

Planilha nomeada `dadosSDW2023NEW.csv`(local) com `url raw no github`: https://raw.githubusercontent.com/brandaonanda/santander-dev-week-2023-apiNEW/refs/heads/main/dadosSDW2023NEW.csv



## Ajustando a etapa de EXTRACT

🟢 Opção 1: Mais Simples (Dados Direto no Código) - **NÃO FOI A QUE EU FIZ**

Ideal se quiser focar exclusivamente na lógica e no uso de IA, sem depender de arquivos externos.

Simula a extração de dados (substituindo o GET da API)
users = [
    {'id': 1, 'name': 'Naruto', 'news': []},
    {'id': 2, 'name': 'Hinata', 'news': []}
]
🟡 **Opção 2: Mais Completa (Leitura de Arquivo) - OPÇÃO ESCOLHIDA**

O ChatGPT gerou o arquivo `dadosSDW2023NEW.csv`, contendo as colunas `nome` , `conta`, `agencia`, `saldo`, `limite_conta`, `numero_cartao`, `limite_cartao`, para que eu pudesse usar como fonte de dados.


**Funcionalidades na etapa EXTRACT**

  1) Lê o CSV e converte para uma lista de dicionários
  2) Garante a estrutura esperada para a etapa de Transformação
  3) Obtenção de dados direto do arquivo `dadosSDW2023NEW.csv`(local) com `url raw no github`: https://raw.githubusercontent.com/brandaonanda/santander-dev-week-2023-apiNEW/refs/heads/main/dadosSDW2023NEW.csv

## Transform


Solicitado: Utilize a API do OpenAI GPT-4 para gerar uma mensagem de marketing personalizada para cada usuário.

No caso, eu já tenho uma API da OpenAI paga por trabalhar com criação de agente de IA, modelo openai-4o-mini, e é essa que usei.

E guardei a API_KEY no .env por razões de segurança.


**Lançamento de mensagens geradas por IA para cada usuário, personalizadas, incentivando fazer investimentos**

Passando um prompt bem simples: "Você é a Fernanda, especialista em marketing bancário. Tom amigável e direto."

E a instrução: Crie uma mensagem para `usuário` sobre a importância dos investimentos (máximo de 80 caracteres)"


Foram geradas mensagens como:

>Paulo, investir é garantir seu futuro e conquistar sonhos! Vamos juntos?
>
>Oi, Carlos! Investir é essencial para garantir um futuro financeiro seguro. Vamos juntos?
>
>Oi Lucas! Investir é garantir seu futuro. Dê o primeiro passo hoje! ✨
>
>Oi, Mariana! Investir é seu caminho para realizar sonhos. Vamos juntos?
>
>Oi Daniela! Investir é o caminho para realizar seus sonhos. Vamos juntos? 🌟💰
>
>Olá, Sérgio! Investir é o caminho para garantir seu futuro. Vamos juntos?
>
>Invista hoje para realizar seus sonhos amanhã. Comece a prosperar!



## Etapa LOAD

Como a API está indisponível, a chamada PUT não funcionou. Para finalizar o Lab, salvei o resultado localmente (em um arquivo CSV) e no github.

Cada usuário que recebia mensagem personalizada, era inserido no novo arquivo de dados csv `dadosSDW2023NEW_atualizado.csv` de `url` https://raw.githubusercontent.com/brandaonanda/santander-dev-week-2023-apiNEW/refs/heads/main/dadosSDW2023NEW_atualizado.csv

E somente para fins de confirmação, antes de fazer a inserção no novo arquivo de dados atualizados, é verificado se o usuário recebeu uma mensagem personalizada, se sim, é inserido e o sistema retorna uma mensagem como:

>User Tatiane Oliveira Silva processed? True!

>CSV atualizado salvo com sucesso!


Este foi meu projeto de ETL com uso de IA Generativa para envio de mensagens!
