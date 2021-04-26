# BigQuery
BigQuery: O banco da dados da Google para Big Data

1) O Google BigQuery é um armazenamento de dados corporativo que faz consultas SQL super-rápidas com a capacidade de processamento da infraestrutura do Google.
2) Basta passar seus dados para o BigQuery que ele fará o trabalho difícil.
3) Controle o acesso ao projeto e aos dados de acordo com a necessidade dos negócios, como a concessão de acesso a outros usuários para visualizar ou consultar dados.
4) O BigQuery é totalmente gerenciado. Para dar os primeiros passos, não é preciso implantar nenhum recurso, como discos e máquinas virtuais.
5) No navegador crie uma nova conta no Gmail.
6) No navegador abra a página do Google Cloud Console. Ative o teste gratuito
7) Crie um novo projeto CURSO BIG QUERY com o ID que seja fácil de lembrar.

# Na área de consultas digite os seguintes comandos:
SELECT gender, tripduration
FROM `bigquery-public-data`.new_york_citibike.citibike_trips
LIMIT 5;####


# Para usar um "apelido" (Alias) em uma coluna da tabela devemos usar o seguinte código:
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
LIMIT 5;####

# O SQL do BigQuery também suporta filtros. No comando vemos as viagens com tempo menor que dez minutos:
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE citibike_trips.tripduration < 600
LIMIT 5;####

# Podemos usar o comando AND e OR como no código:
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE (citibike_trips.tripduration < 600 
    AND citibike_trips.gender = 'female') 
    OR citibike_trips.gender = 'male'
LIMIT 5;####

# Podemos usar os comando NOT
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE (NOT (citibike_trips.tripduration < 600 
    AND citibike_trips.gender = 'female') )
    OR (citibike_trips.gender = 'male')
LIMIT 5;####
9) Aparece um erro ao usar o código :
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE duration_minutes < 10
LIMIT 5;####

Pois estamos usando um Alias em um filtro.

# Para corrigir use o código:
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE (citibike_trips.tripduration/60) < 10
LIMIT 5;####

# Crie a consulta dentro de uma subquery usando o código:
SELECT * FROM (SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips)
WHERE duration_minutes < 10
LIMIT 5;####

Com a subquery podemos usar o Alias.

# Podemos usar o comando WITH e Alias como está escrito no código:
WITH all_trips AS (SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips)
SELECT * FROM all_trips WHERE duration_minutes < 10
LIMIT 5;####

# Temos o comando ORDER que ordena a visualização da consulta. O código seguinte usa o ORDER em ordem decrescente:
SELECT citibike_trips.gender, citibike_trips.tripduration/60 AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE citibike_trips.gender = 'female'
ORDER BY duration_minutes DESC
LIMIT 5;####


## No navegador ente na console do BIG QUERY. Iremos agregar registros com o comando GROUP BY. Também usaremos as cláusulas AVG (média) e ORDER.

# Na área de consultas digite os seguintes comandos:
SELECT citibike_trips.gender, AVG(citibike_trips.tripduration/60) AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
GROUP BY citibike_trips.gender
ORDER BY duration_minutes DESC;####

# Podemos usar as agregações SUM, MAX e MIN com o seguinte código:
SELECT citibike_trips.gender, MAX(citibike_trips.tripduration/60) AS duration_minutes_MAX
, MIN(citibike_trips.tripduration/60) AS duration_minutes_MIN, SUM(citibike_trips.tripduration/60) AS duration_minutes_SUM
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
GROUP BY citibike_trips.gender;####

# Usando o comando COUNT copie ocódigo:
SELECT citibike_trips.gender, Count(*) AS Rides, AVG(citibike_trips.tripduration/60) AS duration_minutes
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
GROUP BY citibike_trips.gender
ORDER BY duration_minutes DESC;####

# Podemos usar o COUNT na seguinte sintaxe:
select count(*) from `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips;####

# A cláusula HAVING em conjunto com GROUP BY é muito útil para eliminar duplicatas condicionalmente. O código seguinte mostra o uso:
SELECT citibike_trips.gender, AVG(citibike_trips.tripduration/60) AS duration_minutes_AVG
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
GROUP BY citibike_trips.gender
HAVING AVG(citibike_trips.tripduration/60) <= 18;####

# Podemos usar também o ALIAS:
SELECT citibike_trips.gender, AVG(citibike_trips.tripduration/60) AS duration_minutes_AVG
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
GROUP BY citibike_trips.gender
HAVING duration_minutes_AVG <= 18;####

# A instrução SELECT DISTINCT é usada para retornar apenas valores distintos (diferentes) e serve para juntar linhas repetidas. A consulta retorna os gêneros existentes na tabela:
SELECT DISTINCT citibike_trips.gender FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips;####

Que retorna um campo nulo.

# Com o código
SELECT citibike_trips.bikeid, citibike_trips.tripduration, citibike_trips.gender FROM 
`bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE gender = ""
LIMIT 100;####

Verificamos que os campos são NULOS.

# Temos o código que não retorna as linhas com campos nulos:
SELECT DISTINCT citibike_trips.gender FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE citibike_trips.bikeid IS NOT NULL;####

# Podemos adicionar o campo "usertype" para mostrar o tipo de usuário:
SELECT DISTINCT citibike_trips.gender, citibike_trips.usertype FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE citibike_trips.bikeid IS NOT NULL;####

# Usando a função EXTRACT* retorna o ano do registro e o COUNT para contar o numero de registros do ano. O código fica assim:
SELECT 
gender, EXTRACT(YEAR FROM starttime) AS year, COUNT(1) AS numtrips
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE gender != 'unknown' AND starttime IS NOT NULL
GROUP BY gender, year
HAVING year > 2016;####

# Para criar o ARRAY uso a função de agregação ARRAY_AGG, que agrega por gênero, digite os seguintes comandos:
SELECT gender, ARRAY_AGG (numtrips ORDER BY year) FROM 
(SELECT 
gender, EXTRACT(YEAR FROM starttime) AS year, COUNT(1) AS numtrips
FROM `bigquery-public-data`.new_york_citibike.citibike_trips citibike_trips
WHERE gender != 'unknown' AND starttime IS NOT NULL
GROUP BY gender, year
HAVING year > 2016)
GROUP BY gender;####

