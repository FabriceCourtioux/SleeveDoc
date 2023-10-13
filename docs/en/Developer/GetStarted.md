## Get started

To connect to APIs, you must use an account with the "store" role that has the necessary rights to take orders.

### The connection

An access token is required for each request. It will have to be added in the header. To obtain it, you must make a request to the address : `https://sleevessaas.azurewebsites.net/connect/token`.

This access token can be reused for the entire lifespan of the current session. There is no need to request a new one for each request.

> Note that for security reasons, it is not recommended to store it in the localStorage without being encrypted. Other backup methods may be used as appropriate.

Here is an example of a JavaScript request (you must replace the tenantId with the one provided).

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

### User information

The current user's information will be necessary for certain actions such as taking an order.

Here is an example of a JavaScript request that will allow you to obtain them:

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
And the result of the query:
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

