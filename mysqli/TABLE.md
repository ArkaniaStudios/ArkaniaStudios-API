# Les tables

Les tables sont des ensembles de données qui sont stockées dans une base de données. Une table est composée de lignes et de colonnes. Une ligne représente une entrée unique dans la table, et une colonne représente une caractéristique de cette entrée. Une table est composée de plusieurs colonnes, et chaque colonne est définie par un nom et un type de données.

# Créer une table

Pour créer une table dans une base de données, vous devez utiliser la commande SQL suivante :

```sql
CREATE TABLE IF NOT EXISTS <nom de la talbe>(<nom de la colonne> <type de données>, ...);
```

# Supprimer une table

Pour supprimer une table dans une base de données, vous devez utiliser la commande SQL suivante :

```sql
DROP TABLE <nom de la table>;
```

# Insérer des données dans une table

Pour insérer des données dans une table, vous devez utiliser la commande SQL suivante :

```sql
INSERT INTO <nom de la table>(<nom de la colonne>, ...) VALUES(<valeur>, ...);
```
⚠ Vous devez mettres les valeurs entre ' ' pour les chaînes de caractères.

# Supprimer des données dans une table

Pour supprimer des données dans une table, vous devez utiliser la commande SQL suivante :

```sql
DELETE FROM <nom de la table> WHERE <nom de la colonne> = <valeur>;
```

❓ Le ``WHERE`` est optionnel, mais si vous ne mettez pas le ``WHERE``, alors toutes les données de la table seront supprimées.

# Modifier des données dans une table

Pour modifier des données dans une table, vous devez utiliser la commande SQL suivante :

```sql
UPDATE <nom de la table> SET <nom de la colonne> = <valeur> WHERE <nom de la colonne> = <valeur>;
```

# Sélectionner des données dans une table

Pour sélectionner des données dans une table, vous devez utiliser la commande SQL suivante :

```sql
SELECT <nom de la colonne>, ... FROM <nom de la table> WHERE <nom de la colonne> = <valeur>;
```