# Para usar a função STRUCT e criar uma estrutura digite o código:
SELECT [
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
] AS bikerides;####

Ou deste jeito:

SELECT [
STRUCT('MALE' , [9306602, 3955871]),
STRUCT('FEMALE', [3236735, 1260893] )
];####

# Usando a função ARRAY_LENGTH retorna o número de elementos
SELECT ARRAY_LENGTH(bikerides) FROM
(SELECT [
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
] AS bikerides);####

# Usamos o OFFSET(0) para extrair o primeiro elemento do Array usamos o código:
SELECT ARRAY_LENGTH(bikerides), bikerides[OFFSET(0)].gender AS fist_gender FROM
(SELECT [
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
] AS bikerides);####

# Usamos o OFFSET(1) para extrair o segundo elemento do Array com o código:
SELECT ARRAY_LENGTH(bikerides), bikerides[OFFSET(1)].gender AS fist_gender FROM
(SELECT [
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
] AS bikerides);####

# Usamos o OFFSET(x) para extrair o elemento desejado do Array usamos o código:
SELECT ARRAY_LENGTH(bikerides), bikerides[OFFSET(1)].numtrips[OFFSET(0)] AS fist_gender FROM
(SELECT [
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
] AS bikerides);####

# A função especial UNNEST recebe um array e trata seus elementos como linhas. Segue o código:
SELECT * FROM UNNEST ([
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
]);####

# Este código só retorna o valor de gender:
SELECT gender FROM UNNEST ([
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
]);####

# Neste código só retorna o valor de numtrips:
SELECT numtrips FROM UNNEST ([
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
]);####

# Este código retorna o valor do primeiro elemento de cada linha do array:
SELECT gender, numtrips[OFFSET(0)] AS First_Element FROM UNNEST ([
STRUCT('MALE' AS gender, [9306602, 3955871] AS numtrips),
STRUCT('FEMALE' AS gender, [3236735, 1260893] AS numtrips)
]);####

# Usando a função INNER JOIN para fazer a junção de duas tabelas. O código para as duas consultas ficam assim:
WITH FROM_ITEM_A AS 
(SELECT 'DALLAS' AS CITY, 'OR' AS STATE
UNION ALL SELECT 'TOKYO' AS CITY, 'TOKYO' AS STATE
UNION ALL SELECT 'MUMBAI' AS CITY, 'MAHARASHIRA' AS STATE),
FROM_ITEM_B AS (
SELECT 'OR' AS STATE, 'USA' AS COUNTRY
UNION ALL SELECT 'TOKYO' AS STATE, 'JAPAN' AS COUNTRY
UNION ALL SELECT 'MAHARASHIRA' AS STATE, 'INDIA' AS CONTRY)
SELECT FROM_ITEM_A.*, COUNTRY
FROM FROM_ITEM_A
INNER JOIN FROM_ITEM_B
ON FROM_ITEM_A.STATE = FROM_ITEM_B.STATE;####

# Verifique o resultado da junção das tabelas através do campo STATE, Podemos fazer uma consulta usando o sinal de diferente !=. Digite o seguinte código:
WITH FROM_ITEM_A AS 
(SELECT 'DALLAS' AS CITY, 'OR' AS STATE
UNION ALL SELECT 'TOKYO' AS CITY, 'TOKYO' AS STATE
UNION ALL SELECT 'MUMBAI' AS CITY, 'MAHARASHIRA' AS STATE),
FROM_ITEM_B AS (
SELECT 'OR' AS STATE, 'USA' AS COUNTRY
UNION ALL SELECT 'TOKYO' AS STATE, 'JAPAN' AS COUNTRY
UNION ALL SELECT 'MAHARASHIRA' AS STATE, 'INDIA' AS CONTRY)
SELECT FROM_ITEM_A.*, COUNTRY
FROM FROM_ITEM_A
INNER JOIN FROM_ITEM_B
ON FROM_ITEM_A.STATE != FROM_ITEM_B.STATE;####


# Com a cláusula CROSS JOIN é possível cruzarmos informações de duas ou mais tabelas através do produto cartesiano.
WITH FROM_ITEM_A AS 
(SELECT 'DALLAS' AS CITY, 'OR' AS STATE
UNION ALL SELECT 'TOKYO' AS CITY, 'TOKYO' AS STATE
UNION ALL SELECT 'MUMBAI' AS CITY, 'MAHARASHIRA' AS STATE),
FROM_ITEM_B AS (
SELECT 'OR' AS STATE, 'USA' AS COUNTRY
UNION ALL SELECT 'TOKYO' AS STATE, 'JAPAN' AS COUNTRY
UNION ALL SELECT 'MAHARASHIRA' AS STATE, 'INDIA' AS CONTRY)
SELECT FROM_ITEM_A.*, COUNTRY
FROM FROM_ITEM_A
CROSS JOIN FROM_ITEM_B;####

# Para usar o OUTER JOIN inclua o código:
WITH FROM_ITEM_A AS 
(SELECT 'DALLAS' AS CITY, 'OR' AS STATE
UNION ALL SELECT 'TOKYO' AS CITY, 'TOKYO' AS STATE
UNION ALL SELECT 'MUMBAI' AS CITY, 'MAHARASHIRA' AS STATE
UNION ALL SELECT 'A CORUNA' AS CITY, 'GALICIA' AS STATE
UNION ALL SELECT 'SÃO PAULO' AS CITY, 'SÃO PAULO' AS STATE
),
FROM_ITEM_B AS (
SELECT 'OR' AS STATE, 'USA' AS COUNTRY
UNION ALL SELECT 'TOKYO' AS STATE, 'JAPAN' AS COUNTRY
UNION ALL SELECT 'MAHARASHIRA' AS STATE, 'INDIA' AS CONTRY
UNION ALL SELECT 'RIO DE JANEIRO' AS STATE, 'BRASIL' AS CONTRY
UNION ALL SELECT 'MEXICO CITY' AS STATE, 'MEXICO' AS CONTRY
)
SELECT FROM_ITEM_A.*, COUNTRY
FROM FROM_ITEM_A
FULL OUTER JOIN FROM_ITEM_B
ON FROM_ITEM_A.STATE = FROM_ITEM_B.STATE;####

