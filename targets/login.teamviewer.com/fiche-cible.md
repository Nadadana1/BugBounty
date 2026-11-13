# Fiche cible — login.teamviewer.com

## Programme et périmètre

Programme étudié — TeamViewer - Bounty Program, sur YesWeHack.

La console `https://login.teamviewer.com` figure dans le périmètre. Ma partie du projet porte sur la page `/LogOn`. Les domaines absents du périmètre officiel sont exclus. La valeur de cet actif est « Critical ». Les récompenses indiquées par gravité sont de 200 € (Low), 1 000 € (Medium), 4 000 € (High) et 10 000 € (Critical).

Les essais doivent être non destructifs. Le déni de service et les interventions sur l’infrastructure sont interdits. La politique demande de s’abstenir d’outils automatisés et de limiter les requêtes. Aucun plafond numérique ni en-tête d’identification obligatoire n’a été relevé dans la page consultée. Les données des utilisateurs doivent être préservées. Une découverte doit être signalée sous 24 heures, exclusivement sur YesWeHack.

Les CSRF de faible gravité sur la connexion, l’énumération d’utilisateurs et les défauts de limitation des tentatives figurent parmi les exclusions de récompense. Cette liste est partielle. La classification « Critical » de l’actif ne détermine pas la gravité d’une éventuelle faille.

## Technologies

| Élément | État des connaissances |
| --- | --- |
| Interface | Titre affiché « TeamViewer Management Console » |
| Serveur et framework | À identifier |
| CDN et WAF | À identifier |
| Authentification | Champs e-mail et mot de passe visibles, texte proposant une authentification unique |
| Mécanisme de session et API | À examiner |

## Comptes de test

Compte de test personnel à prévoir pour la suite. Dans les notes, les comptes seront désignés par des alias comme « compte A ». Les mots de passe seront conservés dans un gestionnaire de mots de passe.

## Surface observée

| Élément visible | Observation initiale | État |
| --- | --- | --- |
| Connexion | E-mail, mot de passe, bouton « S’identifier » | Formulaire affiché |
| Maintien de connexion | Case « Maintenir la connexion » décochée | Affichage observé |
| Récupération du mot de passe | Lien `/LogOn#lost-password` | Destination du lien relevée |
| Inscription | Lien `/LogOn#register` | Destination du lien relevée |
| Authentification unique | Texte « Connexion par authentification unique » | Parcours à explorer |
| Langue | Français affiché | Sélecteur visible |

Les liens `#lost-password` et `#register` donnent accès à des parties de l’interface. Les requêtes envoyées au serveur restent à examiner pour comprendre le fonctionnement de ces parcours.

## Pistes à creuser

- Identifier les parcours disponibles et leurs domaines de destination.
- Noter les protections rencontrées avec un compte de test.
- Choisir les deux ou trois CWE avec le groupe.
