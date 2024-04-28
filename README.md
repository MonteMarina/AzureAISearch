**AZURE COGNITIVE SEARCH: UTILIZANDO AI SEARCH PARA INDEXAÇÃO E CONSULTA DE DADOS**

Passo a Passo para Criar um Índice de Busca Azure AI (UI)

1. **Criação de Recursos no Azure:**
   - Faça login no portal do Azure.
   - Clique no botão + Criar um recurso, pesquise por Azure AI Search e crie um recurso Azure AI Search com as seguintes configurações:
     - Assinatura: Sua assinatura do Azure.
     - Grupo de recursos: Selecione ou crie um grupo de recursos com um nome único.
     - Nome do serviço: Um nome único.
     - Localização: Escolha qualquer região disponível.
     - Nível de preços: Básico.
   - Após ver a resposta de Validação de Sucesso, selecione Criar.

2. **Criação de um Recurso Azure AI Services:**
   - Retorne à página inicial do portal Azure.
   - Clique no botão + Criar um recurso e procure por Azure AI services. Selecione criar um plano de serviços Azure AI.
   - Configure com as seguintes configurações:
     - Assinatura: Sua assinatura do Azure.
     - Grupo de recursos: O mesmo grupo de recursos do seu recurso Azure AI Search.
     - Região: A mesma localização do seu recurso Azure AI Search.
     - Nome: Um nome único.
     - Nível de preços: Padrão S0.
   - Selecione Revisar + criar e, após ver a resposta de Validação Passada, selecione Criar.

3. **Criação de uma Conta de Armazenamento:**
   - Retorne à página inicial do portal Azure e selecione o botão + Criar um recurso.
   - Pesquise por conta de armazenamento e crie um recurso de conta de armazenamento com as seguintes configurações:
     - Assinatura: Sua assinatura do Azure.
     - Grupo de recursos: O mesmo grupo de recursos do seu recurso Azure AI Search e Azure AI services.
     - Nome da conta de armazenamento: Um nome único.
     - Localização: Escolha qualquer local disponível.
     - Desempenho: Padrão.
     - Redundância: Armazenamento redundante local (LRS).
   - Clique em Revisar e depois em Criar. Aguarde o término do provisionamento e vá para o recurso implantado.

4. **Carregamento de Documentos no Armazenamento do Azure:**
   - No painel de menu à esquerda, selecione Contêineres.
   - Selecione + Contêiner. Uma janela à direita será aberta.
   - Insira as seguintes configurações e clique em Criar:
     - Nome: coffee-reviews.
     - Nível de acesso público: Contêiner (acesso de leitura anônimo para contêineres e blobs).
   - Em uma nova aba do navegador, baixe as revisões de café compactadas de https://aka.ms/mslearn-coffee-reviews e extraia os arquivos para a pasta de revisões.
   - No portal Azure, selecione seu contêiner coffee-reviews. No contêiner, selecione Carregar.
   - Na janela Carregar blob, selecione Selecionar um arquivo.
   - Na janela do Explorador, selecione todos os arquivos na pasta de revisões, clique em Abrir e depois em Carregar.
   - Após o término do upload, você pode fechar a janela Carregar blob. Seus documentos estão agora no contêiner de armazenamento coffee-reviews.