# Para usar o LEFT JOIN altere o código para:
WITH FROM_ITEM_A AS 
(SELECT 'DALLAS' AS CITY, 'OR' AS STATE
UNION ALL SELECT 'TOKYO' AS CITY, 'TOKYO' AS STATE
UNION ALL SELECT 'MUMBAI' AS CITY, 'MAHARASHIRA' AS STATE
UNION ALL SELECT 'A CORUNA' AS CITY, 'GALICIA' AS STATE
UNION ALL SELECT 'SÃO PAULO' AS CITY, 'SÃO PAULO' AS STATE
),
FROM_ITEM_B AS (
SELECT 'OR' AS STATE, 'USA' AS COUNTRY
UNION ALL SELECT 'TOKYO' AS STATE, 'JAPAN' AS COUNTRY
UNION ALL SELECT 'MAHARASHIRA' AS STATE, 'INDIA' AS CONTRY
UNION ALL SELECT 'RIO DE JANEIRO' AS STATE, 'BRASIL' AS CONTRY
UNION ALL SELECT 'MEXICO CITY' AS STATE, 'MEXICO' AS CONTRY
)
SELECT FROM_ITEM_A.*, COUNTRY
FROM FROM_ITEM_A LEFT JOIN FROM_ITEM_B
ON FROM_ITEM_A.STATE = FROM_ITEM_B.STATE;####

# Para usar o RIGHT JOIN digite o código;
WITH FROM_ITEM_A AS 
(SELECT 'DALLAS' AS CITY, 'OR' AS STATE
UNION ALL SELECT 'TOKYO' AS CITY, 'TOKYO' AS STATE
UNION ALL SELECT 'MUMBAI' AS CITY, 'MAHARASHIRA' AS STATE
UNION ALL SELECT 'A CORUNA' AS CITY, 'GALICIA' AS STATE
UNION ALL SELECT 'SÃO PAULO' AS CITY, 'SÃO PAULO' AS STATE
),
FROM_ITEM_B AS (
SELECT 'OR' AS STATE, 'USA' AS COUNTRY
UNION ALL SELECT 'TOKYO' AS STATE, 'JAPAN' AS COUNTRY
UNION ALL SELECT 'MAHARASHIRA' AS STATE, 'INDIA' AS CONTRY
UNION ALL SELECT 'RIO DE JANEIRO' AS STATE, 'BRASIL' AS CONTRY
UNION ALL SELECT 'MEXICO CITY' AS STATE, 'MEXICO' AS CONTRY
)
SELECT FROM_ITEM_A.*, COUNTRY
FROM FROM_ITEM_A RIGHT JOIN FROM_ITEM_B
ON FROM_ITEM_A.STATE = FROM_ITEM_B.STATE####


# No exemplo prático usaremos as seguintes tabelas públicas;
-- `bigquery-public-data`.new_york_citibike.citibike_trips
-- `bigquery-public-data.ghcn_d.ghcnd_2016`####

# O código seguinte mostra o uso das cláusulas vistas nesta aula:
WITH bicycle_rentals AS (
SELECT COUNT(starttime) as num_trips,
EXTRACT(DATE from starttime) as trip_date
FROM `bigquery-public-data`.new_york_citibike.citibike_trips
group by trip_date),
rainy_days AS (
SELECT date, (MAX(prcp) > 5) as rainy FROM (
SELECT date, IF(element = 'PRCP', value/10, NULL) as prcp
FROM `bigquery-public-data.ghcn_d.ghcnd_2016`
WHERE id = 'USW00094728')
GROUP BY date)
SELECT trip_date,
num_trips,
rainy_days.rainy
FROM bicycle_rentals
INNE JOIN rainy_days
ON trip_date = rainy_days.date
WHERE rainy_days.rainy = true
ORDER BY trip_date;####

 
####  BigQuery: Funções do BigQuery  ####  
Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.

# Na janela de consultas digite:
WITH example AS
(SELECT 'Sat' AS Day, 1451 AS numrides, 1018 AS oneways
UNION ALL SELECT 'Sun', 2376, 936)
SELECT *, (oneways/numrides) as frac_oneway, 
ROUND(oneways/numrides, 2) AS frac_noeway_round FROM example;####

# Para verificar o funcionamento do ROUND faça os testes conforme o vídeo, No código seguinte é prevenida a divisão por zero:
WITH example AS
(SELECT 'Sat' AS Day, 1451 AS numrides, 1018 AS oneways
UNION ALL SELECT 'Sun', 2376, 936
UNION ALL SELECT 'Wed', 0, 0)
SELECT *, ROUND(IEEE_Divide(oneways, numrides), 2) AS frac_noaway_round FROM example;####

# A função SAFE retorna valor NULL caso haja um erro. Digite o código:
SELECT SAFE.LOG (10, -3), SAFE.LOG (10, 3);####
6) Alterando a consulta para a situação do vídeo com seguinte código:
WITH example AS
(SELECT 'Sat' AS Day, 1451 AS numrides, 1018 AS oneways
UNION ALL SELECT 'Sun', 2376, 936
UNION ALL SELECT 'Mon', NULL, NULL
UNION ALL SELECT 'Tue', IEEE_Divide(3 ,0) , 0
UNION ALL SELECT 'Wed', IEEE_Divide(-3 ,0) , 0
)
SELECT * FROM example 
WHERE numrides < 2000;####

