# Elastifeed collector system

**!!! DEVELOPMENT !!!** deployment of the central collector system

## Installation
First copy the example `.env.example` file and fill the database credentials
```bash
cp .env.example .env
vim .env
```
Then execute:
```bash
composer install
php artisan key:generate
php artisan migrate
php artisan passport:install
```

## Run a PHP development Server
```bash
php artisan cache:clear
php artisan serve
```

## Endpoints

### `/api/v1/fetch/feeds`
Requires a **GET**-Request \
Returns a List of all currently registered RSS-Feeds with the individual subscribed users.

### `/api/v1/register`
Allows for user registration. Requires a `POST` request containing the following fields:
- **email** \
    unique, max-length 255
- **password** \
    between 5 and 32 chars
- **name** \
    max-length 255
    
### `/api/v1/login`
Endpoint to log into the system.
A successful request will return a [`JWT`](https://tools.ietf.org/html/rfc7519) which grants access to all other endpoints.
Requires a `POST` request containing the following fields:
- email
- password

## User-Endpoints
All User-Endpoints need to be accessed with a valid `JWT` generated by the `/api/v1/login` endpoint.
The token should be send in the `Authorization` header, using the Bearer schema.

`Authorization: Bearer {token}`

### `/api/v1/me`
Requires a **GET**-Request \
Returns the current authenticated user object. 

### `/api/v1/feeds`
Requires a **GET**-Request \
Returns the RSS-Feeds of the current authenticated user.

### `/api/v1/feeds`
Adds a new RSS-Feed to the current authenticated user.
Requires a **POST** request containing the following field:
- link \
  Link to the `.rss`resource

### `/api/v1/feeds/{id}`
Requires a  **DELETE**-Request and removes an existing RSS-Feed from the current authenticated user.

### `/api/v1/categories`
Requires a  **GET**-Request and returns all categories defined by the current authenticated user.

### `/api/v1/categories`
Adds a new category to the current authenticated user
Requires a **POST**-Request containing the following fields:
- `name` \
  Category title string
 - `meta` (optional) \
  JSON object containing the categories meta-data
  
### `/api/v1/categories/{id}` *delete*
Removes an existing category with the given `{id}` from the current authenticated users.

### `/api/v1/page`
Pushes a new page into the ElastiFeed System.
Requires a **POST**-Request containing the following fields:
- `url` \
  Link as a string to push into the system.
- `categories` (optional) \
  array of strings containing the pages category **names**
- `tags` (optional) \
   array of strings containing the pages tags