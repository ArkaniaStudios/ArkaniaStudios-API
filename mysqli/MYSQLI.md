# Le mysqli

Le mysql est un langage permettant de stocker des données de 
façon simple st sécurisé. Quand vous faites une requête mysql,
la requête s'ajoute à une file de requête déjà en cours. Tant 
que la requête ne reçoit pas de réponse, alors elle attend.
C'est donc pour cela que nous utilisons les AsyncTask afin de ne
pas pénaliser le thread principal et donc le faire lag.

# Comment se connecter à une database MySQLi ?

Pour vous connectez à une database mysqli, vous devrez utiliser 
un fontion php nommé ``mysqli`` :

````php
new \mysqli('', '', '', '');
````

Le premier argument est l'ip de la database. Le deuxième argument 
est le nom de l'utilisateur, le 3ᵉ argument est le mot de passe
et le dernier argument est le nom de la database.