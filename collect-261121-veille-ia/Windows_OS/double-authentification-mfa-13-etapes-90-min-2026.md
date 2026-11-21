---
id: collect-261121-veille-ia/windows-os/double-authentification-mfa-13-etapes-90-min-2026
title: "Double Authentification (MFA) : 13 Étapes, 90 Min [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["double", "authentification", "microsoft", "comme", "entreprises", "passe", "compte", "vous"]
source: https://tech-insider.org/fr/double-authentification-mfa-phishing-2026/
source_anchor: ""
source_lines: [1, 331]
section: "Windows_OS"
---
Seules 27 % des entreprises de moins de 25 salariés ont activé une forme de double authentification sur leurs comptes professionnels, contre 87 % des groupes de plus de 10 000 employés, selon les données 2025 de JumpCloud. Cet écart explique en grande partie pourquoi les petites structures restent la cible favorite des campagnes de phishing qui visent Microsoft 365 et les messageries professionnelles. Ce tutoriel détaille, étape par étape, comment activer une double authentification qui résiste vraiment aux techniques de contournement actuelles, avec les commandes PowerShell nécessaires, les pièges classiques à éviter et un script complet prêt à déployer sur votre tenant Microsoft Entra ID.

## Pourquoi la double authentification est votre première ligne de défense en 2026

Le mot de passe seul a cessé de protéger grand-chose depuis longtemps. Les campagnes de phishing automatisées, souvent vendues comme service clé en main sur des forums spécialisés, permettent à n’importe quel attaquant de récupérer des identifiants en quelques minutes à peine. La double authentification (MFA, pour multi-factor authentication) ajoute une couche de vérification qui bloque la majorité de ces tentatives, même quand le mot de passe a déjà fuité dans une base de données compromise.

En France, l’ANSSI a mis à jour sa stratégie nationale de cybersécurité en mars 2026, avec un accent marqué sur la résilience des identités numériques et la réduction de l’impact des compromissions de comptes. Le CERT-FR, dans son bilan des menaces 2025, continue de recommander un contact direct ([email protected] ou le 3218) dès qu’un compte semble compromis, signe que le vol d’identifiants reste une source majeure de sollicitations. Au niveau européen, l’ENISA a lancé l’exercice Cyber Europe 2026 pour tester la réponse collective des États membres face à des scénarios de compromission à grande échelle.

Les chiffres mondiaux racontent la même histoire. Le taux d’adoption du MFA au sein des effectifs professionnels a atteint 70 % en janvier 2025, contre 66 % un an plus tôt, selon le rapport Okta Secure Sign-in Trends 2025. Cette moyenne cache toutefois un déséquilibre net : les grandes entreprises dépassent largement les 80 %, quand les TPE et PME plafonnent souvent sous les 35 %. Ce sont précisément ces structures plus petites qui manquent de ressources pour surveiller manuellement chaque tentative de connexion suspecte, ce qui rend l’automatisation décrite plus loin d’autant plus utile.

Ce guide s’adresse aux administrateurs systèmes, aux responsables IT de PME et aux développeurs qui gèrent un tenant Microsoft 365 ou Entra ID et veulent passer d’un MFA basique à une configuration qui tient face aux techniques de contournement actuelles. Il s’inscrit dans notre couverture plus large de la cybersécurité, où nous suivons chaque semaine les failles, ransomwares et nouvelles obligations réglementaires qui touchent les entreprises européennes.

Un compte Microsoft 365 compromis ne se limite presque jamais à une boîte mail lue en douce. Une fois à l’intérieur, un attaquant peut créer des règles de transfert automatique invisibles, usurper l’identité d’un dirigeant pour valider un virement (la fraude dite au président), ou utiliser le compte comme point d’entrée pour déployer un ransomware sur l’ensemble du réseau via OneDrive et SharePoint. La double authentification n’élimine pas ce risque à elle seule, mais elle ferme la porte la plus utilisée pour y entrer, ce qui justifie de la traiter comme un projet prioritaire plutôt que comme une case à cocher dans un audit de conformité.

## Double authentification (MFA) : définition et fonctionnement technique

La double authentification combine au moins deux éléments de nature différente parmi trois catégories reconnues : un secret que vous connaissez (mot de passe, code PIN), un objet que vous possédez (téléphone, clé de sécurité, carte à puce) et une caractéristique qui vous est propre (empreinte digitale, reconnaissance faciale). Le NIST détaille ces niveaux de garantie d’authentification (AAL1 à AAL3) dans son guide SP 800-63B, qui sert de référence à la plupart des standards utilisés par les fournisseurs cloud.

