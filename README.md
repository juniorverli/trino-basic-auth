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
```mv sample.env .env```

### 2. Configure environment variables

#### TRINO_SHARED_SECRET

To set the TRINO_SHARED_SECRET variable in the .env file, run the following command and copy the output to the variable:

```openssl rand 512 | base64```

#### TRINO_HTTPS_KEYSTORE_KEY

Choose a password for the keystore and set the TRINO_HTTPS_KEYSTORE_KEY variable in the .env file. If desired, you can replace changeit with any other password when creating the keystore.

### 3. Generate the keystore file

To generate the keystore file, run the following command. Replace changeit with the password you chose earlier, if necessary:

```keytool -genkeypair -alias trino -keyalg RSA -keystore ./extra-config/keystore.jks -storepass changeit -validity 365 -keysize 2048```

### 4. Validate the keystore file

To validate the keystore file, run the following command. Replace changeit with the password you chose earlier, if necessary:

```keytool -list -v -keystore ./extra-config/keystore.jks -storepass changeit```

### 5. Running

After configuring the environment variables and generating the keystore file, run the following command to start Trino with Docker Compose:

```docker compose up```
