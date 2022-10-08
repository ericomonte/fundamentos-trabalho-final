# Análise de dados sobre a cobertura vacinal no Brasil
Trabalho final da disciplina "Fundamentos e ética em jornalismo de dados" do Master em jornalismo de dados do Insper

Grupo 4 - Érico Monte, Gabriel Ronan, Graziela França e Natália Santos

A base de dados utilizada é a do Sistema de Avaliação do Programa de Imunizações. A justificativa para escolha dessa base se deve à baixa cobertura vacinal que o Brasil tem tido da poliomielite nesta campanha. Além disso, outras doenças que já haviam sido erradicadas, como sarampo, rubéola e difteria, também correm o risco de retornar ao país ou já retornaram. Diante disso, e de todos os problemas enfrentados durante a pandemia, consideramos que o assunto está em evidência e a análise desse tipo de dado é de total interesse público neste cenário.

A base de dados foi obtida no site [Base de Dados](https://basedosdados.org/dataset/br-ms-imunizacoes?bdm_table=municipio). Os números foram compilados pelo Ministério da Saúde e são atualizados anualmente.

**Limitações:** 

- A tabela traz uma coluna das aplicações de imunizantes não especificadas. Precisamos entender se esses dados são de vacinas cuja definição não foram documentadas pelos aplicadores.

- A tabela não traz também os microdados, ou seja, informações sobre as pessoas que se imunizaram e, assim, não é possível avaliar o perfil daqueles que tomaram as vacinas, quantas tomaram com atraso, quais grupos tendem a completar ou não a imunização, entre outras.

## Investigando a base de dados

1. Iniciamos a investigação pesquisando ***Quais vacinas tiveram maior queda de cobertura?***

Todas as vacinas utilizadas para prevenir poliomielite, meningite e sarampo (tríplice viral, tetra viral ou tetravalente, sarampo, poliomielite e meningococo) apresentaram redução na cobertura entre 2012 a 2021. Destes imunizantes, a 2ª dose da tríplice viral foi a que mais reduziu, saindo de uma cobertura de mais de 100% em 2014 para 56% em 2021. 

**Algumas observações:**
- A 2ª dose da tríplice viral e a tetra viral só começam a computar os dados a partir de 2013.
- A tetravalenta só computa até 2016, por isso foi descartada da análise. 

**Passo a passo:**
Após ter baixado a base de dados, criamos um banco de dados no SQL e nomeamos por “imunização”. Em seguida, fizemos o upload do arquivo para o banco de dados e começamos o processo de execução. Nesta etapa, pesquisamos quais vacinas estavam ligadas às doenças e fizemos um filtro da média por ano da cobertura de cada uma dessas vacinas com a seguinte fórmula: 

SELECT ano, avg(cobertura_poliomielite), avg(cobertura_meningococo), avg(cobertura_sarampo), avg(cobertura_tetravalente), avg(cobertura_tetra_viral), avg(cobertura_triplice_viral_d1), avg(cobertura_triplice_viral_d2)

FROM imuni

GROUP by ano

ORDER by ano 

Após isso, fizemos as análises dentro do período de anos selecionados (2012 a 2021). [A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=180170413).


2. Em seguida perguntamos como isso estava representado territorialmente: ***Qual o ranking dos estados com menor cobertura para as doenças antes erradicadas, como sarampo, pólio, rubéola e difteria?***

**Vacinação dtp**

Os dados mostram que o último ano que teve essa vacinação foi em 2016, na cidade de Balneário Rincão em Santa Catarina. Lá, 192 doses foram aplicadas, uma cobertura de 95%. Em 2015, apenas duas cidades aparecem com aplicação da vacina: Pescaria Brava (SC) e Mojuí dos Campos (PA).
O ano mais recente com mais aplicações da dose foi em 2002. A maior aplicação foi em Lagoa Santa (GO) com cobertura de 214%.
 [A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=48535446)

**Vacinação poli**

Os dados mostram que o último ano que teve essa vacinação foi em 2016 e, novamente, na cidade de Balneário Rincão em Santa Catarina. Na época, 167 doses foram aplicadas, uma cobertura de 83%. O ano mais recente com mais aplicações da dose foi em 2002. A maior aplicação foi em Lagoa Santa (GO) com cobertura de 628%.
[A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=1066864454)

**Vacinação sarampo**

Os dados mostram que o último ano que teve vacinação de sarampo foi em 2002. A cidade com maior cobertura foi Palmeira (SC) com 314% de cobertura.
[A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=543481050)

Observação: parece ter havido um caso de _missing data_, pois foram realizadas campanhas de vacinação de sarampo nos últimos anos.

**Vacinação tríplice viral d1 e d2**

Balneário Rincão aparece novamente como a única cidade de 2016 com vacinação de tríplice viral d1. No ano citado, 61 doses foram aplicadas, uma cobertura de 30%.
A mesma cidade em 2016 foi a única que aplicou a df: 169 vacinas, uma cobertura de 84%.
As tabelas podem ser vistas [aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=853446266) e [aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=490459698).

**Passo a passo:**
Importamos também no SQL uma base de dados do IBGE com a população estimada de 2021. Assim trouxemos para a base de vacinação informações importantes como o nome do município, o uf e a população estimada.  Para cada uma das vacinas, fizemos o mesmo esquema abaixo, alterando somente os termos em negrito:

select ano, id_municipio, UF, NOMEDOMUNICÍPIO, POPULAÇÃOESTIMADA, **doses_sarampo, cobertura_sarampo**

from vacinacao as v join pop as p

on v.id_municipio = p.field4

group by id_municipio

order by **COBERTURA_SARAMPO** DESC


3. Ao aprofundarmos nosso conhecimento na base de dados, identificamos um ponto de atenção:   ***A tabela traz uma coluna das aplicações de imunizantes não especificadas. Quais municípios têm o pior controle de sua campanha de vacinação? Alguma região/estado se destaca nesta análise?***

Pela análise dos dados, sabemos que 2.365 prefeituras brasileiras não registram ao menos metade das vacinas que aplicam em suas populações, portanto 44,6% das cidades estão nessa condição. Esses dados escancaram que o Brasil é falho no registro das ações feitas no âmbito do Plano Nacional de Imunizações (PNI). 

Ainda sobre o não registro de vacinas, a análise dos dados mostra que o Rio Grande do Sul é o pior Estado em percentual de doses não aplicadas: 52,29% das vacinas desempenhadas na unidade federativa não tem registro de qual imunizante foi usado. Na sequência, aparecem quatro estados do Nordeste: Rio Grande do Norte, Piauí, Paraíba e Alagoas.

Entre as cidades, os seis maiores percentuais de doses ignoradas são de municípios do RS: União da Serra, Coqueiro Baixo, Herval, Coronel Pilar, Relvado e Morro Redondo. O top 10 ainda tem outras duas cidades dessa unidade federativa: Santa Tereza (8ª) e Cerrito (9ª). Completam as 10 primeiras as prefeituras de Senador José Bento (MG) e Águas de São Pedro (SP). Todas computam ao menos 61% de doses não especificadas.

**Passo a passo:**
Criamos um banco de dados no SQL e o nomeei como “sis_vacina”. Em seguida, importamos o arquivo em CSV para o banco de dados e comecei o processo de execução em SQL. 

Nesta etapa, pesquisamos quais municípios brasileiros são mais falhos no registro das doses que aplicam em sua população, a partir da coluna “doses_ignorado”. Segue a query usada para a consulta:

SELECT id_municipio, Nome_Município as cidade, sum(doses_total) as total_aplicadas, sum(doses_ignorado) as total_ignoradas, Nome_UF

FROM sis_vacina

LEFT JOIN cod_cidade

ON CódigoMunicípioCompleto = id_municipio

GROUP BY cidade

ORDER BY total_ignoradas DESC

[A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1Bqji5RKiQnWcxSsAwfyAFTek9N9hBdT_z99w4Qon8xM/edit#gid=807994399)


4. Por fim, investigamos como está o cenário no país: ***Em média, quais estados brasileiros mais vacinam e menos vacinam seus cidadãos para cada doença? Alguma unidade da federação se destaca para o bem ou para o mal?***

A Bahia apresenta a menor cobertura vacinal do país para poliomielite,  meningococo e tríplice viral d1. O Distrito Federal apresenta a maior cobertura vacinal para tríplice viral d1 e d2, meningococo e a segunda melhor cobertura para poliomielite.

**Passo a passo:**
Criamos um banco de dados no SQL e o nomeei como “sist_imunizacao”. Em seguida, importamos o arquivo em CSV para o banco de dados e comecei o processo de execução em SQL. 

SELECT sigla_uf, sum(cobertura_poliomielite), sum(cobertura_meningococo), sum(cobertura_tetravalente), sum(cobertura_tetra_viral), sum(cobertura_triplice_viral_d1), sum(cobertura_triplice_viral_d2)

FROM sist_imunizacao

where ano>2011

GROUP by sigla_uf

[A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=346068398)

Decidimos também fazer um recorte temporal mais curto para investigar a quantidade de doses aplicadas por estado pré e pós pandemia. Entre 2012 e 2019, o número de doses aplicadas por estado, em média, subiu 108%. Embora, em oito estados o número de doses tenha caído.

Entre 2019 (último ano pré-pandemia) e 2021, o número de doses aplicadas por estado, em média, caiu 28%. Todos os estados apresentaram queda. Destaque para o Amapá, onde a redução foi de 40%. O Espírito Santo apresentou a menor redução: 18%.

**Passo a passo:**
Utilizamos a seguinte fórmula:

SELECT sigla_uf, sum(doses_total)

FROM sist_imunizacao

where ano=2012

GROUP by sigla_uf

Repetimos a fórmula acima com anos de 2019 e 2021. Exportamos o CSV e levamos para uma planilha, adicionando as coluna "2019/2012" e "2021/2019" para calcular o percentual de doses aplicadas de um ano sobre o outro.

[A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1iPHrT-psGWY3QvCFPvf59pLs4HfGTwGLRsLyhpv4arI/edit#gid=1537710458)

## Para onde esses insights poderiam nos levar

As vacinas são responsáveis pela eliminação de diversas doenças no país, como rubéola, sarampo e poliomielite. No entanto, nos últimos anos, com o surgimento de movimentos antivacinas e o desconhecimento da população sobre os efeitos dessas enfermidades são alguns dos possíveis motivos para que as crianças desta década estejam cada vez mais desprotegidas. O reflexo disso pode ser observado com a baixa cobertura vacinal registrada. Campanhas são prorrogadas em todo o país e, mesmo assim, os índices mínimos para proteção da população não são atingidos.

De acordo com dados do Sistema de Imunização do Ministério da Saúde, de 2012 a 2021, todas as vacinas utilizadas para prevenir poliomielite, meningite e sarampo (tríplice viral, tetra viral ou tetravalente, sarampo, poliomielite e meningococo) apresentaram redução na cobertura. Esse cenário se agravou pós-pandemia. Um aprofundamento nessa análise pode ser realizado cruzando com o aumento de registros recentes de casos dessas doenças por região.

Esse caso se agrava ao descobrirmos que há um “apagão” no registro de doses aplicadas. 44% das prefeituras brasileiras não registram ao menos metade das vacinas que aplicam em suas populações. Por que isso acontece? E quais os impactos nas políticas públicas de imunização?

