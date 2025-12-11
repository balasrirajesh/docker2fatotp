# docker2fatotp
# Secure 2FA Microservice

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![Security](https://img.shields.io/badge/Security-RSA%20%2F%20TOTP-red)

A production-ready, containerized microservice that implements **Public Key Infrastructure (PKI)** and **Time-based One-Time Password (TOTP)** authentication. This project demonstrates secure seed transmission using RSA encryption, persistent storage management, and automated background tasks using Cron.

---

## 🚀 Features

* **Secure Seed Transmission:** Decrypts RSA-encrypted seeds using OAEP padding with SHA-256.
* **TOTP Generation:** Generates standard 6-digit 2FA codes (RFC 6238 compliant).
* **Verification Logic:** Verifies codes with a time-window tolerance of ±30 seconds to account for clock drift.
* **Dockerized:** Fully containerized using a multi-stage Docker build for optimized image size.
* **Automated Logging:** Background Cron job logs valid 2FA codes every minute to a persistent volume.
* **Persistence:** Uses Docker Volumes to ensure seed data survives container restarts.

---

## 🛠️ Tech Stack

* **Language:** Python 3.11
* **Framework:** FastAPI
* **Cryptography:** `cryptography` (RSA/OAEP), `pyotp` (TOTP), `hashlib`
* **Containerization:** Docker & Docker Compose
* **Scheduling:** Linux Cron

---

## 📂 Project Structure

```text
├── main.py                 # FastAPI application and logic
├── Dockerfile              # Multi-stage build configuration
├── docker-compose.yml      # Service and volume definition
├── requirements.txt        # Python dependencies
├── cron/
│   └── 2fa-cron            # Cron schedule configuration
├── scripts/
│   └── log_2fa_cron.py     # Script to log codes (runs every minute)
└── README.md
⚙️ Setup & Installation
Prerequisites
Docker Desktop installed and running.

Git.

1. Clone the Repository
Bash

git clone [https://github.com/balasrirajesh/docker2fatotp.git](https://github.com/balasrirajesh/docker2fatotp.git)
cd docker-solved-23MH1A05N6
2. Build and Run
Start the container using Docker Compose. This builds the image and mounts the necessary volumes.

Bash

docker-compose up --build -d
The API will be accessible at http://localhost:8080.

🔌 API Endpoints
1. Decrypt Seed
Decrypts the encrypted seed using the stored private key and saves it securely to persistent storage.

URL: /decrypt-seed

Method: POST

Body:

JSON

{
  "encrypted_seed": "BASE64_STRING_HERE..."
}
2. Generate 2FA Code
Returns the current valid TOTP code and remaining validity time.

URL: /generate-2fa

Method: GET

Response:

JSON

{
  "code": "123456",
  "valid_for": 15
}
3. Verify Code
Verifies a user-provided code against the stored seed (allows ±30s drift).

URL: /verify-2fa

Method: POST

Body:

JSON

{
  "code": "123456"
}
🕰️ Cron Job & Persistence
The service includes a background cron job that runs every minute. It generates a valid TOTP code and logs it to a file. Because of the Docker Volume configuration, this log persists even if you stop or restart the container.

To view the background logs:

Bash

docker exec <container_name> cat /cron/last_code.txt