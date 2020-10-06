# Exercices-database-partie-2
2eme salve d'exo avec sql


# Exercices database partie 2

Le rendu de ces exercices se fera dans un repository accessible depuis github qui contiendra un unique fichier _README.md_.
Ce fichier contiendra toutes les commandes et les outputs(si demandés dans l'énoncé) de ces commandes classées par exercices, comme demandé dans les énoncés.

**LE FICHIER README.md devra être lisible, un chapitre par exercice, les commandes SQL devront être mises en valeur dans un bloc de code, ainsi que les outputs.**

**Ne mettez les outputs des commandes que si ils sont demandés dans l'énoncé**

## 1

Télécharger la base de donnée: https://sp.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip  
Dézipper le fichier _dvdrental.zip_ et installer la base de donnée _dvdrental.tar_ avec les commandes suivantes:

```zsh
% psql -d postgres -U db_user
```

```sql
postgres=> CREATE DATABASE dvdrental;
postgres=> \quit
```

```zsh
% pg_restore -U db_user -d dvdrental ./dvdrental.tar
% psql -d dvbrental -U db_user
```

Vous êtes maintenant connectés à la base de donnée `dvdrental`

Vous pouvez récupérer un modele visuel de cette base de donnée sur:
https://www.postgresqltutorial.com/postgresql-sample-database/  
C'est très utile si vous voulez comprendre que représente cette base de données.

la commande `\dt+ NOM_DE_LA_TABLE` vous donne la liste des tables.  
la commande `\dt+ NOM_DE_LA_TABLE` vous donne le nom des colonnes de d'une table.
la commande `\d NOM_DE_LA_TABLE` vous affiche le nom des colonnes ainsi que les types associés à chaque colonnes.

```sql

psql -d dvdrental -U db_user
Password for user db_user:
psql (12.4)
Type "help" for help.

dvdrental=> \dt
            List of relations
 Schema |     Name      | Type  |  Owner
--------+---------------+-------+---------
 public | actor         | table | db_user
 public | address       | table | db_user
 public | category      | table | db_user
 public | city          | table | db_user
 public | country       | table | db_user
 public | customer      | table | db_user
 public | film          | table | db_user
 public | film_actor    | table | db_user
 public | film_category | table | db_user
 public | inventory     | table | db_user
 public | language      | table | db_user
 public | payment       | table | db_user
 public | rental        | table | db_user
 public | staff         | table | db_user
 public | store         | table | db_user
(15 rows)

```

## 2

Ecrivez une requête SQL qui affiche tous les titres et descriptions des films dont la description contient le mot `Amazing`.
```sql

dvdrental=> SELECT title, description FROM film WHERE description LIKE '%Amazing%';

```
Je t'épargne les 48 entrées commençant par 'A amazing...'

## 3

Ecrivez une requête SQL qui récupère tous les paiements supérieurs à 10.
Il faudra récupérer l'id, le prénom, le nom du client ainsi que le montant et la date du paiement.

```txt
customer_id | first_name |  last_name   | amount |        payment_date
```
```sql
dvdrental=> SELECT customer.customer_id, customer.first_name, customer.last_name, payment.amount, payment.payment_date FROM customer JOIN payment ON customer.customer_id = payment.customer_id WHERE amount > 10 ;
```
## 3bis

Ecrivez une requête SQL qui affiche le chiffre d'affaire gagné par le video club depuis son ouverture.
```sql
dvdrental=> SELECT SUM(amount) FROM payment;
   sum
----------
 61312.04
(1 row)
```
## 4

Ecrivez une requête SQL qui affiche le titre de tous les films dont la langue est l'anglais et dont la durée est supérieure à 120 minutes.
```sql
dvdrental=> SELECT name, language_id FROM language;
         name         | language_id
----------------------+-------------
 English              |           1
 Italian              |           2
 Japanese             |           3
 Mandarin             |           4
 French               |           5
 German               |           6
(6 rows)

dvdrental=> SELECT title FROM film WHERE language_id = 1 AND length > 120 ;
```
```sql
dvdrental=> SELECT film.title FROM film INNER JOIN language ON film.language_id = language.language_id WHERE language.name = 'English' AND film.length > 120 ;
```
## 5

Ecrivez une requête SQL qui affiche le TOP 10 des clients qui ont fait le plus d'achat dans ce video club.
Il faudra récupérer leur id, prénom, nom, email.
Il vous faudra utiliser les requêtes auxiliaires avec `WITH` pour cette exercice.

## 6

Récupérer les mêmes informations que l'exercice précédent, mais ajouter avec un `JOIN` le montant total des achats pour chacun du TOP 10 des clients.

## 7

Faîtes preuves d'imagination et essayez de créer une requête très complexes que vous expliquerez. Cette requête devra utiliser les concepts que nous avons étudié en cours et vu dans les exercices précédents.
