## Get started

### Points of sale

Before any use, it is necessary to create one or more points of sale in order to be able to link the user accounts. This will be essential when registering orders.

The "Prefix order" will allow you to define a personalized order number for each point of sale if and only if no order number is specified during the POST (external application) in the "customId" field.

An increment and the current year will be added (BO prefix) : `BO-0000207-2022`.

### Accounts and roles

Two types of accounts or `rôles` are available :
 - adminStore : gives access to the client configuration of the tenant (models, materials, measures, options, points of sale, ...)
 - store : user account linked to a point of sale

To connect to APIs, you must use an account with the "store" role that has the necessary rights to take orders. The "adminStore" role is rather reserved for the back-office.

### The connection

From the back office: replace tenantName with the tenant name : `https://sleevessaas.azurewebsites.net/Account/Login?__tenant=tenantName`.

From the APIs: an accesstoken is required for each request. It will have to be added in the header. To obtain it, you must make a request to the address : `https://sleevessaas.azurewebsites.net/connect/token`.

Here is an example of a request in javascript (you must replace the tenantId with the one provided).

```javascript
function Login() {
    var settings = {
        "url": "https://sleevessaas.azurewebsites.net/connect/token",
        "method": "POST",
        "timeout": 0,
        "headers": {
            "__tenant": "tenantId",     // type tenantId
            "Content-Type": "application/x-www-form-urlencoded"
        },
        "data": {
            "Client_Id": "OrdersApp_App",
            "UserName": "storetest",    // type userName
            "Password": ".....",        // type password
            "grant_type": "password"
        }
    };

    $.ajax(settings).done(function (response) {
        LocalStorage.setItem("token", response.access_token);
    });
}
```
