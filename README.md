# deteccao-fraude-cartao-credito


🛡️ Detecção de Fraude em Transações de Cartão de Crédito com Machine Learning

Projeto prático desenvolvido para o desafio de projeto da DIO
 no bootcamp Bradesco - Agentes de IA: Do Zero à Prática, sob orientação da Expert Dra. Isadora Garcia Ferrão.

📌 1. O Problema e o Desafio do Desbalanceamento

Em cenários reais de pagamentos com cartão, transações fraudulentas são eventos extremamente raros:

Total de transações normais: 99,83%
Total de transações fraudulentas: apenas 0,17% (492 casos em mais de 284 mil transações).
Por que a Acurácia Engana?

Se criarmos um modelo ingênuo que simplesmente classifique 100% das transações como normais, ele atingirá 99,83% de acurácia, mas deixará passar 100% das fraudes.

Por isso, o foco deste projeto é avaliar métricas específicas para a classe 1 (fraude):

Recall (Sensibilidade): capacidade de capturar o maior número possível de fraudes reais (minimizar Falsos Negativos).
Precisão: garantia de que os alarmes acionados são de fato fraudes (minimizar bloqueios indevidos de clientes legítimos).
F1-Score: média harmônica entre precisão e recall.
⚙️ 2. Preparação dos Dados (Pipeline)
Importação via Link: Dataset carregado dinamicamente via link oficial sem poluir o repositório.
Feature Engineering: Aplicação de transformação logarítmica (log_amount = np.log1p(Amount)) para reduzir o impacto de valores discrepantes.
Divisão Estratificada: Separação entre treino (80%) e teste (20%) utilizando stratify=y, garantindo exatamente a proporção de 0,17% de fraudes em ambos os conjuntos (98 casos de fraude no conjunto de teste).
Remoção de Colunas: Descarte de Time e Amount original para evitar ruídos e multicolinearidade com o log_amount.
📊 3. Comparação dos Modelos (Classe de Fraude - Classe 1)

Foram treinados e comparados três algoritmos com tratamento de desbalanceamento de classes:

Modelo	Estratégia de Balanceamento	Recall	Precisão	F1-Score	Destaque
Regressão Logística (Baseline)	class_weight='balanced'	91,84%	5,79%	10,89%	Capturou 90 das 98 fraudes, porém gerou alto volume de falsos positivos.
Random Forest	class_weight='balanced'	80,61%	80,61%	80,61%	Melhor equilíbrio geral, eliminando quase todos os alarmes falsos.
XGBoost	scale_pos_weight	84,69%	49,11%	62,17%	Alto poder de detecção intermediário entre os modelos.
🎯 4. Ajuste do Limiar de Decisão (Threshold Tuning)

Por padrão, classificadores operam com corte de probabilidade em 0.5 (50%). Na Random Forest, avaliamos o impacto de ajustar esse limiar:

Limiar 0.3 (Mais agressivo na captura):
Recall: 86,73% (captura 85 de 98 fraudes)
Precisão: 54,49%
Limiar 0.5 (Padrão balanceado):
Recall: 80,61%
Precisão: 80,61%
Limiar 0.7 (Mais conservador com clientes legítimos):
Recall: 75,51%
Precisão: 85,06%

Decisão de Negócio: Para ambientes bancários com equipe de análise dedicada, o limiar 0.3 é recomendado para maximizar a captura de fraudes, enquanto em fluxos automatizados com bloqueio direto de cartão, o limiar 0.5 garante excelente proteção sem atrito com clientes.

🔍 5. Explicabilidade do Modelo com SHAP

Utilizando o shap.TreeExplainer sobre a Random Forest:

As componentes principais V2 e V1 despontaram no topo do ranking de importância (Beeswarm summary plot), exercendo o maior peso no direcionamento das predições para fraude.
O uso de SHAP confere transparência e auditoria ao modelo, atendendo a critérios de governança e explicabilidade de IA em instituições financeiras.
💡 O que foi implementado além do básico
Comparação lado a lado das Curvas ROC e Precision-Recall com cálculo de AUC.
Análise empírica do limiar de decisão (Threshold Tuning).
Análise de explicabilidade com SHAP em amostra estratificada de teste.
📁 Estrutura do Repositório
text
├── credit_card_fraud_detection.ipynb   # Notebook completo com todas as saídas e gráficos salvos
└── README.md                           # Documentação e justificativas de negócio
🚀 Como Executar
Abra o arquivo .ipynb no Google Colab
.
Execute todas as células em sequência (Ambiente de execução > Executar tudo). O dataset será baixado automaticamente via URL.

Desenvolvido para fins de estudo e portfólio na DIO 🚀. By Carolina Fernanda 
