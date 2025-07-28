# Certificate Generator

## Overview

`certgen.sh` is a Bash script designed to automate the creation of a root Certificate Authority (CA), private keys, X.509 certificates, and Diffie-Hellman parameters for multiple services such as **nginx**, **redis**, **rabbitmq**, and **postgres**. All generated cryptographic materials are stored in the `certs` directory.

---

## Features

- Generates a **4096-bit RSA root CA** key and a self-signed certificate valid for 10 years.
- Creates **2048-bit RSA private keys** and signed certificates for each specified service.
- Produces **2048-bit Diffie-Hellman parameters** to enhance TLS security.
- Uses an external OpenSSL configuration file for certificate extensions.
- Loads subject Distinguished Name (DN) parameters from an external configuration file.
- Sets secure file permissions:
  - Private keys: `600`
  - Certificates and DH params: `644`

---

## Prerequisites

- **OpenSSL** must be installed and accessible in your system's `PATH`.
- The following configuration files must exist relative to the script:
  - `config/openssl.cnf` — OpenSSL extensions config
  - `config/subject.conf` — Subject DN parameters
  - `usage.txt` — Usage instructions

---

## Usage

```
./certgen.sh [options]
```

### Options

- `-h`, `--help`  
  Display the help message and exit.

---

## File Structure

- `certs/` — Output directory for all generated keys, certificates, and DH parameters.
- `config/` — Contains configuration files:
  - `openssl.cnf`
  - `subject.conf`
- `usage.txt`

---

## How It Works

1. **Root CA Generation**  
   Creates the root CA private key (`ca.key`) and self-signed certificate (`ca.crt`) if they do not already exist.

2. **Service Keys and Certificates**  
   For each service (`nginx`, `postgres`, `rabbitmq`, `redis`), the script:
   - Generates a private RSA key.
   - Creates a certificate signed by the root CA with appropriate extensions.

3. **Diffie-Hellman Parameters**  
   Generates DH parameters (`redis.dh`) for secure key exchange.

4. **Permissions**  
   Ensures private keys are accessible only by the owner and certificates are world-readable.

---

## Example

After running the script:

```
./certgen.sh
```

You will find the following files in the `certs` directory:

- `ca.key` — Root CA private key
- `ca.crt` — Root CA certificate
- `server.key` & `server.crt` — Nginx key and certificate
- `client.key` & `client.crt` — Client key and certificate
- `postgres.key` & `postgres.crt` — Postgres key and certificate
- `rabbitmq.key` & `rabbitmq.crt` — RabbitMQ key and certificate
- `redis.key` & `redis.crt` — Redis key and certificate
- `redis.dh` — Diffie-Hellman parameters for Redis

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
