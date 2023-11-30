Start backend:

```bash
npx @medusajs/medusa-cli develop   
```

Start frontend:

```bash
npm run dev
```

https://dev.to/yinks/how-to-setup-a-medusa-server-locally-44e3

Postgres setup:

```bash
# create a new user
CREATE USER medusa_admin WITH PASSWORD 'medusa_admin_password';

# create a new database and the newly created user as the owner
CREATE DATABASE medusa_db OWNER medusa_admin;

# grant all privileges on medusa_db to the medusa_admin user
GRANT ALL PRIVILEGES ON DATABASE medusa_db TO medusa_admin;

# exit the postgres console
quit
```

Env: 

```env
DATABASE_URL=postgres://medusa_admin:medusa_admin_password@localhost:5432/medusa_db
```

Medusa-config.js:

```js
module.exports = {
  projectConfig: {
        //...other configurations
    database_url: process.env.DATABASE_URL,
    database_type: "postgres",
  },
};
```

```bash
brew services start redis
```

Make sure Redis server is started.

The Redis URL is default so it's already in ENV. I just uncommented a line in medusa-config.js for it to work. 

Admin Details:

admin@medusa-test.com
supersecret

I'm using MinIO to host files:
https://docs.medusajs.com/plugins/file-service/minio

MinIO has a default address port of 9000, which clashes with Medusa, which also uses port 9000. You should change MinIO’s port from 9000 to something else e.g 9001.

Start Minio Server:

```sh
minio server ~/data --address ":9001"
```

**minioadmin**
**minioadmin**
user and password

fVgB6FcrS0NZvbLlzByz - access key
PkwAIRUwYOjyaiC01tftnDOOw8LKMxmMDC9FmGn5 - secret key

For the shitty api use cookies cause I can't figure the other way with api tokens out.

```bash
curl -L -X POST 'http://localhost:9000/admin/price-lists' \
-H 'Cookie: connect.sid=s%3A2PPMu0lEpEnFwYbd68qcOVJGg9zANOld.0hgO40yppf6CrS4ZIx0GWIh0GxRqpYvb4jTN8W5NzI0' \
-H 'Content-Type: application/json' \
--data-raw '{
    "name": "New Price List",
    "description": "A new price list",
    "type": "sale",
        "status": "active",
    "prices": [
        {
            "amount": 1500,
            "variant_id": "<VARIANT_ID>",
            "max_quantity": 3,
            "currency_code": "eur"
        },
        {
            "amount": 1000,
            "variant_id": "<VARIANT_ID>",
            "currency_code": "eur",
            "min_quantity": 4
        }
    ]
}'
```

# Change Product Image Sizes

In the product page:

```
/Users/arazabedi/code/hotel-linen-storefront/src/modules/products/components/image-gallary/index.tsx
```

```jsx
className="relative aspect-[4/3] w-full"
```

In lists such as "you might also like" section:

```
/Users/arazabedi/code/hotel-linen-storefront/src/modules/products/components/thumbnail/index.tsx
```

```jsx
className={clsx("relative aspect-[4/3]", {
```

# Meilisearch Server

Start Meilisearch Server:

```sh
meilisearch --master-key gGTMEiWkEZK0y6jNjuxRx2TY0ORDONCnQ3AE50txNFE
```

