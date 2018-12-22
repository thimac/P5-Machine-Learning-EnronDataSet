# Identificando Fraudes nos Emails da Enron


## Entendendo o DATASET e fazendo a limpeza dos dados

O objetivo desse projeto é usar técnicas de aprendizagem de máquina (machine learning) para indentificar pessoas de interesse (POIs), os quais estavam envolvido em algum tipo de fraude, usando o conjunto de dados da empresa Enron.

### Conjunto de dados
O conjunto de dados possui 146 pessoas e 21 features. Essas features podem ser divididas em 3 categorias: financeira (ex. salary, total_payments, bonus), relacionadas ao email (ex. to_messages, from_messages, email_address) e POIs (que é um feature booleana).
Para entender melhor, abaixo  alguns dados relevantes:
Quantidade de Pessoas:  146
Quantidade de Features:  21
Quantidade de POIs: 18 ou aproximandamente 18%
Quatindade de não POIs: 128

### Seleção de features
A quanditade de NaN para a maioria das Features é bastante alto. Como forma de fazer uma seleção prévia das features relevantes, irei desconsiderar todas que possuem acima de 50% (deferral_payments, restricted_stock_deferred, loan_advances, director_fees, deferred_income long_term_incentive) de NaN, e o email que não me parece ser relevante para análise.

Para saber quais features são mais relevantes, aplicarei o algoritmo Feature Imporante. O mesmo mostrou que algumas fetaures são essenciais pois possuem uma importância alta, como exercised_stock_options e total_stock_value. Usarei a maioria das features com score alto de relevância para esse algoritmo e outros que considero imporante para análise, como a to_messages e a from_messages que o algoritmo deu um score baixo de relevância, mas usarei para criar outras features. Lista de features selecionadas: poi, salary, total_payments, exercised_stock_options, shared_receipt_with_poi, total_stock_value, expenses, from_messages, deferred_income, from_this_person_to_poi, from_poi_to_this_person.

### Análise e remoção de outliers
Eu realizei um plot (ver poi_id.ipynb) para ver quais features possuíam outliers, e a análise mostrou que a maioria dos outliers vinham da "pessoa" TOTAL, removi ela, porque não é relevante. Também removi THE TRAVEL AGENCY IN THE PARK e LOCKHART EUGENE E.

### Criação de features
Criei duas features que ao meu ver fazem sentido: fraction_from_poi, que a procentagem de mensagem recebidas de um POI; e fraction_to_poi que a porcentagem de mensagens enviadas para um POI. Para não ficar reduntande descartarei as features to_messages e from_messages, já que as informações relavantes estão nas features criadas. Lista final das features: **poi, salary, total_payments, exercised_stock_options, shared_receipt_with_poi, total_stock_value, expenses, deferred_income, from_this_person_to_poi, from_poi_to_this_person, fraction_from_poi e fraction_to_poi**.

## Machine Learning

### Testando alguns Classificadores
Eu testei 4 classificadores (esses resultados já são as saídas do test_classifier()):

SVM  -                Accuracy: 0.87473       Precision: 0.0          Recall: 0.0
Decision Tree -       Accuracy: 0.82487       Precision: 0.34062      Recall: 0.33500
Naive Bayes -         Accuracy: 0.85447       Precision: 0.43677      Recall: 0.31600
Linear Discriminant - Accuracy: 0.84607       Precision: 0.36291      Recall: 0.20450

Dois algoritmo apresentaram resultados bons em termos de Precision e Recall, Naive Bayes e Decision Tree. O algoritmo escolhido para as próximas etapas foi o Naive Bayes.

### Tunning meu classificador e Validação
Eu executei várias vezes meu algoritmo e usei sklearn.metrics para analisar os resultados. O algoritmo também foi testando usando o test_classifier() para validar os resultados. Como foi mostrado acima o Naive Bayes foi o melhor deles.Tunning o algoritmo é muito importante para evitar o overfit do aloritmo.

### Métricas utilizadas
As métricas utilizadas foram Precision e Recall, elas foram escolhidas porque o conjunto de dados é pequeno e por isso a acurácia não é uma boa medida. Precision é tp/(tp+fp), onde tp é true positives e fp é false positives. Já recall é tp/(tp+fn), onde fn é false negatives.