Dans la pratique, trois familles de méthodes dominent le marché actuel. Les codes à usage unique (OTP) envoyés par SMS ou générés par une application comme Microsoft Authenticator reposent sur l’algorithme TOTP normalisé (RFC 6238), qui produit un code à six chiffres renouvelé toutes les 30 secondes à partir d’une clé secrète partagée. Les notifications push demandent simplement à l’utilisateur d’approuver la connexion depuis son téléphone, un mécanisme confortable qui a toutefois ouvert la porte aux attaques par lassitude MFA, où l’attaquant spamme les demandes d’approbation jusqu’à ce qu’un utilisateur fatigué clique sur « Approuver » par réflexe.

La troisième famille, bâtie sur le standard FIDO2/WebAuthn, fonctionne différemment : elle génère une paire de clés cryptographiques liée au domaine exact du service. Une clé de sécurité FIDO2 ou un passkey ne peut tout simplement pas être utilisé sur un faux site, même parfaitement imité, puisque la signature cryptographique échoue si le domaine ne correspond pas exactement. C’est cette propriété qui rend cette famille de méthodes réellement résistante au phishing, une distinction que Microsoft et Google mettent aujourd’hui en avant dans leur documentation.

Microsoft Entra ID (anciennement Azure Active Directory) centralise la gestion de ces méthodes via une politique unique, l’« Authentication methods policy » décrite dans la documentation Microsoft Learn, accessible aussi bien depuis le portail d’administration que depuis l’API Microsoft Graph. C’est ce qui permet d’automatiser entièrement son déploiement, comme dans les étapes suivantes.

## MFA classique vs MFA résistante au phishing : ce que les attaquants exploitent

Un code OTP reçu par SMS ou généré par une application TOTP protège contre le vol de mot de passe seul, mais pas contre l’interception en temps réel. Les kits de phishing-as-a-service comme Tycoon2FA ou EvilProxy, documentés par plusieurs éditeurs de sécurité, fonctionnent sur le principe de l’adversary-in-the-middle (AiTM) : ils placent un serveur proxy entre la victime et le vrai portail de connexion Microsoft. La victime saisit son mot de passe, reçoit et valide son code MFA normalement, mais le proxy capture au passage le jeton de session final. L’attaquant rejoue ensuite ce jeton pour accéder au compte, sans jamais avoir eu besoin de connaître le mot de passe ni le code à l’avance.

Ce type d’attaque explique pourquoi le SMS et même les applications TOTP classiques ne suffisent plus pour protéger des comptes à privilèges : administrateurs, direction financière, accès aux données RH. Le SIM swapping, où un attaquant convainc un opérateur téléphonique de transférer un numéro vers une carte SIM qu’il contrôle, reste par ailleurs une méthode active pour intercepter des codes envoyés par SMS.

Face à cela, les méthodes FIDO2, les passkeys et Windows Hello for Business ferment la porte à ce type de contournement, parce que la preuve cryptographique est liée au domaine réel du service et ne peut pas être rejouée ailleurs. Microsoft recommande désormais explicitement de migrer les comptes sensibles vers ces méthodes en priorité, tout en conservant le MFA classique pour les comptes à moindre risque le temps de la transition. Le rapport Okta Secure Sign-in Trends 2025 confirme cette bascule : l’adoption de l’authentification sans mot de passe résistante au phishing a progressé de 63 % en un an, passant de 8,6 % à 14,0 % des utilisateurs professionnels suivis par la plateforme.

Le tutoriel qui suit permet justement de construire cette transition en douceur, sans bloquer vos utilisateurs du jour au lendemain. Si vous cherchez une alternative entièrement sans mot de passe, notre tutoriel dédié aux passkeys FIDO2 détaille la configuration côté utilisateur final.

## Panorama des méthodes d’authentification : ce qui fonctionne vraiment en 2026

Avant de choisir votre configuration cible, il vaut mieux comparer objectivement les méthodes disponibles sur Microsoft Entra ID. Le tableau ci-dessous résume leur niveau de résistance au phishing, leur complexité de déploiement et leur coût réel.

| Méthode | Résistance au phishing | Facilité de déploiement | Coût | Recommandation 2026 | 
|---|---|---|---|---|
| SMS / appel vocal (OTP) | Faible (interceptable, SIM swap) | Très simple | Frais opérateur | À éliminer en priorité | 
| Application TOTP (Authenticator) | Moyenne (vulnérable à l’AiTM) | Simple | Gratuit | Acceptable en transition | 
| Notification push + number matching | Moyenne à bonne | Simple | Gratuit | Bon compromis pour une PME | 
| Clé de sécurité FIDO2 / passkey | Élevée (liée au domaine) | Modérée (matériel à distribuer) | 25 à 60 € par clé | Recommandé pour comptes sensibles | 
| Windows Hello for Business | Élevée | Modérée (nécessite un join Entra ID) | Inclus dans la licence | Recommandé sur parc Windows 11 | 
| Certificat / carte à puce | Élevée | Complexe (PKI à gérer) | Variable | Secteur public et réglementé | 

