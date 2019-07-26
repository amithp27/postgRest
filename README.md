# postgRest
PostgreSQL with REST Endpoints

Intro:
    Using PostgreSQL and PostgREST to create a Product with CRUD operations and Full Text Search for columns. Also includes other queries from REST.


SETUP:
    PostgreSQL
    PostgREST on top of it for REST Endpoints
    Postman for REST calls

CREATE TABLES:
    create table Product1 (
    id serial primary key,
    vendor_uuid text,
    title text unique not null,
    description text,
    price integer not null,
    image text,
    dietry_flags text,
    views integer
    );

    create table vendor (
    id text unique primary key,
    name text not null,
    mob text unique
    );

    insert into vendor (id, name, mob) values ('vendor01', 'Mahesha', '90787865778');
    insert into Product1 (vendor_uuid, title, description, price, image, dietry_flags, views) values ('vendor01', 'Milk 1Ltr', 'Lactosefree', 10, 'images/Milk_1ltr.jpg','Lactose-Free',1);
    insert into Product1 (vendor_uuid, title, description, price, image, dietry_flags, views) values ('vendor01', 'Almonds 500g', 'Vegan product', 25, 'images/Almonds_500g.jpg','Vegan',1);
    ...


REST OUPUTS:

READS:::::
    GET http://localhost:3000/product1?select=*&id=eq.1
    [
        {
            "id": 1,
            "vendor_uuid": "vendor01",
            "title": "Milk 1Ltr",
            "description": "Lactosefree",
            "price": 10,
            "image": "images/Milk_1ltr.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        }
    ]
    GET http://localhost:3000/product1?select=*&id=eq.4
    [
        {
            "id": 4,
            "vendor_uuid": "vendor03",
            "title": "Avocado 1kg",
            "description": "Vegan product",
            "price": 35,
            "image": "images/Avocado_1kg.jpg",
            "dietry_flags": "Vegan",
            "views": 1
        }
    ]

CREATE NEW ::::
    POST: http://localhost:3000/product1
    {
            "vendor_uuid": "vendor05",
            "title": "Milk 500ml",
            "description": "Lactosefree",
            "price": 6,
            "image": "images/Milk_500ml.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
    }
    GET http://localhost:3000/product1?select=*&id=eq.6
    [
        {
            "id": 6,
            "vendor_uuid": "vendor05",
            "title": "Milk 500ml",
            "description": "Lactosefree",
            "price": 6,
            "image": "images/Milk_500ml.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        }
    ]

MODIFY ::::
    PATCH http://localhost:3000/product1?select=*&id=eq.6
    {
            "title": "Milk 500ML"
    }
    GET http://localhost:3000/product1?select=*&id=eq.6
    [
        {
            "id": 6,
            "vendor_uuid": "vendor05",
            "title": "Milk 500ML",
            "description": "Lactosefree",
            "price": 6,
            "image": "images/Milk_500ml.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        }
    ]

DELETE::::::
    DELETE http://localhost:3000/product1?select=*&id=eq.3
    GET http://localhost:3000/product1?select=*&id=eq.3
    []



PRICE GREATER THAN 20:::::::::
    GET http://localhost:3000/product1?price=gt.20
    [
        {
            "id": 2,
            "vendor_uuid": "vendor01",
            "title": "Almonds 500g",
            "description": "Vegan product",
            "price": 25,
            "image": "images/Almonds_500g.jpg",
            "dietry_flags": "Vegan",
            "views": 1
        },
        {
            "id": 4,
            "vendor_uuid": "vendor03",
            "title": "Avocado 1kg",
            "description": "Vegan product",
            "price": 35,
            "image": "images/Avocado_1kg.jpg",
            "dietry_flags": "Vegan",
            "views": 1
        }
    ]




SEARCH FOR PRODUCTS WIH TITLE HAS 'MILK'::::::::
    GET http://localhost:3000/product1?title=fts.milk
    [
        {
            "id": 1,
            "vendor_uuid": "vendor01",
            "title": "Milk 1Ltr",
            "description": "Lactosefree",
            "price": 10,
            "image": "images/Milk_1ltr.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        },
        {
            "id": 6,
            "vendor_uuid": "vendor05",
            "title": "Milk 500ML",
            "description": "Lactosefree",
            "price": 6,
            "image": "images/Milk_500ml.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        }
    ]



LACTOSE-FREE PRODUCTS:::
    GET http://localhost:3000/product1?dietry_flags=fts.Lactose-Free
    [
        {
            "id": 1,
            "vendor_uuid": "vendor01",
            "title": "Milk 1Ltr",
            "description": "Lactosefree",
            "price": 10,
            "image": "images/Milk_1ltr.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        },
        {
            "id": 6,
            "vendor_uuid": "vendor05",
            "title": "Milk 500ML",
            "description": "Lactosefree",
            "price": 6,
            "image": "images/Milk_500ml.jpg",
            "dietry_flags": "Lactose-Free",
            "views": 1
        }
    ]



AWS Auto deployment: 
https://www.freecodecamp.org/news/automated-deployment-in-aws-5aadc2e708a9/
