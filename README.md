[![author](https://img.shields.io/badge/author-MauricioEloy-red.svg)](https://www.linkedin.com/in/mauricio-eloy) [![](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/release/python-365/) [![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)](http://perso.crans.org/besson/LICENSE.html) [![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/MauricioEloy/Portifolio/issues)

<p align="center">
<img src="LOGO.png" >
</p>

# <span style="color:blue"> Previsão do preço da estadia - Airbnb RJ </span>

**Mauricio Eloy**<br>

---
###### Contatos:
*email:* profmauricioeloy@gmail.com | mauricioeloy@usp.br

*LindedIn*: https://www.linkedin.com/in/mauricio-eloy/

---
## <span style="color:blue">Descrição do Problema</span>

O problema principal a ser resolvido é a previsão de preços de estadia (feature ‘price’) do Airbnb no Rio de Janeiro.

Para solucionar este problema principal aplicou-se o seguinte roteiro:

### <span style="color:blue">Análise Exploratória e Pré- Processamento dos Dados</span>

#### <span style="color:blue">Fonte</span>

Os dados utilizados são do Airbnb Rio de Janeiro, conforme fonte:
http://insideairbnb.com/get-the-data.html.

* listings.csv.gz: Dados detalhados de listagens para o Rio de Janeiro.

* calendar.csv.gz: Dados detalhados do calendário para listagens no Rio de Janeiro.

#### <span style="color:blue">Análise Inicial</span>
Em um primeiro momento foi feita uma análise exploratória para avaliar a consistência dos dados e identificar as possíveis variáveis que impactam a variável resposta.

Neste momento a estratégia utilizada foi importar os dados e analidá-los separadamente.

##### <span style="color:blue">dataset listings</span>
O dataset `listings` foi explorado por meio da análise exploratória automática utilizando-se a biblioteca `Pandas Profiling`.

Por meio do relatório gerado percebeu-se que é necessário fazer um o Pré-processamento dos dados.

Assim faz-se necessário verificar quais atributos vamos ou não manter.

Optou-se por excluir as variáveis:

'listing_url', 'scrape_id','last_scraped', 'name', 'description', 'neighborhood_overview', 'picture_url', 'host_id', 'host_url', 'host_name','host_location', 'host_about', 'host_response_time', 'host_acceptance_rate','host_thumbnail_url', 'host_picture_url', 'host_total_listings_count','neighbourhood','neighbourhood_group_cleansed', 'latitude', 'longitude', 'bathrooms', 'minimum_minimum_nights', 'maximum_minimum_nights','minimum_maximum_nights','maximum_maximum_nights', 'minimum_nights_avg_ntm', 'maximum_nights_avg_ntm', 'calendar_updated', 'has_availability', 'availability_30', 'availability_60', 'availability_90', 'availability_365', 'calendar_last_scraped', 'number_of_reviews_ltm', 'number_of_reviews_l30d', 
'first_review', 'last_review', 'license', 'calculated_host_listings_count_entire_homes', 'calculated_host_listings_count_private_rooms', 'calculated_host_listings_count_shared_rooms','reviews_per_month', 'host_neighbourhood' e 'bathrooms_text'.

Embora algumas informações pudessem ser relevantes no modelo, como por exemplo 'bathrooms', em nosso dataset estes dados estão vazios.

Após estar de posse com as possíveis variáveis a serem utilizadas no modelo, deve-se fazer algumas modificações e verificar dados duplicados e faltantes.

* 'host_since' apresenta dois problemas: i) está como variável categórica e ii) tem dados faltantes (0.1%). A solução para o primeiro problema foi desmembrar a variável 'host_since' e criar a variável 'host_since_year', já para o segundo problema, por ter uma porcentagem baixa, foi excluída as linhas com esses dados;

* 'host_response_rate' apresenta dois problemas: i) está como variável categórica e ii) tem dados faltantes (30.1%). A solução para o primeiro problema foi desmembrar a variável 'host_response_rate' e criar a variável numérica 'host_response_rate_numeric', já para o segundo problema, optou-se pelo preencimento utilizando-se da média;

* 'property_type' apresenta o problema de ter uma alta cardinalidade. A solução foi utilizar as informações da variável 'property_type' e criar a variável 'property_type_mod' com as cinco categorias mais frequentes, representando 86,7% dos dados e criar a sexta categoria, 'Other values';

* 'neighbourhood_cleansed' apresenta o problema de ter uma alta cardinalidade. A solução foi utilizar as informações da variável 'neighbourhood_cleansed' e criar a variável 'neighbourhood_cleansed_mod' com as seis categorias mais frequentes, e criar a sexta categoria, 'Other values';

* 'host_verifications' e 'amenities' são variáveis que recebem uma lista como valores, deste modo, fez-se o desmenbramento destas listas criando novas variáveis binárias (dummie).

* Dados faltantes: as variáveis 'bedrooms' e 'beds' foram completados com suas respectivas medianas já as variáveis 'host_response_rate_numeric', 'review_scores_rating', 'review_scores_accuracy', 'review_scores_cleanliness', 'review_scores_checkin', 'review_scores_communication', 'review_scores_location' e 'review_scores_value' foram completados com suas respectivas médias

* 'host_is_superhost', 'host_has_profile_pic', 'host_identity_verified' e 'instant_bookable' são variáveis que tem como valores 't' e 'f', fez-se a troca por valores binários 1 e 0;

Por fim efetuamos a eliminação de Outliers, chegando no dataset 'listings' limpo.