Dans la majorité des PME, la combinaison la plus réaliste consiste à démarrer avec Microsoft Authenticator et le number matching pour l’ensemble des utilisateurs, puis à réserver les clés FIDO2 aux comptes à privilèges où le coût du matériel se justifie face au risque. C’est exactement la logique suivie dans les 13 étapes détaillées plus loin.

## Prérequis : comptes, licences et versions nécessaires

Réunissez les éléments suivants avant de commencer. Un déploiement mal préparé reste la première cause d’échec d’un projet MFA, bien avant la complexité technique elle-même.

| Élément | Version ou niveau minimal | Rôle dans le tutoriel | 
|---|---|---|
| Abonnement Microsoft 365 | Business Premium, E3/E5, ou Entra ID P1/P2 | Accès conditionnel et Identity Protection | 
| Système d’exploitation admin | Windows 11 ou macOS 14 et supérieur | Poste d’administration | 
| PowerShell | 7.4 LTS ou supérieur | Exécution des scripts | 
| Module Microsoft.Graph | Dernière version (PowerShell Gallery) | Automatisation via Microsoft Graph API | 
| Microsoft Authenticator | Dernière version (iOS et Android) | Méthode MFA principale | 
| Clé de sécurité FIDO2 (optionnel) | Certifiée FIDO2, ex. série YubiKey 5 | MFA résistant au phishing | 
| Rôle d’administrateur | Administrateur d’accès conditionnel ou de sécurité | Création des politiques | 

Si vous gérez moins de 300 utilisateurs et n’avez pas de licence Entra ID P1/P2, les valeurs de sécurité par défaut (Security Defaults) de Microsoft 365 constituent un point de départ correct : elles imposent un MFA basique gratuitement, sans la granularité de l’accès conditionnel décrite ici. Les étapes suivantes supposent une licence Entra ID P1 au minimum. Vérifiez également que votre compte dispose du rôle Administrateur d’accès conditionnel avant de passer à l’étape 1 : un rôle insuffisant provoque l’erreur la plus fréquente rencontrée en support, détaillée dans la section dépannage plus bas.

## Tutoriel : activer la double authentification en 13 étapes

Les treize étapes suivantes font passer un tenant sans politique MFA cohérente à une configuration qui impose une authentification résistante au phishing pour les comptes sensibles, tout en gardant une voie de secours pour ne jamais se retrouver bloqué dehors. Comptez environ 90 minutes pour l’ensemble, hors temps de distribution des clés physiques si vous en commandez.

### Étapes 1 à 4 : préparer le terrain avant le déploiement

**Étape 1 : vérifiez les licences et attribuez les rôles nécessaires.** Confirmez dans le Centre d’administration Microsoft 365 que les utilisateurs concernés disposent d’une licence incluant Entra ID P1 (Business Premium, E3 ou E5 la couvrent déjà). Attribuez ensuite le rôle Administrateur d’accès conditionnel à votre compte de déploiement, plutôt que d’utiliser un compte Administrateur général, pour limiter la surface d’attaque de vos propres identifiants.

**Étape 2 : connectez-vous à Microsoft Graph PowerShell et auditez l’état actuel.** Avant de modifier quoi que ce soit, générez un inventaire complet des méthodes déjà enregistrées par vos utilisateurs. Cela évite de bloquer des comptes qui n’ont encore aucune méthode MFA configurée.

```
Connect-MgGraph -Scopes "Reports.Read.All","Policy.Read.All","UserAuthenticationMethod.Read.All"
Get-MgReportAuthenticationMethodUserRegistrationDetail -All |
    Select-Object UserPrincipalName, IsMfaRegistered, IsPasswordlessCapable, MethodsRegistered |
    Export-Csv -Path .\audit-mfa-2026.csv -NoTypeInformation -Encoding UTF8
```
La commande exporte un fichier CSV listant, pour chaque utilisateur, si le MFA est enregistré, si le compte est compatible sans mot de passe et quelles méthodes sont actives. Voici un extrait représentatif du résultat obtenu sur un tenant de test :

