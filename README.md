📘 SQL – Subselect (Subqueries)

Este repositório contém exemplos práticos de subselects (subqueries) em SQL, retirados de um curso e aplicados aos bancos de dados Sakila e World.

O objetivo é compreender como usar consultas internas para filtrar, relacionar e agregar dados.

🎯 Objetivo

Entender o funcionamento de subselect

Usar in e not in

Aplicar filtros com subqueries

Resolver problemas reais com SQL

Comparar subselect com join

🗂 Bancos utilizados
📀 Sakila

Tabelas:

actor

film

film_actor

🌍 World

Tabelas:

city

country

countrylanguage

🎬 Exemplos – Banco Sakila
Atores que participaram de um filme específico

Busca todos os atores que atuaram no filme film_id = 1.

select * from actor
where actor_id in (
  select actor_id 
  from film_actor 
  where film_id = '1'
);

Filmes feitos por um ator

Filmes da atriz Penelope Guiness (actor_id = 1).

select * from film
where film_id in (
  select film_id 
  from film_actor 
  where actor_id = '1'
);

Filmes do ator com filtro de classificação
select * from film
where film_id in (
  select film_id 
  from film_actor 
  where actor_id = '1'
)
and rating = 'PG';

Filmes que o ator não fez
select * from film
where film_id not in (
  select film_id 
  from film_actor 
  where actor_id = '1'
);

🌎 Exemplos – Banco World
Quantidade de cidades por país
select a.country_id, a.country,
(
  select count(*) 
  from city b 
  where a.country_id = b.country_id
) as qtda
from country a;

População total dos países que falam espanhol (subselect)
select a.countrycode,
       sum(a.population) as total_pop,
       (
         select name 
         from country b 
         where a.countrycode = b.code
       ) as pais
from city a
where a.countrycode in (
  select countrycode 
  from countrylanguage 
  where language = 'Spanish'
)
group by a.countrycode;

Mesma solução usando JOIN
select a.countrycode,
       sum(a.population) as total_pop,
       b.name
from city a
inner join country b 
  on a.countrycode = b.code
inner join countrylanguage c
  on a.countrycode = c.countrycode
where language = 'Spanish'
group by a.countrycode, b.name;

🧠 Conceitos abordados

Subselect simples

Subselect correlacionado

in e not in

Agregações (count, sum)

Comparação entre subselect e join
