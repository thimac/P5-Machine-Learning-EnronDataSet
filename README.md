# Identificando Fraudes nos Emails da Enron


## Entendendo o DATASET e fazendo a limpeza dos dados

O objetivo desse projeto é usar técnicas de aprendizagem de máquina (machine learning) para indentificar pessoas de interesse (POIs), os quais estavam envolvido em algum tipo de fraude, usando o conjunto de dados da empresa Enron.

### Conjunto de dados
O conjunto de dados possui 146 pessoas e 21 features. Essas features podem ser divididas em 3 categorias: financeira (ex. salary, total_payments, bonus), relacionadas ao email (ex. to_messages, from_messages, email_address) e POIs (que é um feature booleana).
Para entender melhor, abaixo  alguns dados relevantes:<br/>
Quantidade de Pessoas:  146<br/>
Quantidade de Features:  21<br/>
Quantidade de POIs: 18 ou aproximandamente 18%<br/>
Quatindade de não POIs: 128

### Seleção de features
A quanditade de NaN para a maioria das Features é bastante alto. Como forma de fazer uma seleção prévia das features relevantes, iremos desconsiderar todas que possuem acima de 75% de NaN (director_fees, loan_advances, restricted_stock_deferred), e o email que não me parece ser relevante para análise.

Para saber quais features são mais relevantes, apliquei o método Recursive Feature Elimination (RFE). selecionou recursivamente as 8 melhores features, todas as features relacionadas ao finaceiro, como exercised_stock_options e total_stock_value. Usarei algumas features selecionadas pelo algoritmo e depois criarei mais usando algumas feature não selecionadas, a to_messages e a from_messages. Lista de features selecionadas: poi, salary, total_payments, exercised_stock_options, shared_receipt_with_poi, total_stock_value, expenses, from_messages, deferred_income, from_this_person_to_poi, from_poi_to_this_person.

### Análise e remoção de outliers
Eu realizei um plot (ver poi_id.ipynb) para ver quais features possuíam outliers, e a análise mostrou que a maioria dos outliers vinham da "pessoa" TOTAL, removi ela, porque não é relevante. Também removi THE TRAVEL AGENCY IN THE PARK e LOCKHART EUGENE E.

### Criação de features
Criei duas features que ao meu ver fazem sentido: fraction_from_poi, que a procentagem de mensagem recebidas de um POI; e fraction_to_poi que a porcentagem de mensagens enviadas para um POI. Para não ficar reduntande descartarei as features to_messages e from_messages, já que as informações relavantes estão nas features criadas. Lista final das features: **poi, salary, total_payments, exercised_stock_options, shared_receipt_with_poi, total_stock_value, expenses, deferred_income, from_this_person_to_poi, from_poi_to_this_person, fraction_from_poi e fraction_to_poi**.

## Machine Learning

### Testando alguns Classificadores
Eu testei 4 classificadores:<br/>
-------- Logistic Regression ----------<br/>
Recall score:  0.11500000000000002<br/>
Precision score:  0.05024132730015083<br/>
Accuracy score:  0.6333333333333333<br/>

-------- Decision Tree ----------------<br/>
Recall score:  0.28869047619047616<br/>
Precision score:  0.43999999999999995<br/>
Accuracy score:  0.8583333333333332<br/>

-------- Naive Bayes ------------------<br/>
Recall score:  0.2710714285714286<br/>
Precision score:  0.44000000000000006<br/>
Accuracy score:  0.8520833333333334<br/>

-------- Linear Discriminant ----------<br/>
Recall score:  0.1713492063492063<br/>
Precision score:  0.4600000000000001<br/>
Accuracy score:  0.8541666666666666<br/>

Dois algoritmo apresentaram resultados bons em termos de Precision e Recall, Naive Bayes e Decision Tree. O algoritmo escolhido para as próximas etapas foi o Naive Bayes.

Foram feitos testes antes e depois de adicionar as features também, e os testes mostraram que a adição das features deram uma ligeira melhora no recall:<br/>
-------- NB with new features ----------<br/>
Recall score:  0.3271825396825397<br/>
Precision score:  0.3938095238095238<br/>
Accuracy score:  0.8541666666666667<br/>

-------- NB without new features ----------<br/>
Recall score:  0.2710714285714286<br/>
Precision score:  0.44000000000000006<br/>
Accuracy score:  0.8520833333333334<br/>


### Afinamento do classificador e Validação
Eu fiz o afinamento do meu algoritmo utilizando o GridSearchCV() juntamente com a função test_class() que criei para realizar os testes no modelo. O algoritmo também foi testando usando o test_classifier() para validar os resultados. Como foi mostrado acima o Naive Bayes foi o melhor deles. Depois do afinamento esses foram os resultados:<br/>
-------- Tunning Naive Bayes ----------<br/>
Recall score:  0.32884311091207646<br/>
Precision score:  0.4369540229885057<br/>
Accuracy score:  0.8591954022988506<br/>

O afinamento do algoritmo é necessário porque ele testa diversos parâmetros e encontra o melhor que se encaixa no modelo.

### Métricas utilizadas
As métricas utilizadas foram Precision e Recall, elas foram escolhidas porque o conjunto de dados é pequeno e por isso a acurácia não é uma boa medida. Precision é tp/(tp+fp), onde tp é true positives e fp é false positives. Já recall é tp/(tp+fn), onde fn é false negatives.



