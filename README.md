# Identificando Fraudes nos Emails da Enron


##Entendendo o DATASET e fazendo a limpeza dos dados

O objetivo desse projeto é usar técnicas de aprendizagem de máquina (machine learning) para indentificar pessoas de interesse (POIs), os quais estavam envolvido em algum tipo de fraude, usando o conjunto de dados da empresa Enron.

###Conjunto de dados
O conjunto de dados possui 146 pessoas e 21 features. Essas features podem ser divididas em 3 categorias: financeira (ex. salary, total_payments, bonus), relacionadas ao email (ex. to_messages, from_messages, email_address) e POIs (que é um feature booleana).
Para entender melhor, abaixo  alguns dados relevantes:
Quantidade de Pessoas:  146
Quantidade de Features:  21
Quantidade de POIs: 18 ou aproximandamente 18%
Quatindade de não POIs: 128

###Seleção de features
A quanditade de NaN para a maioria das Features é bastante alto. Como forma de fazer uma seleção prévia das features relevantes, irei desconsiderar todas que possuem acima de 50% (deferral_payments, restricted_stock_deferred, loan_advances, director_fees, deferred_income long_term_incentive) de NaN, e o email que não me parece ser relevante para análise.

Para saber quais features são mais relevantes, aplicarei o algoritmo Feature Imporante. O mesmo mostrou que algumas fetaures são essenciais pois possuem uma importância alta, como exercised_stock_options e total_stock_value. Usarei a maioria das features com score alto de relevância para esse algoritmo e outros que considero imporante para análise, como a to_messages e a from_messages que o algoritmo deu um score baixo de relevância, mas usarei para criar outras features. Lista de features selecionadas: poi, salary, total_payments, exercised_stock_options, shared_receipt_with_poi, total_stock_value, expenses, from_messages, deferred_income, from_this_person_to_poi, from_poi_to_this_person.

###Análise e remoção de outliers
Eu realizei um plot (ver poi_id.ipynb) para ver quais features possuíam outliers, e a análise mostrou que a maioria dos outliers vinham da "pessoa" TOTAL, removi ela, porque não é relevante. Também removi THE TRAVEL AGENCY IN THE PARK e LOCKHART EUGENE E.

###Criação de features
Criei duas features que ao meu ver fazem sentido: fraction_from_poi, que a procentagem de mensagem recebidas de um POI; e fraction_to_poi que a porcentagem de mensagens enviadas para um POI. Para não ficar reduntande descartarei as features to_messages e from_messages, já que as informações relavantes estão nas features criadas.
