# Detenção de Fraude em Cartões de Crédito

## 1. O Problema e o Desequilíbrio dos Dados
O objetivo deste projeto é detetar fraudes em transações reais de cartões de crédito. O principal desafio é o extremo desequilíbrio da base de dados: cerca de 99,8% das transações são normais e apenas 0,17% são fraudes. 
Neste cenário, a métrica de "Acurácia" é enganadora. Um modelo que classifique todas as compras como "normais" teria 99,8% de acurácia, mas seria inútil para o banco. Por isso, a avaliação deste projeto foca-se no **Recall** da classe minoritária, priorizando a deteção do maior número possível de fraudes, mesmo que isso custe alguns falsos positivos.

## 2. Preparação dos Dados
Para preparar os dados para o modelo de Machine Learning, realizei as seguintes etapas:
* **Ajuste de Escala:** Apliquei a transformação Logarítmica (`log1p`) na coluna de valor (`Amount`) para mitigar o impacto de transações de valor extremo.
* **Padronização:** Utilizei o `StandardScaler` nas variáveis de Tempo e Valor, colocando-as na mesma escala das restantes variáveis anonimizadas (V1 a V28).
* **Divisão Estratificada:** Separei os dados em treino (80%) e teste (20%) utilizando o parâmetro `stratify`. Isto garantiu que a proporção exata de fraudes fosse mantida em ambos os conjuntos.

## 3. Avaliação do Modelo (Métricas)
Como base (*baseline*), treinei um modelo de **Regressão Logística** utilizando o parâmetro `class_weight='balanced'` para forçar a atenção nas fraudes. Os resultados para a classe de fraude foram:
* **Recall:** 0.92 (O modelo detetou 92% das fraudes reais).
* **Precisão:** 0.06 (Dos bloqueios efetuados, 6% eram realmente fraudes).
* **F1-Score:** 0.11.
Esta configuração priorizou a segurança: garantimos a captura de quase todos os roubos, aceitando o custo operacional de rever os alertas falsos (falsos positivos).

## 4. Explicabilidade com SHAP
Para entender as decisões da inteligência artificial, utilizei a biblioteca SHAP. O gráfico de impacto revelou que as variáveis mais determinantes foram a **V4**, **V14** e **V10**. O modelo aprendeu regras claras: valores elevados na variável V4 e valores baixos na variável V14 aumentam drasticamente a probabilidade de a transação ser classificada como fraude.

## 5. Adaptações em Relação à Expert
Sendo este o meu arranque prático com o tema, optei por focar a 100% na compreensão do fluxo completo usando a Regressão Logística, em vez de implementar múltiplos algoritmos complexos simultaneamente. A prioridade foi dominar o tratamento do desbalanceamento (com `stratify` e `class_weight`) e a interpretação correta das métricas de negócio (Recall) e do SHAP, construindo uma fundação sólida para futuras evoluções com Random Forest e XGBoost.