5. **Indexação dos Documentos:**
   - No portal Azure, navegue até o recurso Azure AI Search. Na página de visão geral, selecione Importar dados.
   - Na página Conectar aos seus dados, na lista Fonte de dados, selecione Azure Blob Storage. Complete os detalhes da fonte de dados com os seguintes valores:
     - Fonte de dados: Azure Blob Storage.
     - Nome da fonte de dados: coffee-customer-data.
     - Dados para extrair: Conteúdo e metadados.
     - Modo de análise: Padrão.
     - String de conexão: * Selecione Escolher uma conexão existente. Selecione sua conta de armazenamento, selecione o contêiner coffee-reviews e depois clique em Selecionar.
     - Autenticação de identidade gerenciada: Nenhum.
     - Nome do contêiner: este ajuste é preenchido automaticamente após você escolher uma conexão existente.
     - Pasta de blob: Deixe em branco.
     - Descrição: Avaliações para lojas Fourth Coffee.
   - Selecione Avançar: Adicionar habilidades cognitivas (Opcional).
   - Na seção Anexar Serviços Cognitivos, selecione seu recurso Azure AI services.
   - Na seção Adicionar enriquecimentos:
     - Altere o nome do conjunto de habilidades para coffee-skillset.
     - Selecione a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content.
     - Garanta que o campo Dados de origem esteja definido como merged_content.
     - Altere o nível de granularidade do enriquecimento para Páginas (pedaços de 5000 caracteres).
     - Não selecione Habilitar enriquecimento incremental.
     - Selecione os seguintes campos enriquecidos:
        - Extração de nomes de locais: locations.
        - Extração de frases-chave: keyphrases.
        - Detecção de sentimento: sentiment.
        - Gerar tags de imagens: imageTags.
        - Gerar legendas de imagens: imageCaption.
   - Em Salvar enriquecimentos em um repositório de conhecimento, selecione:
        - Projeções de imagens Azure.
        - Documentos.
        - Páginas.
        - Frases-chave.
        - Entidades.
        - Detalhes da imagem.
        - Referências de imagem.
   - Se aparecer um aviso solicitando uma String de Conexão de Conta de Armazenamento.
   - Selecione Escolher uma conexão existente. Escolha a conta de armazenamento que você criou anteriormente.
   - Clique em + Contêiner para criar um novo contêiner chamado knowledge-store com o nível de privacidade definido como Privado e selecione Criar.
   - Selecione o contêiner knowledge-store e clique em Selecionar na parte inferior da tela.
   - Selecione Projeções de blob Azure: Documento. Uma configuração para Nome do contêiner com o contêiner knowledge-store preenchido automaticamente é exibida. Não altere o nome do

 contêiner.
   - Selecione Avançar: Personalizar índice de destino. Altere o Nome do índice para coffee-index.
   - Garanta que a Chave esteja definida como metadata_storage_path. Deixe o Nome do sugestor em branco e o Modo de pesquisa autopreenchido.
   - Revise as configurações padrão dos campos do índice. Selecione filtrável para todos os campos já selecionados por padrão.
   - Selecione Avançar: Criar um indexador.
   - Altere o Nome do indexador para coffee-indexer.
   - Deixe a Programação definida como Uma vez.
   - Expanda as opções Avançadas. Garanta que a opção Codificar chaves em Base-64 esteja selecionada, pois codificar chaves pode tornar o índice mais eficiente.
   - Selecione Enviar para criar a fonte de dados, conjunto de habilidades, índice e indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
        - Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
        - Executa o conjunto de habilidades de habilidades cognitivas para gerar mais campos enriquecidos.
        - Mapeia os campos extraídos para o índice.
   - Retorne à página do seu recurso Azure AI Search. No painel esquerdo, em Gerenciamento de pesquisa, selecione Indexadores. Selecione o indexador coffee-indexer recém-criado. Aguarde um minuto e selecione &orarr; Atualizar até que o Status indique sucesso.
   - Selecione o nome do indexador para ver mais detalhes.

6. **Consulta do Índice:**
   - Use o Search Explorer para escrever e testar consultas. O Search Explorer é uma ferramenta incorporada ao portal Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Search Explorer para escrever consultas e revisar resultados em JSON.
   - Na página de visão geral do seu serviço de pesquisa, selecione o Search explorer no topo da tela.
   -    - Observe como o índice selecionado é o coffee-index que você criou. Abaixo do índice selecionado, altere a visualização para Visualização JSON.
   - No campo do editor de consulta JSON, copie e cole:
     
    ![image](https://github.com/MonteMarina/AzureAISearch/assets/154125061/4b0b2479-ebd8-4f59-9804-6fa040b25c79)

- Selecione Pesquisar. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count. O índice de pesquisa deve retornar um documento JSON contendo seus resultados de pesquisa.
   - Agora vamos filtrar por localização. No campo do editor de consulta JSON, copie e cole:

  ![image](https://github.com/MonteMarina/AzureAISearch/assets/154125061/27136af2-e5f6-4fe5-ab88-30902ccaa8e9)

     - Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra as avaliações com uma localização de Chicago. Você deve ver 3 no campo @odata.count.
   - Agora vamos filtrar por sentimento. No campo do editor de consulta JSON, copie e cole:
 
     ![image](https://github.com/MonteMarina/AzureAISearch/assets/154125061/480f61ab-6c6e-453f-91cf-4a7f556eb071)

   - Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra as avaliações com um sentimento negativo. Você deve ver 1 no campo @odata.count.

7. **Revisão do Repositório de Conhecimento:**
   - Vamos ver o poder do repositório de conhecimento em ação. Quando você executou o Assistente de Importação de Dados, também criou um repositório de conhecimento. Dentro do repositório de conhecimento, você encontrará os dados enriquecidos extraídos pelas habilidades de IA, persistindo na forma de projeções e tabelas.
   - No portal Azure, navegue de volta para sua conta de armazenamento do Azure.
   - No painel de menu à esquerda, selecione Contêineres. Selecione o contêiner knowledge-store.
   - Selecione qualquer um dos itens e clique no arquivo objectprojection.json.
   - Selecione Editar para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.
   - Selecione o caminho do blob de armazenamento no canto superior esquerdo da tela para retornar aos Contêineres de armazenamento.
   - Nos Contêineres, selecione o contêiner coffee-skillset-image-projection. Selecione qualquer um dos itens.
   - Selecione qualquer um dos arquivos .jpg. Selecione Editar para ver a imagem armazenada do documento. Observe como todas as imagens dos documentos são armazenadas dessa maneira.
   - Selecione o caminho do blob de armazenamento no canto superior esquerdo da tela para retornar aos Contêineres de armazenamento.
   - Selecione Navegador de armazenamento no painel esquerdo e selecione Tabelas. Há uma tabela para cada entidade no índice. Selecione a tabela coffeeSkillsetKeyPhrases.
   - Olhe para as frases-chave que o repositório de conhecimento foi capaz de capturar do conteúdo nas avaliações. Muitos dos campos são chaves, então você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.






