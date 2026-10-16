Jira project:
https://datamatikerfullystacked.atlassian.net/jira/software/projects/ISSUE/boards/1/backlog

## Live på Azure (VM)

| Del | Adresse |
|---|---|
| Hjemmeside (frontend) | http://20.240.135.254 |
| API (backend, via frontendens nginx) | http://20.240.135.254/api/movies |

Personale-login: mads / kode123

### Deployment
Hele systemet kører på én Azure-VM (Ubuntu 24.04) med Docker Compose (`compose.yaml` i BackendApp):

| Container | Indhold | Port |
|---|---|---|
| `kino_frontend` | nginx + HTML/CSS/JS fra FrontendApp | 80 (offentlig) |
| `kino_backend` | Spring Boot (bygget med multi-stage `Dockerfile`), profil `docker` | 8080 (kun internt) |
| `kinodb_mysql` | MySQL 8 fra Docker Hub | kun 127.0.0.1 |

- Browseren taler kun med nginx på port 80. nginx sender `/api`-kald videre til backenden
  på Docker-netværket, så frontend og API har samme adresse.
- VM'ens firewall har kun port 22 (SSH) og 80 åbne. Databasen kan ikke nås udefra.
- GitHub Actions (`.github/workflows/java.yaml`) compiler og tester koden ved push og pull requests.
- Opdatering af VM'en: `git pull` i begge repos og derefter
  `FRONTEND_PORT=80 docker compose --profile app up -d --build` (se README i BackendApp).
- Lokal udvikling bruger H2; Docker/VM bruger MySQL.