```
UserPrincipalName            IsMfaRegistered  IsPasswordlessCapable  MethodsRegistered
----------------------------  ---------------  ---------------------  --------------------------------
[email protected]           True             False                  {microsoftAuthenticatorPush, sms}
[email protected]            False            False                  {}
[email protected]           True             True                   {fido2SecurityKey}
```
**Étape 3 : créez un compte d’accès d’urgence (break-glass) avant toute modification.** Créez au moins deux comptes cloud-only, exclus de toute politique d’accès conditionnel et de toute exigence MFA, avec un mot de passe long généré aléatoirement et stocké hors ligne, par exemple dans un coffre-fort physique. Ces comptes évitent de se retrouver totalement bloqué hors de son tenant si une politique mal configurée bloque tous les administrateurs en même temps, un scénario plus courant qu’on ne le pense.

**Étape 4 : déployez Microsoft Authenticator auprès des utilisateurs.** Lancez une campagne d’inscription groupée depuis Entra ID (Protection, puis Authentication methods, puis Registration campaign) pour inciter chaque utilisateur à installer Microsoft Authenticator et à enregistrer son compte avant la date limite fixée. Prévoyez une communication interne claire : une bascule surprise vers le MFA obligatoire génère mécaniquement un pic de tickets au support technique.

### Étapes 5 à 9 : basculer vers un MFA qui résiste au phishing

**Étape 5 : activez le number matching et les notifications enrichies.** Dans les paramètres de la méthode Microsoft Authenticator, activez la correspondance de numéro : l’utilisateur doit désormais saisir le chiffre affiché à l’écran de connexion dans l’application, au lieu de simplement appuyer sur « Approuver ». Cette seule modification réduit fortement l’efficacité des attaques par lassitude MFA, car elle empêche l’approbation accidentelle ou automatique.

**Étape 6 : créez une politique d’accès conditionnel exigeant le MFA.** Plutôt que de cliquer manuellement dans le portail, définissez la politique via Microsoft Graph pour pouvoir la reproduire sur plusieurs tenants.

```
$params = @{
    displayName = "Exiger le MFA pour tous les utilisateurs"
    state       = "enabledForReportingButNotEnforced"
    conditions  = @{
        users        = @{ includeUsers = @("All"); excludeUsers = @("
```
") }
        applications = @{ includeApplications = @("All") }
    }
    grantControls = @{
        operator        = "OR"
        builtInControls = @("mfa")
    }
}
New-MgIdentityConditionalAccessPolicy -BodyParameter $params Le paramètre `state` défini sur `enabledForReportingButNotEnforced` journalise l’impact de la politique sans bloquer personne. Cela permet de vérifier pendant quelques jours qu’aucun compte légitime n’est pénalisé avant de basculer sur `enabled`.

**Étape 7 : ajoutez une méthode résistante au phishing avec une clé de sécurité FIDO2.** Activez la méthode FIDO2 au niveau de la politique d’authentification du tenant, puis distribuez des clés physiques aux comptes à privilèges.

```
$body = @{
    "@odata.type" = "#microsoft.graph.fido2AuthenticationMethodConfiguration"
    state = "enabled"
    isSelfServiceRegistrationAllowed = $true
    isAttestationEnforced = $true
    keyRestrictions = @{
        isEnforced = $false
        enforcementType = "allow"
        aaGuids = @()
    }
} | ConvertTo-Json -Depth 5
Invoke-MgGraphRequest -Method PATCH `
    -Uri "https://graph.microsoft.com/v1.0/policies/authenticationMethodsPolicy/authenticationMethodConfigurations/Fido2" `
    -Body $body -ContentType "application/json"
```
Le paramètre `isAttestationEnforced` réglé sur `$true` garantit que seules des clés certifiées FIDO2, et non des implémentations logicielles non vérifiées, peuvent être enregistrées. Un détail qui compte pour les comptes à haut risque.

**Étape 8 : activez Windows Hello for Business sur les postes Windows 11.** Pour les postes joints à Entra ID, activez Windows Hello for Business via Intune ou une stratégie de groupe. Les utilisateurs se connectent alors avec leur empreinte, leur visage ou un code PIN local, chaque facteur étant lié cryptographiquement à la puce TPM de l’appareil. Cette méthode couvre le même niveau de résistance au phishing que FIDO2, sans matériel supplémentaire à acheter.

**Étape 9 : désactivez progressivement le SMS et l’appel vocal.** Une fois qu’une majorité d’utilisateurs a basculé vers Authenticator ou une clé FIDO2, restreignez les méthodes les plus faibles.