# Para verificar a precisão dos cálculos numéricos copie o código:
WITH example AS 
(SELECT 1.23 AS PAYMENT
UNION ALL SELECT 7.89
UNION ALL SELECT 12.43)
SELECT SUM(PAYMENT) AS TOTAL_PAYMENT, AVG(PAYMENT) AS AVG_PAYMENT
FROM example;####

# Com resultado:
-- 21.55 (sum)
-- 7.1833333333 (avg)####

# Para melhorar a precisão usamos o comando NUMERIC que converte um FLOAT em um NUMERIC:
WITH example AS 
(SELECT NUMERIC '1.23' AS PAYMENT
UNION ALL SELECT NUMERIC '7.89'
UNION ALL SELECT NUMERIC '12.43')
SELECT SUM(PAYMENT) AS TOTAL_PAYMENT, AVG(PAYMENT) AS AVG_PAYMENT
FROM example;####

# Existem outras funções matemáticas que podem ser usadas. Estes são alguns exemplos que pode ser vistos no vídeo:
•	SIGN - retorna um numero 1 ou 0. 1 se for positivo e 0 se for negativo
SELECT SIGN (-3.45);####

•	IS_INF checa se o numero é infinito ou não
SELECT IS_INF(IEEE_DIVIDE(0,0)), IEEE_DIVIDE(0,0);####

•	IS_NAN checa se o numero é NAN ou não
SELECT IS_NAN(IEEE_DIVIDE(3,0)), IEEE_DIVIDE(3,0);####

•	RAND - Gera um número randomico. entre 0 e 1 (exceto o 1)
SELECT RAND();####

•	SQRT - RAIZ QUADRADA
SELECT SQRT(144);####

•	POW - Elevado a potencia
SELECT POW (2, 4);####

•	LN, LOG, LOG10
SELECT LOG10(5);####

•	GREATEST - Retorna o maior numero dentro de um array.
SELECT GREATEST (1,2,3,3,4,4,5,4,5,6,7,6,5,8,2,3,4);####

•	LEAST - Retorna o maior numero dentro de um array.
SELECT LEAST (1,2,3,3,4,4,5,4,5,6,7,6,5,8,2,3,4);####

•	EXPRESSÕES MATEMATICAS - SAFE_ADD, SAFE_SUBSTRACT, SAFE_DIVIDE, SAVE_NEGATIVE
SELECT ((4+5)/3)*10;
SELECT SAFE_MULTIPLY (300000000000, 400000000000);####

•	MOD - Resto
SELECT MOD (10,3);####

•	ROUND, TRUNC
SELECT ROUND(3.42, 1), TRUNC(3.42, 1);####

•	CEIL E FLOOR
o	CEIL MAIOR INTEIRO DEPOIS DO NUMERO
o	FLOOR MENOR INTEIRO ANTES DO NUMERO
SELECT CEIL(3.48), FLOOR(3.48);####

•	SEN, COS, TAG, ACOS, ASEN, ATAG, SENH e outras.

# Segundo o vídeo o uso do RANGE_BUCKET. O código seguinte responde à pergunta:
# QUANTOS ALUNOS EU TENHO ENTRE 10 E 13, ENTRE 13 E 15 E ENTRE 15 E 18?
WITH Students AS
(SELECT 'A1' AS ALUNO, 11 AS AGE
UNION ALL SELECT 'A2' , 12
UNION ALL SELECT 'A3' , 11
UNION ALL SELECT 'A4' , 14
UNION ALL SELECT 'A5' , 17
UNION ALL SELECT 'A6' , 17
UNION ALL SELECT 'A7' , 18
UNION ALL SELECT 'A8' , 16
UNION ALL SELECT 'A9' , 11
UNION ALL SELECT 'A10' , 12
UNION ALL SELECT 'A11' , 13
UNION ALL SELECT 'A12' , 13
UNION ALL SELECT 'A13' , 16)
SELECT RANGE_BUCKET( AGE, [9, 13, 15, 19]), COUNT(*) FROM Students
GROUP BY 1;####

# No exemplo de operadores lógicos vá na janela do editor de consultas e digite:
SELECT
  gender, tripduration
FROM
  `bigquery-public-data`.new_york_citibike.citibike_trips
  WHERE (tripduration < 600 AND gender = 'female') OR gender = 'male'
  LIMIT 100;####

# Nos códigos seguinte são testados os operadores lógicos:
WITH example AS (
  SELECT true AS is_valid, 'a' as letter, 1 as position
  UNION ALL SELECT false , 'b', 2
  UNION ALL SELECT false , 'c', 3
)
SELECT * FROM example;####

# Teste os operadores lógicos usando o código:
WITH example AS (
  SELECT true AS is_valid, 'a' as letter, 1 as position
  UNION ALL SELECT false , 'b', 2
  UNION ALL SELECT false , 'c', 3
)
SELECT * FROM example WHERE is_valid != false AND position > 0;####

# Teste usando um campo lógico como condição:
WITH example AS (
  SELECT true AS is_valid, 'a' as letter, 1 as position
  UNION ALL SELECT false , 'b', 2
  UNION ALL SELECT false , 'c', 3)
SELECT * FROM example WHERE is_valid AND position > 0;####

# Colocando um operador lógico para testar a cláusula WHERE alterando o código que acessa a base de dados pública:
SELECT
  gender, tripduration
FROM
  `bigquery-public-data`.new_york_citibike.citibike_trips
  WHERE (tripduration < 600 AND gender = 'female') 
  LIMIT 100;####

# A expressão completa para a cláusula WHERE mostrando todos os operadores lógicos:
SELECT
  gender, tripduration
FROM
  `bigquery-public-data`.new_york_citibike.citibike_trips
  WHERE (tripduration < 600 AND gender = 'female') = true
  LIMIT 100;####

# Para testar expressões condicionais use o código:
WITH catalog AS (
  SELECT 30.0 AS costPrice, 0.15 as margin, 0.1 as taxRate
  UNION ALL SELECT NULL, 0.21, 0.15
  UNION ALL SELECT 30.0, NULL, 0.09
  UNION ALL SELECT 30.0, 0.30, NULL
  UNION ALL SELECT 30.0, NULL, NULL
)
SELECT * FROM catalog;####

