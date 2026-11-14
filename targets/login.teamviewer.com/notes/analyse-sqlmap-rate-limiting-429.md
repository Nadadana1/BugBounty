# Analyse sqlmap — limitation HTTP 429 sur l'authentification TeamViewer

**Date de l'observation :** 14 novembre 2026  
**Cible observée :** `https://login.teamviewer.com/LogOn/Authenticate`  
**Méthode :** POST, corps JSON  
**Paramètre analysé :** `UserName`  
**Outil :** sqlmap 1.10.11#pip  
**Statut :** observation documentée ; injection SQL **non démontrée**

## Objectif et périmètre

Analyser les réponses observées lors d'un essai de détection d'injection SQL sur le champ JSON `UserName` d'un endpoint d'authentification. Cette fiche décrit **uniquement** les messages du journal fourni. Elle ne démontre ni une faille SQLi ni l'identité d'une technologie WAF.

La requête de test utilisait deux valeurs fictives (`UserName=test`, `Password=test`). Les requêtes originales capturées ailleurs dans le projet comportaient des cookies et jetons sensibles : ils ne sont **pas reproduits** ici.

## Preuves horodatées — extraits du terminal

```text
[22:38:21] [*] starting @ 22:38:21 /2026-11-14/
JSON data found in POST body. Do you want to process it? [Y/n/q] Y
[22:38:22] [INFO] testing connection to the target URL
[22:38:52] [WARNING] target appears to be rate-limiting requests; sqlmap is backing off and throttling accordingly
[22:40:28] [WARNING] the web server responded with an HTTP error code (429) which could interfere with the results of the tests
[22:40:28] [INFO] checking if the target is protected by some kind of WAF/IPS
[22:41:09] [CRITICAL] heuristics detected that the target is protected by some kind of WAF/IPS
[22:41:09] [INFO] using a random (non-scanner) User-Agent and browser-like headers to bypass the WAF/IPS
[22:41:09] [INFO] testing if the target URL content is stable
[22:41:41] [CRITICAL] connection timed out to the target URL. sqlmap is going to retry the request(s)
[22:43:17] [INFO] target URL content is stable
[22:45:31] [WARNING] heuristic (basic) test shows that (custom) POST parameter 'JSON UserName' might not be SQL injectable
[22:47:51] [INFO] testing for SQL injection on (custom) POST parameter 'JSON UserName'
[22:47:51] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
```

*Ces extraits proviennent du journal transmis dans la conversation. Ce ne sont pas des captures réseau indépendantes. Le journal fourni se termine après le début des tests boolean-based blind.*

## Constatations et niveau de preuve

| Élément | Constat | Portée de la preuve |
| --- | --- | --- |
| Format de requête | sqlmap a reconnu le corps JSON POST | Confirmé par le journal |
| Code HTTP | Réponse `429` signalée à 22:40:28 | Confirmé par le journal de l'outil ; réponse brute non fournie |
| Limitation du trafic | sqlmap a signalé un *rate limiting* | Observé ; mécanisme et seuil exacts inconnus |
| WAF/IPS | Détection heuristique déclarée par sqlmap | Indice, **pas** identification certaine d'un WAF |
| Timeouts | Timeout à 22:41:41 | Observé ; cause exacte inconnue |
| SQLi sur `UserName` | Avertissement heuristique négatif, puis début de tests | **Ni exploitabilité ni innocuité démontrées** |

## Interprétation

L'erreur **HTTP 429 (Too Many Requests)** constitue le constat le plus solide : une réponse de limitation a été signalée pendant le scan. Le journal signale aussi une détection **heuristique** WAF/IPS et des délais de réponse. Cela peut perturber la comparaison des réponses utilisée par sqlmap, notamment pour les tests aveugles.

Les neuf minutes environ entre le lancement et les premiers tests SQLi résultent d'une séquence comportant limitation, attente, timeout et nouvelles tentatives ; **elles ne sont pas une preuve d'injection SQL temporelle**.

L'avertissement `might not be SQL injectable` ne démontre pas que l'endpoint est sécurisé. Aucun message du journal fourni ne confirme qu'il est injectable.

## Conclusion pour le rapport

> **Résultat : non concluant pour l'injection SQL.** La requête POST JSON a été reconnue ; une réponse HTTP 429 et une limitation présumée des requêtes ont été observées. sqlmap a également signalé une possible protection WAF/IPS, sur la base de ses heuristiques. Aucune injection SQL n'est confirmée par les preuves disponibles.

**Classification proposée :** observation de mécanisme anti-automatisation / limitation HTTP, **pas une vulnérabilité avérée**.  
**Sévérité CVSS :** non applicable sans impact de sécurité démontré.

## Limites et éléments complémentaires utiles

- Le journal transmis ne comprend ni les en-têtes complets du `429`, ni son corps, ni un fichier HAR/Burp de la réponse.
- Aucun seuil précis de limitation, ni technologie WAF, ne peut être déduit de ces seules lignes.
- L'extrait ne contient pas la fin du scan ni sa synthèse finale.
- Pour compléter la preuve, conserver une capture **expurgée** de la réponse HTTP `429` déjà enregistrée (statut, date, éventuel `Retry-After`) et la fin du journal, si disponible.
- Ne pas inclure dans le dépôt les cookies `.ASPXAUTH`, `ELT`, `BrowserAuthToken`, jetons SSO/CAPTCHA ni mots de passe réels. Révoquer les sessions exposées.

**Source primaire :** sortie terminal sqlmap fournie par l'utilisatrice, horodatée le 14/11/2026 à 22:38–22:47.