##### <span style="color:blue">dataset calendar</span> 
Verificou-se que no dataset `calendar` tinhamos dados faltantes nas variáveis 'price','adjusted_price', 'minimum_nights' e 'maximum_nights'.

Desde modo percebeu-se que é necessário fazer um o Pré-processamento dos dados.

Assim faz-se necessário verificar quais atributos vamos ou não manter.

Optou-se por excluir as variáveis: 'available', 'adjusted_price', 'minimum_nights' e 'maximum_nights'.

Após estar de posse com as possíveis variáveis a serem utilizadas no modelo, deve-se fazer algumas modificações.

* 'price' apresenta dois problemas: i) está como variável categórica e ii) tem dados faltantes. A solução para o primeiro problema foi desmembrar a variável 'price' e criar a variável 'price_ca', já para o segundo problema, por ser a variável a ser predita, foi excluída as linhas com esses dados;

* 'date' é uma variável categórica, e neste caso vamos criar por meio dela quatro novas variáveis: i) 'holiday'; ii) 'weekday'; iii) 'month' e iv) 'year'. Como o objetivo não era fazer a análise das séries temporais dos preços de cada estadia, fez-se necessário criar as variáveis acima com o intuito de fazer com que o modelo capture se isso pode ou não influenciar no preço. 

* Após a retirada da variável 'date', inevitavelmente, ficamos com valores duplicados, que foram excluídos.

##### <span style="color:blue">dataset final</span>
Com o intuito de utilizar das informações relacionadas com data no modelo, foi realizada a junção dos datasets `listings` e `calendar` por meio do identificar de estadia.

Após a junção criou-se as variáveis dummie utilizando-se dos atributos 'property_type_mod' e 'room_type', chegando ao dataset final a ser utilizado.

### <span style="color:blue">Análise de atributos importantes</span>

Neste momento foi realizada a análise de importância dos atributos, existem diversas formas para efetuar esta análise, a metodologia utilizada foi através do PCA e LASSO.

##### <span style="color:blue">PCA</span>

Através da análise de componentes pricipais foi possível verificar que faz-se necessário o uso de 36 componentes para que tenhamos uma variabilidade sigificativamente compatível com os dados originais.

Utilizando-se das 3 primeiras componentes e de um valor absoluto mínimo de 0.24, o ideal seria superior a 0.5, mas neste caso não teríamos nenhuma, chegou-se a conclusão que as seguintes variáveis importantes: 'review_scores_location','property_type_mod_Entire apartment','room_type_Entire home/apt','review_scores_checkin','bedrooms','room_type_Private room','host_identity_verified','review_scores_accuracy','accommodates','review_scores_cleanliness','review_scores_rating','host_verifications_ government_id','review_scores_communication','host_verifications_ offline_government_id','host_verifications_ jumio' e 'review_scores_value'

Também foram incorporadas as 41 componentes principais no dataset, esta imcorporação será utilizada na estratégia de comparação dos modelos.

##### <span style="color:blue">LASSO</span>

Através da regressão LASSO, foi possível obter coeficientes diferentes de 0, trazendo assim a importância dos respectivos atributos.

Neste caso chegou-se a conclusão que apenas duas variáveis não tinha o coeficiente diferente de zero.

##### <span style="color:blue">PCA & LASSO</span>

A estratégia utilizada foi efetuar a intersecção de inportância apresentada nos dois métodos para se ter um conjunto final de atributos importantes.

Tendo então o seguinte resultado de atributos importantes: 'review_scores_accuracy','host_identity_verified','review_scores_location','accommodates','review_scores_cleanliness','room_type_Entire home/apt','review_scores_rating','review_scores_checkin','host_verifications_ government_id','review_scores_communication','host_verifications_ offline_government_id','bedrooms','host_verifications_ jumio' e 'review_scores_value'

#### <span style="color:blue">Criação do Modelo</span>

A estratégia utilizada foi a comparação do desempenho dos modelos em 5 cenários diferentes, dos quais 4 temos a aplicação dos filtros relativos aos atributos importantes e o último utilizando-se do dataset sem os filtros.

Os modelos utilizados para a comparação foram uma regressão linear: Linear Regression e três modelos baseados em árvores: Decision Tree Regressor, Random Forest Regressor e XGBoost Regressor.

Para a comparação utilizou-se de 4 princípios:

* R2 Score: quanto maior melhor;
* Mean Absolute Error: quanto menor melhor;
* Teste de Breusch-Pagan: se p-value<0.05, a hipótese nula, que consiste na homocedasticidade dos resíduos, é rejeitada.
* Teste de Kolmogorov-Smirvoc: se p-value<0.05 os valores não apresentam uma distribuição normal.

#### <span style="color:blue">Conclusão</span>

O modelo com melhor comportamento foi o Random Forest Regressor, tanto aplicado com as variáveis do LASSO quanto as variáveis originais, optou-se pelos atributos LASSO.

As seguintes métricas de avaliação foram obtidas:
* R2: 0.832
* MAE: 235.72

Ainda não pode-se dizer que é o modelo desejável, uma vez que, o MAE está elevado, e os resíduos ndo modelo não passaram nos testes de Breusch-Pagan e Kolmogorov-Smirvoc.

#### <span style="color:blue">Próximos Passos</span>

De modo a obter melhores resultados, os próximos passos seriam:

1. Realizar uma feature engineering mais detalhada;
2. Após a obtenção do modelo com melhor comportamento, efetuar hyperparameter optimization;
3. Avaliar se estas alterações foram relevantes.