```
$body = @{
    "@odata.type" = "#microsoft.graph.smsAuthenticationMethodConfiguration"
    state = "disabled"
} | ConvertTo-Json
Invoke-MgGraphRequest -Method PATCH `
    -Uri "https://graph.microsoft.com/v1.0/policies/authenticationMethodsPolicy/authenticationMethodConfigurations/Sms" `
    -Body $body -ContentType "application/json"
```
Ne réalisez cette étape qu’après avoir vérifié, via le rapport d’inscription généré à l’étape 2, qu’aucun utilisateur actif ne dépend plus exclusivement du SMS.

### Étapes 10 à 13 : verrouiller, automatiser et vérifier

**Étape 10 : bloquez les protocoles d’authentification hérités.** Les protocoles comme POP, IMAP basique ou SMTP AUTH ne savent pas transmettre de demande MFA : ils constituent donc une porte dérobée qui contourne entièrement toute politique mise en place plus haut. Créez une politique d’accès conditionnel dédiée qui bloque l’authentification héritée pour tous les utilisateurs, sauf exception documentée pour un système legacy identifié.

**Étape 11 : activez l’accès conditionnel basé sur le risque.** Avec une licence Entra ID P2, activez Identity Protection pour évaluer en temps réel le niveau de risque de chaque connexion (adresse IP inhabituelle, déplacement impossible, jeton potentiellement volé) et exiger automatiquement une réauthentification MFA, ou bloquer la connexion quand le risque dépasse un seuil moyen.

**Étape 12 : générez des Temporary Access Pass pour l’inscription à distance.** Pour les nouveaux employés ou les utilisateurs qui perdent l’accès à leurs méthodes existantes, générez un Temporary Access Pass à durée de vie limitée depuis le portail Entra ID. Ce code temporaire permet d’enregistrer une nouvelle méthode MFA sans jamais transmettre de mot de passe par téléphone ou par e-mail, un vecteur d’ingénierie sociale classique.

**Étape 13 : testez le parcours utilisateur et surveillez les journaux de connexion.** Connectez-vous avec un compte test pour valider chaque méthode de bout en bout, puis mettez en place une surveillance continue des journaux de connexion Entra ID, idéalement remontés dans Microsoft Sentinel ou un autre SIEM.

```
SigninLogs
| where ResultType == 0
| where AuthenticationRequirement == "multiFactorAuthentication"
| where TimeGenerated > ago(1d)
| summarize Connexions = count() by UserPrincipalName, AppDisplayName
| where Connexions > 20
| order by Connexions desc
```
Cette requête repère les comptes qui accumulent un nombre anormal de connexions MFA réussies en une journée, un signal parfois révélateur d’un jeton volé rejoué en boucle par un script automatisé.

## Script complet : automatiser le déploiement avec Microsoft Graph PowerShell

Pour éviter de reproduire chaque commande une à une sur plusieurs tenants, voici un script qui regroupe l’audit initial, la création de la politique d’accès conditionnel et l’activation de FIDO2 en une seule exécution. Remplacez l’identifiant du compte break-glass par le vôtre, et testez-le systématiquement sur un tenant de démonstration avant la production.

```
# deploy-mfa-hardening.ps1
# Script complet : audit, politique d'accès conditionnel et FIDO2
param(
    [string]$BreakGlassAccountId = "
```
"
)
Connect-MgGraph -Scopes "Reports.Read.All","Policy.ReadWrite.ConditionalAccess","Policy.ReadWrite.AuthenticationMethod"
Write-Host "Étape 1/3 : audit des méthodes MFA enregistrées..."
Get-MgReportAuthenticationMethodUserRegistrationDetail -All |
    Select-Object UserPrincipalName, IsMfaRegistered, IsPasswordlessCapable |
    Export-Csv -Path .\audit-mfa-2026.csv -NoTypeInformation -Encoding UTF8
Write-Host "Rapport enregistré dans audit-mfa-2026.csv"
Write-Host "Étape 2/3 : création de la politique d'accès conditionnel (mode audit)..."
$caParams = @{
    displayName = "Exiger le MFA pour tous les utilisateurs"
    state       = "enabledForReportingButNotEnforced"
    conditions  = @{
        users        = @{ includeUsers = @("All"); excludeUsers = @($BreakGlassAccountId) }
        applications = @{ includeApplications = @("All") }
    }
    grantControls = @{ operator = "OR"; builtInControls = @("mfa") }
}
New-MgIdentityConditionalAccessPolicy -BodyParameter $caParams
Write-Host "Politique créée en mode rapport uniquement."
Write-Host "Étape 3/3 : activation de FIDO2 comme méthode résistante au phishing..."
$fido2Body = @{
    "@odata.type" = "#microsoft.graph.fido2AuthenticationMethodConfiguration"
    state = "enabled"
    isSelfServiceRegistrationAllowed = $true
    isAttestationEnforced = $true
} | ConvertTo-Json -Depth 5
Invoke-MgGraphRequest -Method PATCH `
    -Uri "https://graph.microsoft.com/v1.0/policies/authenticationMethodsPolicy/authenticationMethodConfigurations/Fido2" `
    -Body $fido2Body -ContentType "application/json"
