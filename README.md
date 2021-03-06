# Déploiement d'une infrastructure "Windows Virtual Desktop"<br/>

Exemple de chaîne de déploiement complète d'une infrastructure Windows Virtual Desktop.<br/>
Ce "Workflows" "GitHub Actions" exécute quatre jobs:<br/>
- Build d'une image Windows 10 multi-sessions avec Microsoft 365 Apps  avec Packer :
    - L'installation et paramétrage de FSLogix
    - Player VLC
    - Notification via Webhook dans Slack 
- Test/What-If de template ARM
    - Génération du "Token Expiration Time"
    - Test & What-If du Template ARM
    - Notification via Webhook dans Slack 
- Déploiement du template ARM
    - "Deployment Approuval"
    - Génération du "Token Expiration Time"
    - Déploiement du template ARM
- Assignation d'un groupe utilisateurs à "l'application group"
    - Assignation d'un groupe utilsateur à l'"application group" en Powershell
    - Notification via Webhook dans Slack

Prérequis (WVD):<br/>
- Un controleur de domaine (Windows Serveur avec le rôle ADDS) dans Azure
- Le domaine ADDS (Windows Server) doit être synchronisé avec AD Connect
- Un serveur Windows Server membre du domaine Active Directory pour le stockage des profiles utilisateur (FSLogix)
- Un "virtual network/subnet" sur lequel on peut joindre de controleur de domaine
- Un Service Principal owner de l'abonnement Azure (déploiement de l'image via "Packer" & déploiement des ressources WVD)

Prérequis pour "GitHub Actions":<br/>
- Créer des secrets dans "<a href="https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository">Github Secret </a>" avec la structure ci-dessous:
    - AZURE_CREDENTIALS
    ```
  {
    "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "clientSecret": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "subscriptionId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  }
    ```
    - DOMAINE_USER_NAME_PASSWORD -> mot de passe d'un compte utilisateur du domaine (compte machine dans l'AD)
    ```
    PasswordAdmin123$
    ```
    - PWD_USER -> compte admin local des hosts
    ```
    PasswordUser123$
    ```   
    - SLACK_WEBHOOK_URL -> url du canal SLACK
    ```
    https://hooks.slack.com/services/XXXXXXXXXX/XXXXXXXXXXX/xxxxxxxxxxxxxxxxxx
    ```
    - SP_ID -> Service Principal (Packer)
    ```
    xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ```
    - SP_SECRET -> Secret du Service Principal (Packer)
    ```
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```
    - SUBSCRIPTION_ID -> ID de l'abonnement (Packer)
    ```
    xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ```
    - TENANT_ID -> ID du tenant (Packer)
    ```
    xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ```