#) Após as alterações do vídeo o código fica assim:
WITH catalog AS (
  SELECT 30.0 AS costPrice, 0.15 as margin, 0.1 as taxRate
  UNION ALL SELECT NULL, 0.21, 0.15
  UNION ALL SELECT 30.0, NULL, 0.09
  UNION ALL SELECT 30.0, 0.30, NULL
  UNION ALL SELECT 30.0, NULL, NULL
)
SELECT ROUND (
  IF (costPrice IS NULL, 30.0, costPrice) * 
  IF (margin IS NULL, 0.10, margin) * 
  IF (taxrate IS NULL, 0.15, taxrate) , 2
) as FORMULA FROM catalog;####

# No editor de consultas teste as expressões:
SELECT COALESCE ('A', 'B', 'C');####

E:
SELECT COALESCE (NULL, 'B', 'C');####

# Aplicando COALESE na expressão anterior. Alteramos o código para:
WITH catalog AS (
  SELECT 30.0 AS costPrice, 0.15 as margin, 0.1 as taxRate
  UNION ALL SELECT NULL, 0.21, 0.15
  UNION ALL SELECT 30.0, NULL, 0.09
  UNION ALL SELECT 30.0, 0.30, NULL
  UNION ALL SELECT 30.0, NULL, 0.10
)
SELECT 
  IF (costPrice IS NULL, 30.0, costPrice) * 
  IF (margin IS NULL, 0.10, margin) * 
  IF (taxrate IS NULL, 0.15, taxrate) 
  as FORMULA1 ,
  COALESCE (
    costPrice * margin * taxrate, 
    30.0 * margin * taxrate, 
    costprice * 0.10 * taxrate, 
    costPrice * margin * 0.15
  ) as FORMULA2 FROM catalog;####

# Usamos o SAFE_CAST que é idêntico a CAST, exceto que retorna NULL em vez de gerar um erro:
WITH example AS (
    SELECT 'Jonh' AS employee, 'Doente' as Hours_work
    UNION ALL SELECT 'Jean', '100'
    UNION ALL SELECT 'Peter', 'De férias'
    UNION ALL SELECT 'Mary', '80'
)
SELECT SUM (SAFE_CAST(Hours_work AS INT64)) AS TOTAL FROM example;####

# O código seguinte mostra o uso da CASE WHEN:
WITH Numbers AS (
    SELECT 90 as A, 2 as B
    UNION ALL SELECT 50, 8
    UNION ALL SELECT 60, 6
    UNION ALL SELECT 50, 10
)
SELECT A, B, 
    CASE 
        WHEN (A = 90 AND B > 10) THEN 'red'
        WHEN A = 50 THEN 'blue'
        ELSE 'green' END AS Color
    FROM Numbers;####

# Comparando o CASE WHEN e RANGE_BUCKET usando o código:
WITH Students AS
(SELECT 'A1' AS ALUNO, 11 AS AGE
UNION ALL SELECT 'A2' , 12
UNION ALL SELECT 'A3' , 11
UNION ALL SELECT 'A4' , 14
UNION ALL SELECT 'A5' , 17
UNION ALL SELECT 'A6' , 17
UNION ALL SELECT 'A7' , 18
UNION ALL SELECT 'A8' , 16
UNION ALL SELECT 'A9' , 11
UNION ALL SELECT 'A10' , 12
UNION ALL SELECT 'A11' , 13
UNION ALL SELECT 'A12' , 13
UNION ALL SELECT 'A13' , 16)
SELECT ALUNO, RANGE_BUCKET( AGE, [9, 13, 15, 18]),
CASE 
  WHEN AGE >= 9 AND AGE < 13 THEN '1'
  WHEN AGE >= 13 AND AGE < 15 THEN '2'
  WHEN AGE >= 15 AND AGE < 18 THEN '3'
  ELSE '4' END 
FROM Students;####

# Salve as consultas desta aula.

# No exemplo para testar a função LOWER vá na janela do editor de consultas e digite:
WITH items AS
 (SELECT 'FOO' AS ITEM
 UNION ALL SELECT 'BAR'
 UNION ALL SELECT 'BAZ')
 SELECT LOWER(ITEM) FROM items;####

# Para testar a função UPPER copie:
WITH items AS
 (SELECT 'foo' AS ITEM
 UNION ALL SELECT 'bar'
 UNION ALL SELECT 'baz')
 SELECT UPPER(ITEM) FROM items;####

# Usando LOWERe UPPER use o código:
WITH items AS
 (SELECT 'rUA Antonio Carlos' AS ITEM
 UNION ALL SELECT 'AVENIDA sÃO pAULO'
 UNION ALL SELECT 'tv atroS DO aLTO')
 SELECT ITEM, UPPER(ITEM), LOWER(ITEM) FROM items;####

# A função INITCAP funciona com o código:
WITH examples AS
 (SELECT "Alo Mundo-todo mundo!" AS FRASES
 UNION ALL SELECT "o cachorro TORNADO é alegre+manso"
 UNION ALL SELECT "maça&laranja&pera"
 UNION ALL SELECT "tata ta tavendo a tatia")
 SELECT FRASES, INITCAP(FRASES) FROM examples;####

# Para usar delimitadores junto com o INITCAP copie o código:
WITH examples AS
 (SELECT "Alo Mundo-todo mundo!" AS FRASES, " " AS DELIMITER
 UNION ALL SELECT "o cachorro TORNADO é alegre+manso", "+"
 UNION ALL SELECT "maça&laranja&pera", "&"
 UNION ALL SELECT "tata ta tavendo a tatia", "t")
 SELECT FRASES, INITCAP(FRASES), INITCAP(FRASES, DELIMITER) FROM examples;####

# Para mostrar o uso das funções LTRIM, RTRIM e TRIM use como exemplo o código:
WITH items AS
 (SELECT "     MAÇA     " AS ITEM
 UNION ALL SELECT "     BANANA     "
 UNION ALL SELECT "     LARANJA     ")
 SELECT ITEM, LTRIM(ITEM), RTRIM(ITEM), TRIM(ITEM) FROM items;####

