## Spring Boot microservices application, using mono repo approach, to create customers and send product order details via email.
Used:
- Spring Boot
- Kafka
- Postgresql
- MongoDb
- Mail-dev

## Project arhitecture:

![ProductOrders diagram](https://github.com/user-attachments/assets/dbce5220-616d-45c5-a471-88fcc77b98b2)

Project modules are found in the services folder. Entities are defined using Domain-driven design (DDD):

![DDD](https://github.com/user-attachments/assets/2b41ca29-eba4-4b1c-9fe0-bbf0a959c4bb)

The project uses a Eureka Discovery Server, a Configuration Server, an API Gateway, and Keycloak.
The API Gateway is secured using JWT token, using client credentials grant flow type.

Docker images are created using Dockerfile.

## Installation with docker compose:

Use the second commit from the project.

Run maven build for each module:
mvn clean install

Create docker images for each module:
Ex.: docker build . -t <docker_username>/customer:s1

Run:
docker compose up -d

## Kubernetes instalation:

Install keycloak - navigate to "\k8s\helm\product-orders-services".
Run: 
helm install keycloak keycloak

Install revision with all microservices: navigate to "\k8s\helm\environments\dev-env". Run: 

helm dependencies build

Navigate to "\k8s\helm\environments". Run:

helm install product-orders dev-env --debug

Access pgadmin http://localhost:5050/browser/, create "product", "payment" and "order" databases.

## Use

get access_token:
GET http://localhost:80/realms/micro-services/protocol/openid-connect/token

Create customer:
POST http://localhost:8222/api/v1/customers

Raw json body:
```json
{
    "firstname": "John9",
    "lastname": "Doe9",
    "email": "john_doe@yahoo.com",
    "address": {
        "street": "Street Name 1",
        "houseNumber": "123",
        "zipCode": "500001"
    }
}
```

Use customerId in next endpoint:

Create order:
POST http://localhost:8222/api/v1/orders

Raw json body:
```json
{
    "reference": "MS-1111124",
    "amount": 900,
    "paymentMethod": "PAYPAL",
    "customerId": "67dbf24705caea2a33c4226e", -> use customerId from previous step
    "products": [
        {
            "productId": 1,
            "quantity": 1
        },
        {
            "productId": 51,
            "quantity": 1
        },
        {
            "productId": 101,
            "quantity": 1
        }
    ]
}
```

Check email server for notifications:

http://localhost:1080/

![MailDev](https://github.com/user-attachments/assets/b2b54256-291c-4cce-a818-a2052eb85832)


