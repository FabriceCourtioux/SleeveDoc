## Démarrer

### Les points de vente

Avant toute utilisation, il faut créer un ou plusieurs points de vente afin de pouvoir y lier les comptes utilisateurs. Cela sera indispensable lors de l’enregistrement des commandes.

Le « Prefix commande » permettra de définir un numéro de commande personnalisé pour chaque point de vente si et seulement si aucun numéro de commande n’est précisé lors du POST (application externe) dans le champ « customId ».

Un incrément et l’année en cours seront ajoutés (préfix BO) : `BO-0000207-2022`.

### Les comptes et rôles

Deux types de compte ou `rôles` sont disponibles :
 - adminStore : donne accès à la configuration client du tenant (modèles, matières, mesures, options, points de vente, …)
 - store : compte utilisateur lié à un point de vente

Pour la connexion aux API, il faut 
utiliser un compte avec le rôle « store » ayant les droits nécessaires pour la prise de commande. Le rôle « adminStore » est quant à lui réservé au back-office.

### La connexion

Depuis le back-office : remplacer tenantName par le nom du tenant : `https://sleevessaas.azurewebsites.net/Account/Login?__tenant=tenantName`.

Depuis les API : un accesstoken est nécessaire à chaque requête. Il faudra l’ajouter dans le header. Pour l’obtenir il faut faire une requête à l’adresse : `https://sleevessaas.azurewebsites.net/connect/token`.

Voici un exemple de requête en javascript (il faut remplacer le tenantId par celui fourni).

```javascript
function Login() {
    var settings = {
        "url": "https://sleevessaas.azurewebsites.net/connect/token",
        "method": "POST",
        "timeout": 0,
        "headers": {
            "__tenant": "tenantId",     // saisir le tenantId
            "Content-Type": "application/x-www-form-urlencoded"
        },
        "data": {
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
