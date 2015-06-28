# REST and idempotency principle sample


# API documentation

## Users

### Data types

#### User

**application/json**

```javascript
{
  "id": 5000,
  "name": "Pepe",
  "age": 54
}
 ```
 
**application/xml**

```xml
<user>
  <id>5000</id>
  <name>5000</Pepe>
  <age>54</age>
</user>
 ```
 
| Field        | Type           | Required | Description  |
| ------------- |:-------------:|:-------------:| -----|
| id      | int | true | User identifier. E.g: 3009 |
| name     | string | true | User name. E.g: Manuel |
| age     | int | true | User age. E.g: 62 |
 
### Resources

#### GET Users

URL: **/api/users**

Method: **GET**

Accept: **application/json**, **application/xml**

#### GET User

URL: **/api/users/{userId}**

Method: **GET**

Accept: **application/json**, **application/xml**

Path param: **userId** = user identifier. E.g: /api/users/5001 

**Sample**
```java
Request:

URL: http://MY_SERVER:SERVER_PORT/APP_CONTEXT/api/users/5001
Method: GET
Headers:
    Accept: application/json
-------------------------------------------------------
Response:

Status: 200 OK
Headers:
     Content-Type: application/json
Body:
   {
     "id": 5001,
     "name": "Luis",
     "age": 23
   }
     
-------------------------------------------------------

* APP_CONTEXT: Application context. Default: rest-idempotency.
* MY_SERVER: Server address. Default: localhost.
* SERVER_PORT: Port. Default: 8080 (Tomcat).

```

#### Add User

URL: **/api/users**

Method: **POST**

Accept: **application/json**, **application/xml**

Content-Type: **application/json**, **application/xml**

Body: **user** to create (id is not required. See user data type) 

**Sample**
```java
Request:

URL: http://MY_SERVER:SERVER_PORT/APP_CONTEXT/api/users
Method: POST
Headers:
    Accept: application/json
    Content-Type: application/json
Body:
   {
     "name": "Manuel",
     "age": 17
   }    
-------------------------------------------------------
Response:

Status: 201 CREATED
Headers:
     Location: /APP_CONTEXT/api/users/5002
     Content-Type: application/json
Body:
   {
     "id": 5002,
     "name": "Manuel",
     "age": 17
   }
-------------------------------------------------------

* APP_CONTEXT: Application context. Default: rest-idempotency.
* MY_SERVER: Server address. Default: localhost.
* SERVER_PORT: Port. Default: 8080 (Tomcat).

```

#### Update User

URL: **/api/users/{userId}**

Method: **PUT**

Accept: **application/json**, **application/xml**

Content-Type: **application/json**, **application/xml**

Path param: **userId** = user identifier. E.g: /api/users/5001 

Body: **user** to update (id is not required in body. See user data type) 

## Orders

### Data types

#### Order

**application/json**

```javascript
{
  "id": "999919_bd64fe278c6192c821a80ce6038effe8",
  "price": 6.98,
  "vouncher": "KKFEV3",
  "status" : 2
}
 ```
 
**application/xml**

```xml
<order>
  <id>999919_bd64fe278c6192c821a80ce6038effe8</id>
  <price>6.98</price>
  <vouncher>KKFEV3</vouncher>
  <status>2</status>
</order>
 ```
 
| Field        | Type           | Required | Description  |
| ------------- |:-------------:|:-------------:| -----|
| id      | string | true | Order identifier. E.g: 999919_bd64fe278c6192c821a80ce6038effe8 |
| price     | double | true | Order price. E.g: 83.24 |
| vouncher     | int | true | A vouncher code associated to the order. E.g: VC705R |
| status     | int | true | Order status. It must be: 1 (CREATED), 2 (FINISHED) or 3 (CANCELED). E.g: 2 |

### Resources

#### GET Orders

URL: **/api/orders**

Method: **GET**

Accept: **application/json**, **application/xml**
 
#### GET Orders

URL: **/api/orders/{orderId}**

Method: **GET**

Accept: **application/json**, **application/xml**

Path param: **orderId** = order identifier. E.g: /api/users/999919_bd64fe278c6192c821a80ce6038effe8

#### CREATE "Virtual" Order

URL: **/api/orders**

Method: **POST**

Accept: **application/json**, **application/xml**

**Sample**
```java
Request:

URL: http://MY_SERVER:SERVER_PORT/APP_CONTEXT/api/orders
Method: POST
Headers:
    Accept: application/json
-------------------------------------------------------
Response:

Status: 204 NO CONTENT
Headers:
     Location: /APP_CONTEXT/api/orders/17000014_642d69b37201b6c79c2f19d1cc09db10
-------------------------------------------------------

* APP_CONTEXT: Application context. Default: rest-idempotency.
* MY_SERVER: Server address. Default: localhost.
* SERVER_PORT: Port. Default: 8080 (Tomcat).

```

#### UPDATE/CREATE Order

URL: **/api/orders/{orderId}**

Method: **PUT**

Accept: **application/json**, **application/xml**

Content-Type: **application/json**, **application/xml**

Path param: **orderId** = order identifier. E.g: /api/users/999919_bd64fe278c6192c821a80ce6038effe8

Body: **order** to create/update (id is not required in body. See order data type) 

**Sample**
```java
Request:

URL: http://MY_SERVER:SERVER_PORT/APP_CONTEXT/api/orders/999919_bd64fe278c6192c821a80ce6038effe8
Method: PUT
Headers:
    Content-Type: application/json
    Accept: application/json
Body:
    {
       "price" : 98.05,
       "vouncher" : "AAB09Z",
       "status" : 1
    }
-------------------------------------------------------
Response:

Status: 201 CREATED
Headers:
     Content-Type: application/json
Body: 
    {
       "id" : "999919_bd64fe278c6192c821a80ce6038effe8",
       "price" : 98.05,
       "vouncher" : "AAB09Z",
       "status" : 1
    }     
-------------------------------------------------------

* APP_CONTEXT: Application context. Default: rest-idempotency.
* MY_SERVER: Server address. Default: localhost.
* SERVER_PORT: Port. Default: 8080 (Tomcat).

```