Write-Host "Déploiement terminé. Vérifiez le portail Entra ID avant de passer en mode enforced." Enregistrez ce script sous le nom `deploy-mfa-hardening.ps1` et exécutez-le avec un compte disposant des rôles Administrateur d’accès conditionnel et Administrateur de méthode d’authentification. Le script journalise chaque action dans la console, ce qui facilite l’audit après coup.

## Résultats attendus : exemples de sortie et vérifications

Une fois le script exécuté, la console affiche une progression semblable à celle-ci :

```
Étape 1/3 : audit des méthodes MFA enregistrées...
Rapport enregistré dans audit-mfa-2026.csv
Étape 2/3 : création de la politique d'accès conditionnel (mode audit)...
Politique créée en mode rapport uniquement.
Étape 3/3 : activation de FIDO2 comme méthode résistante au phishing...
Déploiement terminé. Vérifiez le portail Entra ID avant de passer en mode enforced.
```
Trois vérifications confirment ensuite que le déploiement s’est bien déroulé.

- La politique apparaît dans Entra ID, sous Protection puis Accès conditionnel, avec l’état attendu.
- Le rapport d’inscription MFA de l’étape 2 montre une progression du pourcentage d’utilisateurs inscrits chaque semaine.
- Les journaux de connexion affichent `authenticationRequirement: multiFactorAuthentication` pour les applications ciblées.

Si l’un de ces trois points ne correspond pas à ce qui est attendu, consultez la section dépannage plus bas avant de basculer la politique en mode `enabled`.

## Adoption du MFA en entreprise : les chiffres 2025-2026

Les chiffres suivants aident à situer une organisation par rapport au reste du marché, et à justifier si besoin un budget dédié auprès de la direction.

| Taille d’entreprise | Taux d’adoption MFA | Source | 
|---|---|---|
| Jusqu’à 25 salariés | 27 % | JumpCloud, 2025 | 
| 26 à 100 salariés | 34 % | JumpCloud, 2025 | 
| 1 001 à 10 000 salariés | 78 % | JumpCloud, 2025 | 
| Plus de 10 000 salariés | 87 % | JumpCloud, 2025 | 
| Secteur technologique (tous effectifs) | 87 % | JumpCloud, 2025 | 
| Moyenne mondiale, tous secteurs | 70 % (+4 points sur un an) | Okta Secure Sign-in Trends 2025 | 
| Authentification sans mot de passe résistante au phishing | 14,0 % (contre 8,6 % un an plus tôt) | Okta Secure Sign-in Trends 2025 | 

L’écart entre grandes et petites structures ne s’explique pas uniquement par le budget. Selon une enquête JumpCloud menée en 2024 auprès de plus de 1 000 professionnels IT de PME, 83 % des répondants imposaient déjà le MFA pour l’ensemble des ressources de l’entreprise, ce qui montre que la volonté existe mais que la mise en œuvre technique reste le principal frein. Côté applications grand public, la bascule progresse aussi : Descope indique que 94 % des organisations proposent une forme de MFA à leurs clients, mais seulement 10 % l’appliquent de façon uniforme sur l’ensemble de leurs applications. La fragmentation reste donc la norme, y compris chez les éditeurs de logiciels eux-mêmes.

## 5 erreurs courantes qui sabotent votre déploiement MFA

Ces pièges reviennent le plus souvent dans les retours d’expérience des équipes IT qui déploient le MFA à grande échelle.

- **Ne pas créer de compte break-glass avant d’activer une politique large.** Sans ce filet de sécurité, une politique mal configurée peut bloquer tous les administrateurs en même temps, y compris de leurs propres comptes.
- **Laisser le SMS comme méthode de secours par défaut.** Cela réintroduit exactement la faille que la migration vers FIDO2 ou Authenticator cherche à combler.
- **Déployer sans campagne de communication interne.** Une bascule surprise génère un pic de tickets et pousse certains utilisateurs à contourner les règles, par exemple en partageant leur téléphone avec un collègue.
- **Oublier les comptes de service et les applications héritées.** Ces comptes cassent silencieusement en production s’ils sont inclus dans une politique MFA sans exception explicite.
- **Ignorer le rapport d’inscription avant l’enforcement.** Basculer une politique en mode obligatoire sans vérifier qui n’est pas encore inscrit bloque les retardataires du jour au lendemain.
- **Ne pas activer le number matching.** Sans cette option, la porte reste ouverte aux attaques par lassitude MFA décrites plus haut.

