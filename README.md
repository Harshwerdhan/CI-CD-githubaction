# CI-CD-githubaction

A simple CI/CD pipeline built with **GitHub Actions** that automatically deploys a static website to an **AWS EC2** instance every time code is pushed to the `main` branch.

## How It Works

```
git push (main) → GitHub Actions runner → SCP files to EC2 → SSH → copy to /var/www/html → live site
```

1. A push to `main` triggers the workflow (`.github/workflows/main.yml`).
2. The runner checks out the code (`actions/checkout`).
3. Files are copied to the EC2 instance over SCP (`appleboy/scp-action`).
4. The workflow connects to EC2 via SSH (`appleboy/ssh-action`) and copies the files to the web server's document root `/var/www/html`.

## Tech Stack

- GitHub Actions (CI/CD)
- AWS EC2 (Ubuntu) with a web server (Nginx/Apache)
- SCP / SSH for secure file transfer
- HTML (sample static site)

## Project Structure

```
.
├── .github/workflows/main.yml   # CI/CD workflow
├── index.html                   # Home page
└── pages/
    ├── about.html
    └── contact.html
```

## Prerequisites

- An AWS EC2 instance (Ubuntu) with a web server installed and port 22 (SSH) and 80 (HTTP) open in the security group
- An SSH key pair for the instance

## Setup

1. Fork or clone this repo.
2. In GitHub go to **Settings → Secrets and variables → Actions** and add:

   | Secret | Description |
   |---|---|
   | `EC2_HOST` | Public IP / DNS of the EC2 instance |
   | `EC2_USERNAME` | SSH user (e.g. `ubuntu`) |
   | `EC2_SSH_KEY` | Private key contents (`.pem`) |

3. Push a change to `main`.
4. Open the EC2 public IP in a browser to see the updated site. Pipeline progress is under the **Actions** tab.

## Future Improvements

- Add a lint/test step before deployment
- Use `rsync` or a Docker-based deployment instead of copying files
- Restrict SSH access and use a dedicated deploy user
