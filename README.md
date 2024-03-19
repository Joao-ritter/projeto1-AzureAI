

**

# Abaixo segue o passo a passo para criação de um ML automatizado, conforme documentação da Microsoft.

**


No Studio Azure Machine Learning, vá para a página de ML Automatizado (em Criação).

Crie um novo trabalho de ML automatizado com as seguintes configurações, avançando conforme necessário dentro da interface do usuário:

Configurações básicas:

    Nome do trabalho: mslearn-bike-automl
    Nome do novo experimento: mslearn-bike-rental
    Descrição: Previsão automatizada de aluguel de bicicletas usando machine learning
    Tags: Nenhum

Tipo de tarefa e dados:

    Selecione o tipo de tarefa como regressão.
    Escolha um conjunto de dados existente ou crie um novo com as seguintes configurações:
        Nome: bike-rentals
        Descrição: Dados históricos de aluguel de bicicletas
        Tipo: Tabular
        Fonte de dados: De arquivos da Web
        URL da Web: https://aka.ms/bike-rentals
        Validar dados: Não selecionado
        Configurações:
            Formato do arquivo: Delimitado
            Delimitador: Vírgula
            Codificação: UTF-8
            Cabeçalhos de coluna: Apenas o primeiro arquivo contém cabeçalhos
            Ignorar linhas: Nenhuma
            O conjunto de dados contém dados multilinhas: Não selecionado
        Esquema: Incluir todas as colunas exceto a de Caminho, e examinar automaticamente os tipos detectados.

Selecione o conjunto de dados de aluguel de bicicletas depois de criá-lo.

Configurações da tarefa:

    Tipo de tarefa: Regressão
    Conjunto de dados: bike-rentals
    Coluna alvo: aluguéis (inteiro)
    Configurações adicionais:
        Métrica primária: Erro Quadrático Médio da Raiz Normalizado
        Explicar melhor modelo: Não selecionado
        Usar todos os modelos compatíveis: Não selecionado. Restringir o trabalho para experimentar apenas alguns algoritmos específicos.
        Modelos permitidos: Selecionar apenas RandomForest e LightGBM.

Limites:

    Avaliações máximas: 3
    Avaliações simultâneas máximas: 3
    Máximo de nós: 3
    Limite de pontuação da métrica: 0,85 (O trabalho será encerrado se um modelo atingir uma pontuação de métrica de raiz do erro quadrático médio normalizado de até 0,085.)
    Tempo limite: 15
    Tempo limite de iteração: 5
    Habilitar encerramento antecipado: Selecionado

Validação e teste:

    Tipo de validação: Divisão de validação de treinamento
    Percentual de dados de validação: 10%
    Conjunto de dados de teste: Nenhum

Computação:

    Selecionar tipo de computação: Sem servidor
    Tipo de máquina virtual: CPU
    Camada da máquina virtual: Dedicada
    Tamanho da máquina virtual: Standard_DS3_V2
    Número de instâncias: 1

Envie o trabalho de treinamento. Ele será iniciado automaticamente.

Aguarde a conclusão do trabalho.




**

# Após conclusão da criação e configuração de um trabalho ML, vamos examinar o melhor modelo.

**






Quando o trabalho de machine learning automatizado estiver concluído, você poderá revisar o melhor modelo treinado.

    Na guia Visão geral do trabalho de machine learning automatizado, confira o resumo do melhor modelo.
    Selecione o texto em "Nome do algoritmo do melhor modelo" para ver os detalhes correspondentes.
    Vá para a guia Métricas e certifique-se de selecionar os gráficos de resíduos e predicted_true, se ainda não estiverem selecionados.
    Analise os gráficos que demonstram o desempenho do modelo. O gráfico de resíduos mostra as diferenças entre os valores previstos e reais como um histograma. O gráfico predicted_true compara os valores previstos com os valores verdadeiros.

Implantar e testar o modelo:

    Na guia Modelo, obtenha o melhor modelo treinado pelo trabalho de machine learning automatizado, e selecione "Implantar". Use a opção de serviço Web com as seguintes configurações:
        Nome: predict-rentals
        Descrição: Previsão de aluguéis de bicicletas
        Tipo de computação: Instância de Contêiner do Azure
        Habilitar autenticação: Selecionado
    Aguarde o início da implantação, que pode levar alguns segundos. O status de implantação para o ponto de extremidade "predict-rentals" será indicado como "Em execução".
    Aguarde até que o status de implantação mude para "Bem-sucedida". Isso pode levar de 5 a 10 minutos.

Testar o serviço implantado:

    Agora, você pode testar o serviço implantado.
    No Azure Machine Learning Studio, no menu à esquerda, vá para "Pontos de Extremidade" e abra o ponto de extremidade em tempo real "predict-rentals".
    Na página do ponto de extremidade em tempo real de previsão de aluguel, vá para a guia "Teste".
    No painel "Dados de entrada" para testar o ponto de extremidade, substitua o modelo JSON pelos seguintes dados de entrada:

json

{
   "Inputs": { 
     "data": [
       {
        "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }

    Clique no botão "Testar".
    Analise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada, similar ao seguinte:

json

{
   "Results": [
     444.27799000000000
   ]
 }

    O painel de teste utilizou os dados de entrada e o modelo treinado para retornar o número previsto de locações.

Limpar:

    O serviço Web criado está hospedado em uma Instância de Contêiner do Azure. Se não planeja utilizá-lo mais, exclua o ponto de extremidade para evitar custos desnecessários no Azure.
    No Azure Machine Learning Studio, na guia "Pontos de extremidade", selecione o ponto de extremidade "predict-rentals". Em seguida, escolha "Excluir" e confirme que deseja excluir o ponto de extremidade.
    Excluindo sua computação, você garante que não haja cobranças adicionais pela assinatura. No entanto, pode haver uma pequena cobrança pelo armazenamento de dados enquanto o workspace do Azure Machine Learning existir em sua assinatura. Se terminou de explorar o Azure Machine Learning, considere excluir o workspace do Azure Machine Learning e os recursos associados.

Para excluir seu workspace:

    No portal do Azure, na página de Grupos de recursos, abra o grupo de recursos especificado ao criar seu Workspace do Azure Machine Learning.
    Clique em "Excluir grupo de recursos", insira o nome do grupo de recursos para confirmar a exclusão e selecione "Excluir".

 TODAS ESSAS INFORMAÇÕES PODEM SER ACESSADAS NO SEGUINTE [SITE DA MICROSOFT](https://microsoftlearning.github.io/AI-900-AIFundamentals.pt-BR/Instructions/02-module-02.html), SENDO ESTE ARQUIVO APENAS UM FACILITADOR PARA QUEM ESTIVER NO REPOSITÓRIO.
