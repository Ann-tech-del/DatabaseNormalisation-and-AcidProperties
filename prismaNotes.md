# PRISMA

**Prisma**  is a modern and lightweight ORM that provides out-of-the-box support for TypeScript and Node.js.
## WHAT IS ORM

**ORM**  it's a powerful tool that allows us to translate database logic into simple JavaScript code. 

## How to install Prisma
To use Prisma you need to install it .

To install Prisma you need to run the following command in your command line:

```bash
npm install prisma -D

```
## Set up Prisma

After installing prisma we need to set up we run the following command:

```bash
npx prisma init --datasource-provider DATABASE
```
Replace **DATABASE** with the name of database you are using i.e Postresql

## Models

Prisma model represents a table in a database. Think of it as a blueprint for how your data is structured within the database, defining the columns (fields) of that table.

### Key concepts of Prisma Models

1. Data Models: Define the tables and their columns (fields) in your database.

2. Field Types: Specify the data types for each field (e.g., String, Int, Boolean, DateTime).

3. Relations: Describe how models relate to each other using relation fields.

4. Attributes: Add additional metadata to fields and models using attributes like @id, @default, @relation @map.

### Field (Column) are made up of :
- Field Name **(Required)**

-  The data type of the field **(Required)**

-  Attributes **(Optional)** such as @id , @Default.

## CREATE YOUR MODEL

```js

model User {
  id  Int @id @default(uuid())
  email  String @unique
  userName String
  age  Int
  userOrigin String
  followers Int
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
  @@map ("userInfo")
}
```
## MIGRATIONS

Way to manage and apply changes to your database .
Each and everytime you make changes you need to run it.
Run the following commands 

```bash
npx prisma migrate dev --name MIGRATION-NAME
```
Replace the **MIGRATION-NAME** with what your have done.

## CRUD Operations - Create
Creating a single record using the **create ()** method.

```js
import { PrismaClient } from '@prisma/client';
const client = new PrismaClient();

const createProduct = async () => {
    const newUser = await client.User.create({
        data: {
            email: "ann89@gmail.com",
            age:23,
            userName: "Ann254",
            userOrigin: "Kenya",
            followers : 200
        }
    })

    console.log(newUser);
}
```
Create multiple records using **createMany()**

```js

import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const createUsers = async () => {
  const newUser = await client.User.createMany({
    data: [
      {
         email: "Doe24@gmail.com",
            age:33,
            userName: "Doe254",
            userOrigin: "Kenya"
            followers : 107
      },
      {
        email: "juliet23@gmail.com",
            age:18,
            userName: "Juliet255"
            userOrigin : "Tanzania"
            followers : 89
      },
    ],
  });

  console.log(newUser);
};

```
## CRUD Operations - Read

### Get all records using findMany()

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getUsers = async () => {
  const users = await client.user.findMany();
  console.log(users)
};
```

### Find the first record that matches a criteria using findFirst() and apply the filter using where

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getUser = async () => {
  const users = await client.user.findFirst({
    where: { userName: "Juliet255"}
  });
  console.log(users)
};

getUser();
```
### Specify the fields you want returned with the select field
``` js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getUser = async () => {
  const users = await client.user.findFirst({
    where: { userName: "Juliet255" },
    select: {
      userName: true,
      userOrigin: true,
    },
  });
  console.log(users);
};

```
### Applying the AND operator to select only records that match multiple criterias

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getUser = async () => {
  const users = await client.user.findMany({
    where: {
        AND: [
            { age: { gt: 20} },
            { followers : {gt: 100} }
        ]
    },
  });
  console.log(users);
};

```
### Applying OR operator to select records that match any of the Criteria

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getUser = async () => {
  const users = await client.user.findMany({
    where: {
        OR: [
            { follwers: { gt: 100} },
            { age: {gt: 25} }
        ]
    },
  });
  console.log(users);
};

getUser();
```

## CRUD Operations - Update
```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const updatedUser = async () => {
  const updatedUser = await client.user.update({
    where: { id: 3 },
    data: { userName: 'Granian254' }
  })
  console.log(updatedUser);
};

```
### Update multiple records using updateMany()
```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const updatedUser = async () => {
  const updatedUser = await client.user.updateMany({
    where: { followers: { gt: 50 } },
    data: { followers: 150 }
  })
  console.log(updatedUser);
};

```
## CRUD Operations - Delete

### Delete a single record using delete()

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const deleteUser = async () => {
  const deletedUser = await client.user.delete({
    where: { id: 1 },
  })
  console.log(deletedUser);
};

```
### Delete multiple records using deleteMany()

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const deleteUser = async () => {
  const deletedUser = await client.user.deleteMany({
    where: { followers: { gte: 100} },
  })
  console.log(deletedUser);
};

```
## Relationships
- one-to-one relationship (1-1): means that one record in a table is associated to only one record in another table.

- one-to-many relationship (1-n): means that one record in a table can be associated with multiple other records in another table.

- many-to-many relationship (m-n): means that multiple records in one table can be associated to multiple records in another table.

### Example Of 1:1

```js
model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  profile Profile? 
}

model Profile {
  id     Int    @id @default(autoincrement())
  bio    String?
  userId Int    @unique 
  user   User   @relation(fields: [userId], references: [id])
}

```
### Example of 1:n Relationship

```js
model User {
  id  Int @id @default(autoincrement())
  email String @unique
  posts Post[] 
}

model Post {
  id   Int  @id @default(autoincrement())
  title   String
  content   String?
  authorId  Int      
  author  User  @relation(fields: [authorId], references: [id])
}
```

