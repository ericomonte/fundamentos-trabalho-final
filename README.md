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

Após isso, fizemos as análises dentro do período de anos selecionados (2012 a 2021). [A tabela pode ser vista aqui](https://docs.google.com/spreadsheets/d/1VOvtFY8p7ln40Ax7lEXRW7pQE-I20--fFeGsriiY9HU/edit#gid=1618821006).


2. Em seguida perguntamos como isso estava representado territorialmente: ***Qual o ranking dos estados com menor cobertura para as doenças antes erradicadas, como sarampo, pólio, rubéola e difteria?***

**Vacinação dtp**
Os dados mostram que o último ano que teve essa vacinação foi em 2016, na cidade de Balneário Rincão em Santa Catarina. Lá, 192 doses foram aplicadas, uma cobertura de 95%. Em 2015, apenas duas cidades aparecem com aplicação da vacina: Pescaria Brava (SC) e Mojuí dos Campos (PA).
O ano mais recente com mais aplicações da dose foi em 2002. A maior aplicação foi em Lagoa Santa (GO) com cobertura de 214%.

**Vacinação poli**
Os dados mostram que o último ano que teve essa vacinação foi em 2016 e, novamente, na cidade de Balneário Rincão em Santa Catarina. Na época, 167 doses foram aplicadas, uma cobertura de 83%. O ano mais recente com mais aplicações da dose foi em 2002. A maior aplicação foi em Lagoa Santa (GO) com cobertura de 628%.

**Vacinação sarampo**
Os dados mostram que o último ano que teve vacinação de sarampo foi em 2002. A cidade com maior cobertura foi Palmeira (SC) com 314% de cobertura.

**Vacinação tríplice viral d1 e d2**
Balneário Rincão aparece novamente como a única cidade de 2016 com vacinação de tríplice viral d1. No ano citado, 61 doses foram aplicadas, uma cobertura de 30%.
A mesma cidade em 2016 foi a única que aplicou a df: 169 vacinas, uma cobertura de 84%.