# As funções LEFT e RIGHT também podem ser combinadas como neste código.
WITH items AS
 (SELECT "     MAÇA     " AS ITEM
 UNION ALL SELECT "     BANANA     "
 UNION ALL SELECT "     LARANJA     ")
 SELECT TRIM(ITEM), LEFT(TRIM(ITEM), 2), RIGHT(TRIM(ITEM),2) FROM items;####

# Usando a função CONCAT para concatenar campos de uma tabela. Use o código:
WITH examples AS
(SELECT "DR" AS Titulo, "Carlos" as NOME, "Junior" as SOBRENOME
UNION ALL SELECT "SR", "Marcos", "Almeida"
UNION ALL SELECT "DR" , "Mario", "Costa"
UNION ALL SELECT "MS" , "Maria", "Rosa")
SELECT CONCAT (Titulo, " ", Nome, " ", Sobrenome) FROM examples;####

# Usando a função CHAR_LENGTH para retornar o número de caracteres que o string possui. Digite o código:
WITH examples AS
(SELECT "DR" AS Titulo, "Carlos" as NOME, "Junior" as SOBRENOME
UNION ALL SELECT "SR", "Marcos", "Almeida"
UNION ALL SELECT "DR" , "Mario", "Costa"
UNION ALL SELECT "MS" , "Maria", "Rosa")
SELECT CONCAT (Titulo, " ", Nome, " ", Sobrenome), 
CHAR_LENGTH(CONCAT (Titulo, " ", Nome, " ", Sobrenome)) FROM examples;####

# Usando a função STARTS_WITH para os campos que iniciam com Dr e terminam com a letra a. Digite o código:
WITH examples AS
(SELECT "DR" AS Titulo, "Carlos" as NOME, "Junior" as SOBRENOME
UNION ALL SELECT "SR", "Marcos", "Almeida"
UNION ALL SELECT "DR" , "Mario", "Costa"
UNION ALL SELECT "MS" , "Maria", "Rosa")
SELECT CONCAT (Titulo, " ", Nome, " ", Sobrenome), 
CHAR_LENGTH(CONCAT (Titulo, " ", Nome, " ", Sobrenome)),
STARTS_WITH(CONCAT (Titulo, " ", Nome, " ", Sobrenome), "DR"), 
ENDS_WITH(CONCAT (Titulo, " ", Nome, " ", Sobrenome), "Dr") FROM examples;####

# Continuando com as funções de stringtemo a INSTR que funciona conforme o código:
WITH example AS
(SELECT 'banana' AS source_value, 'an' AS search_value, 1 as position, 1 as occcurrence
UNION ALL SELECT 'banana' AS source_value, 'an' AS search_value, 3 as position, 1 as occcurrence
UNION ALL SELECT 'banana' AS source_value, 'xx' AS search_value, 1 as position, 2 as occcurrence)
SELECT *, INSTR(source_value, search_value, position, occcurrence) FROM example;####

# A função SUBSTR neste exemplo retorna os três caracteres a partir da posição 3 como no código:
WITH example AS
(SELECT 'banana' AS source_value,
UNION ALL SELECT 'melancia'
UNION ALL SELECT 'tangerina')
SELECT source_value, SUBSTR(source_value,3,3) FROM example;####

# A função SUBSTR neste exemplo retorna todos os caracteres a partir da posição 3 como no código:
WITH example AS
(SELECT 'banana' AS source_value,
UNION ALL SELECT 'melancia'
UNION ALL SELECT 'tangerina')
SELECT source_value, SUBSTR(source_value,3) FROM example;####

# STRPOS neste exemplo retorna a posição do caractere @ :
WITH example AS
(SELECT 'foo@example.com' AS source_value,
UNION ALL SELECT 'victor@gmail.com'
UNION ALL SELECT 'quexample@brazil.com')
SELECT source_value, STRPOS(source_value, "@") FROM example;####

# Este código sua as funções SUBSTR e STRPOS combinadas.
WITH example AS
(SELECT 'foo@example.com' AS source_value,
UNION ALL SELECT 'victor@gmail.com'
UNION ALL SELECT 'quexample@brazil.com')
SELECT source_value, SUBSTR(source_value,1, STRPOS(source_value, "@") - 1) FROM example;####

# Neste exemplo vemos o uso da função REVERSE
WITH example AS
(SELECT 'foo@example.com' AS source_value,
UNION ALL SELECT 'victor@gmail.com'
UNION ALL SELECT 'quexample@brazil.com')
SELECT source_value, REVERSE(source_value) FROM example;####

# REPLACE é usado desta forma:
WITH example AS
(SELECT 'foo@example.com' AS source_value,
UNION ALL SELECT 'victor@gmail.com'
UNION ALL SELECT 'quexample@brazil.com')
SELECT source_value, REPLACE(source_value, "@","XXXXXX") FROM example;####

1#A função SPLIT é mostrada no código:
WITH example AS
(SELECT 'foo@example.com' AS source_value,
UNION ALL SELECT 'victor@gmail.com'
UNION ALL SELECT 'quexample@brazil.com')
SELECT source_value, SPLIT(source_value, "@") FROM example;####

# Uma Expressão Regular é uma representação para que você encontre padrões em um texto. O exemplo mostra o uso das funções REGEXP_CONTAINS, REGEXP_EXTRACT, REGEXP_EXTRACT_ALL e REGEXP_REPLACE:
SELECT FIELD,
REGEXP_CONTAINS(FIELD, r'[0-9]{5}-[0-9]{3}') AS TEM_CEP,
REGEXP_EXTRACT(FIELD, r'[0-9]{5}-[0-9]{3}', 1, 1) AS CEP,
REGEXP_EXTRACT(FIELD, r'[0-9]{5}-[0-9]{3}', 1, 2) AS CEP2,
REGEXP_EXTRACT_ALL(FIELD, r'[0-9]{5}-[0-9]{3}') AS CEP3,
REGEXP_REPLACE(FIELD, r'[0-9]{5}-[0-9]{3}', 'XXXXX-XXX') AS CEP2,
FROM
(SELECT * from UNNEST
(["22222-22","     22222-222  ","Meu CEP é 222222-22", "Do CEP 22222-222 ATÉ O 22333-222"]) AS FIELD);####

