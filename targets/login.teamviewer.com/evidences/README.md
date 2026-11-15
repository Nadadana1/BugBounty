# Figures de l'investigation — login.teamviewer.com

Ces quatre figures sont des **reconstitutions graphiques à partir des sorties de commande et observations fournies pendant l'investigation**. Elles reprennent le style du rapport collectif (figures numérotées, extraits techniques, encadrés rouges), mais **ne sont pas des captures d'écran authentiques** de Burp Suite, Arjun ou SQLmap. Ne pas les présenter comme des preuves brutes.

## Figures

![Figure 1 — Arjun](figure-01-arjun.svg)

*Figure 1 — Recherche de paramètres cachés avec Arjun 2.2.7. Six noms candidats extraits ; aucun paramètre caché confirmé.*

![Figure 2 — Wayback Machine](figure-02-wayback.svg)

*Figure 2 — Reconnaissance passive par Wayback Machine. Échantillon limité à 500 URL de www.teamviewer.com, 298 chemins distincts et trois paramètres GET.*

![Figure 3 — SQLmap](figure-03-sqlmap.svg)

*Figure 3 — Tests SQLmap sur UserName et réponses 403/429. Aucune injection SQL confirmée, conclusions limitées par la protection.*

![Figure 4 — Protection anti-bot](figure-04-challenge.svg)

*Figure 4 — Challenge anti-automatisation observé dans Burp Suite lors d'une vérification XSS. Résultat non concluant.*

## Usage dans le rapport

Utiliser ces figures pour **illustrer** la démarche et les résultats. Les preuves originales doivent être ajoutées séparément à partir de captures réelles, avec secrets (cookies, JWT, mot de passe, identifiants de session) masqués.

Voir aussi : [Fiche Arjun](../notes/recherche-parametres-arjun.md) et [Fiche Wayback Machine](../notes/reconnaissance-wayback-machine.md).


## Série complémentaire — résultats de mon investigation

Les figures suivantes reproduisent **les résultats textuels effectivement partagés dans la conversation**. Le rendu reprend le principe du rapport collectif : panneaux de requête/réponse, encadrement rouge des observations et légendes numérotées. Elles sont des **reconstitutions documentaires**, non des captures originales de logiciels. Les secrets ne sont pas reproduits.

![Figure 5 — Requête d'authentification](figure-05-burp-authentification.svg)

*Figure 5 — Champs observés de la requête POST d'authentification et réponse HTTP 200 observée dans Burp Suite ; valeurs sensibles masquées.*

![Figure 6 — Challenge anti-automatisation](figure-06-burp-challenge.svg)

*Figure 6 — Test de réflexion du marqueur et réponse HTTP 403 avec challenge Anubis ; test XSS non concluant.*

![Figure 7 — SQLmap](figure-07-sqlmap.svg)

*Figure 7 — Synthèse des deux essais SQLmap portant sur UserName, avec les nombres de réponses HTTP 403 et 429 relevés.*

![Figure 8 — Arjun](figure-08-arjun.svg)

*Figure 8 — Arjun 2.2.7 : six noms candidats extraits de la réponse, sans paramètre caché découvert.*

![Figure 9 — Wayback Machine](figure-09-wayback.svg)

*Figure 9 — API CDX : 500 URL retournées pour www.teamviewer.com, 298 chemins distincts et trois noms de paramètres GET.*

![Figure 10 — Synthèse](figure-10-synthese.svg)

*Figure 10 — Synthèse des vérifications et de leurs limites, sans vulnérabilité confirmée.*

**À ne pas confondre avec des preuves brutes :** pour un rapport de vulnérabilité, les vraies captures originales de Burp Suite et du terminal doivent être conservées à part, avec cookies, JWT et identifiants masqués.