## Dépannage : 9 problèmes fréquents et leurs solutions

Voici les incidents les plus signalés lors d’un déploiement MFA sur Microsoft Entra ID, avec la cause probable et la correction à appliquer.

- **Erreur « Insufficient privileges to complete the operation » lors de Connect-MgGraph.** Les scopes demandés à la connexion ne correspondent pas aux permissions consenties par un administrateur. Revérifiez la liste des scopes et redonnez le consentement admin si nécessaire.
- **Code AADSTS50076 ou AADSTS50079 à la connexion.** Ce n’est pas un bug : l’utilisateur doit simplement terminer son inscription MFA obligatoire avant de continuer.
- **Notification push jamais reçue.** Vérifiez les autorisations de notification en arrière-plan de l’application, la connectivité réseau du téléphone, et réenregistrez l’appareil si le problème persiste.
- **Clé FIDO2 non reconnue par le navigateur.** Le navigateur est probablement obsolète et ne supporte pas WebAuthn correctement, ou le pilote USB de la clé manque sur le poste.
- **Boucle de vérification MFA infinie.** Un cookie de session corrompu ou une horloge système désynchronisée en est souvent la cause, puisque le TOTP dépend d’une horloge précise à quelques secondes près.
- **Comptes de service bloqués par la politique.** Excluez-les explicitement de la politique d’accès conditionnel et migrez-les vers des identités managées quand c’est techniquement possible.
- **Rapport d’inscription vide ou incomplet.** Il s’agit généralement d’un délai de réplication côté Microsoft Graph. Réessayez après quelques heures ou vérifiez que le scope Reports.Read.All a bien été accordé.
- **Conflit entre plusieurs politiques d’accès conditionnel.** Utilisez l’outil de simulation « What If » d’Entra ID pour repérer les chevauchements avant qu’ils ne bloquent un utilisateur légitime.
- **Synchronisation Microsoft Entra Connect en retard.** Les nouveaux groupes de sécurité utilisés dans une politique n’apparaissent pas immédiatement : forcez un cycle de synchronisation delta pour accélérer la propagation.

## Conseils avancés : accès conditionnel basé sur le risque et sans mot de passe

Une fois le socle des 13 étapes en place, plusieurs options permettent d’aller plus loin. La fonctionnalité Authentication Strengths d’Entra ID permet d’exiger spécifiquement une méthode résistante au phishing, FIDO2 ou certificat, pour les applications les plus sensibles, tout en laissant Authenticator classique pour le reste du parc. Cette granularité évite d’imposer du matériel physique à l’ensemble de l’organisation alors que seuls quelques comptes en ont réellement besoin.

Identity Protection, disponible avec Entra ID P2, va plus loin que le simple accès conditionnel classique en calculant un score de risque par utilisateur et par connexion, basé sur des signaux comme les adresses IP anonymisées, les schémas de connexion inhabituels ou la présence de l’identifiant dans une fuite de données connue. Coupler ce score à une politique qui force automatiquement une réinscription MFA en cas de risque élevé referme rapidement la fenêtre d’exploitation d’un identifiant volé.

Enfin, pensez à faire remonter la télémétrie MFA (échecs répétés, changements de méthode, nouvelles inscriptions) vers votre SIEM plutôt que de la laisser dormir dans les journaux Entra ID. Une règle de détection simple, comme celle utilisée à l’étape 13, suffit souvent à repérer une tentative de contournement avant qu’elle ne se transforme en incident. À moyen terme, l’objectif réaliste pour la plupart des organisations reste une bascule complète vers le sans mot de passe : moins de tickets de réinitialisation, moins de surface d’attaque, et une expérience de connexion plus rapide pour les utilisateurs.

Pour limiter les frictions, déployez toujours par vagues plutôt qu’en une seule bascule générale. Un premier groupe pilote, composé de l’équipe IT elle-même et de quelques utilisateurs volontaires, permet de repérer les applications legacy qui cassent avec l’accès conditionnel avant que le reste de l’entreprise ne s’en aperçoive. Ajoutez ensuite les comptes à privilèges, puis élargissez département par département en gardant environ une semaine entre chaque vague. Ce rythme laisse le temps au support de traiter les tickets sans être submergé, et donne une fenêtre pour ajuster les exclusions de la politique si un outil métier critique se révèle incompatible avec l’authentification héritée bloquée à l’étape 10.

## Au-delà de Microsoft 365 : Google Workspace, GitHub et AWS