# Para testar a função FORMAT use o código SELECT FORMAT("%d", 10);
# O comando SELECT FORMAT("%015d", 10);retorna 000000000000010 como resposta.


# Veja a diferença dos resultados ao testar as funções de Data e Hora. Use o código:
SELECT CURRENT_DATETIME,
CURRENT_TIMESTAMP,
CURRENT_DATE,
CURRENT_TIME;####

#  Podem ser incluídas informações geográficas como no exemplo:
SELECT CURRENT_DATETIME('America/Sao_Paulo'),
CURRENT_DATETIME('Europe/London'),
CURRENT_TIMESTAMP,
CURRENT_DATE,
CURRENT_TIME;####

#  Para definir Datae Hora use como exemplo:
SELECT TIMESTAMP('2020-07-01 10:00:00'),
DATETIME (2020, 7, 1, 10, 0 , 0),
DATE(2020, 7, 1),
TIME(10,0,0);####

#  TIMESTAMP pode ser usado como parâmetro em funções de Data e Hora.
SELECT DATE(TIMESTAMP('2020-07-01 10:00:00')),
DATETIME(TIMESTAMP('2020-07-01 10:00:00')),
TIME(TIMESTAMP('2020-07-01 10:00:00'));####

#  Operações matemáticas com datas. Verifique o que cada função retorna.
SELECT 
  DATE_ADD (DATE(2008, 12, 25), INTERVAL 5 DAY) AS CINCO_DIAS_DEPOIS,
  DATE_ADD (DATE(2008, 12, 25), INTERVAL 4 YEAR) AS QUATRO_ANOS_DEPOIS,
  TIMESTAMP_ADD (CURRENT_TIMESTAMP, INTERVAL 45 MINUTE) AS QUARENTA_CINCO_MINUTOS_DEPOIS;####
  
#  Para subtrair datas vemos o exemplo:
SELECT 
  DATE_SUB (DATE(2008, 12, 25), INTERVAL 5 DAY) AS CINCO_DIAS_ANTES,
  DATE_SUB (DATE(2008, 12, 25), INTERVAL 4 YEAR) AS QUATRO_ANOS_ANTES,
  TIMESTAMP_SUB (CURRENT_TIMESTAMP, INTERVAL 45 MINUTE) AS QUARENTA_CINCO_MINUTOS_ANTES;####
  
#  Para saber os dias ou horas entre data usamos o exemplo:
SELECT 
  DATE_DIFF (DATE(2010,12,25), DATE(2008, 9, 15), DAY),
  DATETIME_DIFF (CURRENT_DATETIME, DATETIME(TIMESTAMP('2020-07-01 10:00:00')), MINUTE);####
  
#  Para extrair elementos de uma Data usamos EXTRACT como está mostrado a seguir:
SELECT DATA,
  EXTRACT(MONTH FROM DATA) AS MES,
  EXTRACT(DAY FROM DATA) AS DIA,
  EXTRACT(YEAR FROM DATA) AS ANO,
  EXTRACT(DAYOFWEEK FROM DATA) AS SEMANA
FROM UNNEST (GENERATE_DATE_ARRAY('2015-12-23', '2016-01-09')) AS DATA
ORDER BY DATA;####

#  Para testar e verificar as datas truncadas acompanhe no vídeo a explicação do código:
SELECT 
  DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), 
  DATETIME_TRUNC(DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), DAY), 
  DATETIME_TRUNC(DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), MINUTE), 
  DATETIME_TRUNC(DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), MONTH), 
  DATETIME_TRUNC(DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), YEAR);####
  
#  Os últimos dias do mês e ano podem ser obtidos com o código:
SELECT 
  DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), 
  LAST_DAY(DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), MONTH),
  LAST_DAY(DATETIME_ADD(CURRENT_DATETIME, INTERVAL 90 DAY), YEAR);####
  
#  As funções baseadas em FORMAT (FORMAT_TIME, FORMAT_DATETIME, FORMAT_DATE, FORMAT_TIMESTAMP) usam a seguinte notação:
<FUNCAO> (<FORMATO>, VALOR)####
  
##########################   Onde podemos usar os seguintes elementos no campo FORMATO:   ##########################   
	
