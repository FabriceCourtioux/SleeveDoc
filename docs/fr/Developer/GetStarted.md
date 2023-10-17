## Démarrer

Pour la connexion aux API, il faut utiliser un compte avec le rôle « store » ayant les droits nécessaires pour la prise de commande.

### La connexion

Un jeton d'accès est nécessaire à chaque requête. Il faudra l’ajouter dans le header. Pour l’obtenir, il faut faire une requête à l’adresse : `https://sleevessaas.azurewebsites.net/connect/token`.

Ce jeton d'accès pourra être réutilisé pour toute la durée de vie de la session en cours. Il est inutile d'en redemander un nouveau à chaque requête.
<br/><br />
> A noter que pour des raisons de sécurité, il est déconseillé de le stocker  dans le localStorage sans être crypté. D'autres méthodes de sauvegarde peuvent être utilisées selon le cas.

Voici un exemple de requête en JavaScript (il faut remplacer le tenantId par celui fourni).

```javascript
function Login() {
    var settings = {
        url: "https://sleevessaas.azurewebsites.net/connect/token",
        method: "POST",
        timeout: 0,
        headers: {
            "__tenant": "tenantId",     // saisir le tenantId
            "Content-Type": "application/x-www-form-urlencoded"
        },
        data: {
            "Client_Id": "OrdersApp_App",
            "UserName": "storetest",    // saisir le userName
            "Password": ".....",        // saisir le mot de passe
            "grant_type": "password"
        }
    };

    $.ajax(settings).done(function (response) {
        LocalStorage.setItem("token", response.access_token);
    });
}
```

### Les informations utilisateur

Les informations de l'utilisateur courant seront nécessaires pour certaines actions comme la prise de commmande.

Voici un exemple de requête JavaScript qui va permettre de les obtenir :

```javascript
function UserInfos() {
    $.ajax({
        url: "https://sleevessaas.azurewebsites.net/api/app/vendor/getInfos",
        method: "GET",
        beforeSend: function (request) {
            request.setRequestHeader("Authorization", 'Bearer ' + LocalStorage.getItem("token"));
        },
        success: function (data) {
            alert(JSON.stringify(data));
        }
    });
}
```
Et le résultat de la requête :
```json
{
  "userId": "b8121447-27b5-c4ba-3951-39fcbb7ae7d5",
  "userName": "userStore",
  "name": "User name",
  "email": "xxx@xxx.com",
  "storeId": "0d440829-7c60-4fa8-8951-6a98529452a7",
  "jacket": true,
  "trouser": true,
  "vest": true,
  "shirt": true,
  "polo": true,
  "tee": true,
  "dress": true,
  "accessory": false
}
```
