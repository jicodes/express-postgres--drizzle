### install packages

```sh
npm install
```

### run services in docker container

```sh
docker compose up --build
```

1. Building Images: The --build flag forces Docker Compose to build images for services that have a build context specified in the docker-compose.yml file before starting the containers. If your server service has a `build: .` directive, Docker Compose will build an image using the Dockerfile in the current directory. This process includes installing dependencies, copying source files, and any other build steps defined in your Dockerfile.

2. Starting Services: Docker Compose starts the services defined in the docker-compose.yml file. This includes the db service (your PostgreSQL database) and the server service (your application). If there are dependencies specified with depends_on, Docker Compose ensures that dependent services are started in the correct order. In your case, the db service will start before the server service because the server depends on db.

3. Environment Variables: Docker Compose sets up environment variables for your services as defined under the environment section in the docker-compose.yml file. For the server service, this includes DB_HOST, DB_NAME, DB_USER, and DB_PASSWORD, among others. These variables are made available to your application, allowing it to configure its database connection dynamically.

4. Database Connection: When your server service starts, the TypeScript code you've shown attempts to establish a connection to the database using the Pool from pg (assuming Pool is imported from the pg module). It uses environment variables for the database connection details:

- port: Hardcoded as 5432, which is the default PostgreSQL port.
- host: Set to process.env.DB_HOST, which would be db as defined in your Docker Compose environment variables, pointing to the container running PostgreSQL.
- database: The name of the database, taken from process.env.DB_NAME.
- user: The database user, from process.env.DB_USER.
  password: The database password, from process.env.DB_PASSWORD.

5. Database Operations: Once the connection is established, your application can perform database operations using the pool. The export const db line suggests that you're using a library (possibly a custom one or an ORM) named drizzle to interact with the database, passing the pool and a schema object to it.

If any of the required environment variables (DB_HOST, DB_NAME, DB_USER, DB_PASSWORD) are missing or incorrect, or if the db service is not ready when the server service attempts to connect, your application might fail to start or throw an error. Docker Compose's logs will show output from both the db and server services, which can help diagnose any issues with the startup or database connection process.

Generate SQL

```sh
npm run generate
```

Run migration

```sh
npm run migrate
```

Open UI to connected db

```sh
npm run studio
```