%A	O nome completo do dia da semana
%a	O nome abreviado do dia da semana
%B	O nome completo do mês
%b ou %h	O nome abreviado do mês
%C	O século (um ano dividido por 100 e truncado como um inteiro) como um número decimal (00-99)
%D	A data no formato %m/%d/%y
%d	O dia do mês como número decimal (01-31)
%e	O dia do mês como número decimal (1-31). Dígitos únicos são precedidos por um espaço
%F	A data no formato %Y-%m-%d
%G	O ano ISO 8601 com o século como número decimal. Cada ano ISO começa na segunda-feira antes da primeira quinta-feira do ano calendário gregoriano. Observe que %G e %Y podem produzir -- resultados diferentes próximos aos limites do ano gregoriano, em que o ano gregoriano e o ano ISO podem divergir
%g	O ano ISO 8601 sem o século como número decimal (00-99). Cada ano ISO começa na segunda-feira antes da primeira quinta-feira do ano calendário gregoriano. Observe que %g e %y podem --- produzir resultados diferentes próximos aos limites do ano gregoriano, em que o ano gregoriano e o ano ISO podem divergir
%j	O dia do ano como número decimal (001-366)
%m	O mês como número decimal (01-12)
%n	Um caractere de nova linha
%Q	O trimestre como um número decimal (de 1–4)
%t	Um caractere de tabulação
%U	O número da semana do ano (domingo como o primeiro dia da semana) como número decimal (00-53)
%u	O dia da semana (segunda-feira como o primeiro dia da semana) como número decimal (1-7)
%V	O número da semana ISO 8601 do ano (segunda-feira como o primeiro dia da semana) como um número decimal (01–53). Se a semana que tem 1 de janeiro tiver quatro ou mais dias no ano ----- novo, então será a semana 1. Caso contrário, será a semana 53 do ano anterior e a semana seguinte será a semana 1
%W	O número da semana do ano (segunda-feira como o primeiro dia da semana) como número decimal (00-53)
%w	O dia da semana (domingo como o primeiro dia da semana) como número decimal (0-6)
%x	A representação de data no formato MM/DD/YY
%Y	O ano com o século como número decimal
%y	O ano sem o século como número decimal (00-99), com um zero opcional à esquerda. Pode ser misturado com %C. Se %C não for especificado, os anos 00-68 são os 2000, enquanto os anos 6--- 69-99 são os 1900
%E4Y	Anos com quatro caracteres (0001 ... 9999). %Y produz quantos caracteres forem necessários para processar totalmente o ano
%H	A hora em um relógio de 24 horas como número decimal (00-23)
%I	A hora em um relógio de 12 horas como número decimal (01-12)
%j	O dia do ano como número decimal (001-366)
%k	A hora em um relógio de 24 horas como número decimal (0-23). Dígitos únicos são precedidos por um espaço
%l	A hora em um relógio de 12 horas como número decimal (1-12). Dígitos únicos são precedidos por um espaço
%M	O minuto como número decimal (00-59)
%n	Um caractere de nova linha
%P	am ou pm
%p	AM ou PM
%R	A hora no formato %H:%M
%r	A hora em um relógio de 12 horas usando a notação AM/PM
%S	O segundo como número decimal (00-60)
%T	A hora no formato %H:%M:%S
%t	Um caractere de tabulação
%X	A representação da hora no formato HH:MM:SS
%%	Um único caractere %
%E#S	Segundos com # dígitos de precisão fracionária
%E*S	Segundos com precisão fracionária total (um literal '*')

#  A expressão é exemplo para a função FORMAT com datas:
SELECT CURRENT_DATETIME, FORMAT_DATETIME('%A, Dia %d de %B de %Y', CURRENT_DATETIME);####

#  Digite o código que acessa uma tabela pública para acompanhar o resultado no vídeo:
SELECT
  visitStartTime FROM
 `bigquery-public-data.google_analytics_sample.ga_sessions_20170731`
LIMIT 10;####

#  Para retornar
SELECT
  visitStartTime, TIMESTAMP_SECONDS(visitStartTime) FROM
 `bigquery-public-data.google_analytics_sample.ga_sessions_20170731`
LIMIT 10;####

#  O código retorna a quantidade de segundos até hoje desde 01-01-1970.
SELECT CURRENT_TIMESTAMP, UNIX_SECONDS(CURRENT_TIMESTAMP);####

#  A partir de agora sempre copie o código para o console do BigQuery e para o shell do BigQuery Geo Viz.Para retornar um ponto no mapa use o código:
SELECT ST_GEOGPOINT(longitude, latitude) AS Station, num_bikes_available
FROM 
`bigquery-public-data.new_york.citibike_stations`
WHERE num_bikes_available > 10;####

#  Para fazer uma linha entre dois pontos digite o código:
SELECT ST_MAKELINE(ARRAY_AGG(Ponto)) as Linha FROM 
(SELECT ST_GEOGPOINT(-22.9349, -43.1730) AS Ponto
UNION ALL SELECT ST_GEOGPOINT(-22.9365, -43.1771));####

#  Para calcular a distância entre dois pontos use o código:
SELECT ST_DISTANCE (ST_GEOGPOINT(-22.9349, -43.1730),
                    ST_GEOGPOINT(-22.9365, -43.1771))
AS Distancia;####

#  Para desenhar um polígono copie o código:
SELECT ST_MAKEPOLYGON(ST_MAKELINE(ARRAY_AGG(Ponto))) as Poligono FROM 
(SELECT ST_GEOGPOINT(-22.9349, -43.1730) AS Ponto
UNION ALL SELECT ST_GEOGPOINT(-22.9365, -43.1771)
UNION ALL SELECT ST_GEOGPOINT(-22.9375, -43.1781));####

#  Para calcular a área do polígono use o código:
SELECT ST_AREA(ST_MAKEPOLYGON(ST_MAKELINE(ARRAY_AGG(Ponto)))) as Area FROM 
(SELECT ST_GEOGPOINT(-22.9349, -43.1730) AS Ponto
UNION ALL SELECT ST_GEOGPOINT(-22.9365, -43.1771)
UNION ALL SELECT ST_GEOGPOINT(-22.9375, -43.1781));####

#  Vamos fazer algumas consultas usando a tabela curso-big-query-0965.sucos_vendas.tabela_de_clientes referente a base de dados criada e carregada no curso anterior.Para saber se a data da idade dos clientes, na base de dados, corresponde a idade real podemos fazer a seguinte consulta:
WITH TAB_IDADE AS (
SELECT NOME, DATE_DIFF(CURRENT_DATE, DATA_DE_NASCIMENTO, YEAR) AS IDADE_ATUAL, IDADE 
FROM `curso-big-query-0965.sucos_vendas.tabela_de_clientes`)

SELECT NOME, IDADE_ATUAL, IDADE,
CASE WHEN (IDADE_ATUAL - IDADE) <> 0 THEN 'IDADE NÃO BATE'
ELSE 'IDADE BATE COM A BASE DE DADOS' END AS RESULTADO FROM TAB_IDADE;####
20) Outro problema é o de apresentar o endereço completo de cada cliente Para isso faça:
SELECT CPF, NOME, CONCAT(ENDERECO_1, ' ', BAIRRO, ' ', CIDADE, ' ', ESTADO, ' ', CEP) AS ENDERECO_COMPLETO FROM `curso-big-query-0965.sucos_vendas.tabela_de_clientes`
ORDER BY NOME ;####