La logique décrite dans ce tutoriel s’applique presque telle quelle à d’autres plateformes. Sur Google Workspace, la validation en deux étapes peut être rendue obligatoire au niveau de l’organisation depuis la console d’administration, avec un support natif des clés de sécurité FIDO2 pour les comptes à privilèges. Sur GitHub, l’authentification à deux facteurs est déjà exigée pour les contributeurs de code depuis plusieurs années, et la plateforme prend désormais en charge les passkeys pour une connexion sans mot de passe.

Du côté d’AWS, le compte racine (root) devrait systématiquement être protégé par un dispositif MFA matériel dédié, distinct de celui utilisé pour les comptes IAM quotidiens, afin d’isoler l’accès le plus critique du reste de l’organisation. Bonne nouvelle pour les équipes qui suivent ce guide : une même clé de sécurité FIDO2 physique peut généralement être enregistrée sur plusieurs de ces plateformes à la fois, puisque WebAuthn est un standard ouvert et non une technologie propriétaire à Microsoft.

La double authentification fonctionne aussi mieux quand elle s’appuie sur des mots de passe uniques et suffisamment longs pour chaque service, ce qui suppose en pratique d’utiliser un gestionnaire de mots de passe plutôt que de réutiliser la même combinaison partout. Notre comparatif des gestionnaires de mots de passe et notre tutoriel pour auto-héberger Vaultwarden détaillent les deux approches, commerciale et auto-hébergée, pour combler ce premier facteur avant d’ajouter le second.

## Foire aux questions

**Qu’est-ce que la double authentification (MFA) ?**

C’est une méthode de connexion qui exige au moins deux preuves d’identité différentes, par exemple un mot de passe et un code généré par une application, avant d’accorder l’accès à un compte. L’objectif est qu’un attaquant qui vole un seul facteur, le mot de passe le plus souvent, ne puisse pas se connecter seul.

**Le MFA par SMS protège-t-il encore contre le phishing en 2026 ?**

Partiellement. Il reste largement préférable à l’absence de MFA, mais il ne résiste pas aux kits de phishing AiTM ni au SIM swapping. Pour les comptes à privilèges, une méthode FIDO2 ou Windows Hello for Business est recommandée à la place.

**Quelle est la différence entre MFA et authentification sans mot de passe ?**

Le MFA classique conserve le mot de passe comme premier facteur et y ajoute une vérification supplémentaire. L’authentification sans mot de passe, via FIDO2 ou passkey, supprime entièrement le mot de passe au profit d’une preuve cryptographique liée à l’appareil.

**Combien de temps faut-il pour déployer le MFA dans une PME ?**

Le socle technique décrit dans ce tutoriel prend environ 90 minutes à mettre en place. La phase d’inscription des utilisateurs et de bascule progressive, elle, s’étale généralement sur deux à quatre semaines pour éviter un pic de tickets au support.

**Le MFA est-il obligatoire avec Microsoft 365 ?**

Microsoft impose déjà le MFA pour certaines actions administratives sensibles et pousse activement les nouveaux tenants vers l’activation des valeurs de sécurité par défaut. L’application complète à l’ensemble des comptes dépend toutefois encore de la configuration choisie par chaque organisation, d’où l’intérêt de suivre les étapes de ce tutoriel.

**Que faire en cas de perte d’une clé de sécurité FIDO2 ?**

Enregistrez toujours une seconde clé de secours au moment de l’inscription initiale. En cas de perte des deux, un Temporary Access Pass généré par un administrateur permet de réinscrire une nouvelle méthode sans jamais transmettre de mot de passe par un canal non sécurisé.

**Une clé FIDO2 fonctionne-t-elle avec plusieurs comptes à la fois ?**

Oui. Une même clé physique peut stocker des identifiants pour Microsoft, Google, GitHub et de nombreux autres services, puisque FIDO2 et WebAuthn sont des standards ouverts partagés par l’ensemble de l’industrie.

**Quelle est la meilleure méthode MFA contre le phishing en 2026 ?**

Les méthodes classées AAL3 par le NIST, à savoir les clés de sécurité FIDO2, les passkeys liés à l’appareil et Windows Hello for Business, offrent aujourd’hui le meilleur niveau de résistance documenté face aux techniques de contournement actuelles.

### Pour aller plus loin

La double authentification ne demande ni gros budget ni compétences exotiques : les 13 étapes ci-dessus tiennent en une matinée pour un tenant de taille moyenne. Ce qui prend réellement du temps, c’est la conduite du changement auprès des utilisateurs. Commencez par les comptes à privilèges, mesurez l’impact avec le rapport d’inscription avant chaque bascule en mode obligatoire, et gardez toujours un compte break-glass hors de portée de vos propres politiques.
