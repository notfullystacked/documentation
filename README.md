Jira project:
https://datamatikerfullystacked.atlassian.net/jira/software/projects/ISSUE/boards/1/backlog

## Live på Azure

| Del | Adresse |
|---|---|
| Hjemmeside (frontend) | https://kino-frontend-nfs-cggkfffkbnfnc8c4.swedencentral-01.azurewebsites.net 
| API (backend) | https://kino-backend-nfs-bca3hqgzabfhauev.swedencentral-01.azurewebsites.net/api/movies 

Personale-login: mads / kode123

### Deployment
Ved hvert push til `main` bygger GitHub Actions (`.github/workflows/docker-azure.yml`) et Docker-image,
sender det til Azure Container Registry og opdaterer Azure Web App'en.
Frontenden er en nginx-container, der sender `/api`-kald videre til backenden.
Backenden kører med Spring-profilen `docker` mod Azure Database for MySQL, mens lokal udvikling bruger H2.
Passwords ligger som GitHub Secrets og miljøvariabler i Azure, ikke i koden.
