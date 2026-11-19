# Investigation TeamViewer Account — Récupération de mot de passe

## Périmètre et objectif

L’investigation concerne le parcours de récupération de mot de passe sur `account.teamviewer.com`, observé avec un compte de test. Le contrôle porte sur l’invalidation du jeton de récupération après une réinitialisation réussie.

## Éléments examinés

| Observation | Interprétation |
| --- | --- |
| L’ouverture du lien de récupération a produit une réponse HTTP `200 OK` de type HTML. | Le chargement de la page ne suffit pas à établir la validité du jeton. |
| Les réponses de traduction de l’interface contiennent des libellés de réinitialisation, de confirmation et de connexion. | Ces fichiers statiques ne renseignent pas sur le résultat d’une opération ni sur l’état du jeton. |
| Un échange de vérification du statut de récupération figure dans les traces. Un `204 No Content` relevé à proximité concerne une requête de collecte distincte. | Les extraits ne relient pas la vérification de statut à une réinitialisation réussie avec le même jeton. La réponse de collecte ne renseigne pas sur la récupération. |
| Après un premier changement de mot de passe rapporté, la réouverture du lien a affiché la connexion et une erreur générique. Une réponse JSON avec `s: 0` et `e: 5` porte le même identifiant d’erreur que la capture. | La capture et le JSON documentent le même échec applicatif. Le motif précis et la requête associée au JSON restent inconnus. |
| Lors d’une réinitialisation suivante, l’historique Burp enregistre une réponse HTTP `200 OK` avec `{"s":1}`. | La réponse est cohérente avec le changement de mot de passe rapporté. L’erreur `e: 5` précède cette opération. |
| Un essai ultérieur dans Burp Repeater avec un ancien jeton et une requête modifiée a produit `200 OK` avec `s: 0`, `e: 8` et le message « An unspecified error occurred ». | La requête modifiée a échoué au niveau applicatif. Le message ne précise pas si le jeton a été consommé, s’il a expiré ou si un autre contrôle a motivé l’échec. La capture ne montre pas le chemin de cette requête. |

## Conclusion

**Aucune vulnérabilité confirmée.** Les éléments disponibles documentent une réinitialisation cohérente avec un succès et des échecs applicatifs lors de tentatives distinctes. L’invalidation après usage reste indéterminée. Une confirmation des journaux du service reliant le jeton à la réinitialisation réussie et explicitant son état permettrait de conclure.

La fiche repose sur les extraits HTTP et les captures fournis. Les jetons, cookies, données de compte, données cryptographiques et identifiants de diagnostic sont omis.
