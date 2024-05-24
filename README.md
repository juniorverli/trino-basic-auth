# Basic Authentication with Trino

This repository provides an example of setting up basic authentication (username and password) for Trino, using environment variables and Docker Compose.


## Prerequisites

- Docker
- Docker Compose
- OpenSSL
- Keytool (included in the JDK)


## Setup


### 1. Rename the environment file

Rename the `sample.env` file to `.env`:

```
mv sample.env .env
```


### 2. Configure environment variables

#### 2.1 TRINO_SHARED_SECRET

To set the TRINO_SHARED_SECRET variable in the .env file, run the following command and copy the output to the variable:

```
openssl rand 512 | base64
```

#### 2.2 TRINO_HTTPS_KEYSTORE_KEY

Choose a password for the keystore and set the TRINO_HTTPS_KEYSTORE_KEY variable in the .env file. If desired, you can replace changeit with any other password when creating the keystore.


### 3. The keystore file

#### 3.1 Generate the keystore file
To generate the keystore file, run the following command. Replace changeit with the password you chose earlier, if necessary:

```
keytool -genkeypair -alias trino -keyalg RSA -keystore ./extra-config/keystore.jks -storepass changeit -validity 365 -keysize 2048
```

#### 3.2 Validate the keystore file

To validate the keystore file, run the following command. Replace changeit with the password you chose earlier, if necessary:

```
keytool -list -v -keystore ./extra-config/keystore.jks -storepass changeit
```

### 4. Generate the password file

To create the password file password.db, execute the following commands. You can choose the password for the admin user after executing the commands:

```
touch ./extra-config/password.db
htpasswd -B -C 10 ./extra-config/password.db admin
```

### 5. Running

#### 5.1 Running Docker

After configuring the environment variables, generating the password file and generating the keystore file, run the following command to start Trino with Docker Compose:

```
docker compose up
```

#### 5.2 Using Trino CLI

For [Trino CLI](https://trino.io/docs/current/client/cli.html), use the following command:

```
trino https://localhost:8443?SSL=true&SSLVerification=NONE --user admin --password
```

After executing this command, use the chosen password when prompted.

#### 5.3 DBeaver

For DBeaver, use the following JDBC URL: jdbc:trino://localhost:8443?SSL=true&SSLVerification=NONE. Enter the username "admin" and the chosen password.

#### 5.4 Web UI - Query history

Additionally, if accessing via a web browser at https://localhost:8443, using the username "admin" and the chosen password will grant access to the query history.
