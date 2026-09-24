# Timer à tronçons

Un minuteur découpé en tronçons successifs, avec un checkpoint entre chaque tronçon. Tout tient dans un seul fichier HTML, sans dépendance ni installation, et fonctionne hors connexion.

## Lancer l'application

Ouvre `index.html` dans ton navigateur (double-clic sur le fichier). Aucun serveur n'est nécessaire.

Si tu préfères le servir en local, par exemple pour y accéder depuis un autre appareil du réseau :

```bash
python3 -m http.server 8000
```

puis ouvre `http://localhost:8000/timer-troncons.html`.

## Utilisation

1. Dans la section **Tronçons**, règle le nombre de tronçons avec « Ajouter un tronçon » et le bouton × de chaque ligne.
2. Renomme chaque tronçon si tu le souhaites et saisis sa durée.
3. Clique sur **Démarrer**. Quand un tronçon se termine, le chrono passe brièvement en couleur d'accent et le tronçon suivant démarre automatiquement.
4. À la fin du dernier tronçon, l'écran affiche « Terminé » et la durée réelle totale.

### Formats de durée acceptés

| Saisie    | Durée               |
|-----------|---------------------|
| `1:30`    | 1 minute 30         |
| `90`      | 90 secondes         |
| `1m30`    | 1 minute 30         |
| `4m`      | 4 minutes           |
| `1:05:00` | 1 heure 5 minutes   |
| `1h5m`    | 1 heure 5 minutes   |

La saisie est normalisée au format `m:ss` quand tu quittes le champ. Une durée invalide est signalée par un contour coloré et n'est pas prise en compte.

### Contrôles

- **Démarrer / Pause / Reprendre** : lance ou suspend le timer. La touche Espace fait la même chose.
- **Tronçon suivant** : valide le checkpoint immédiatement et passe au tronçon suivant.
- **Réinitialiser** : remet le timer à zéro en conservant la configuration des tronçons.

Pendant le décompte, le tronçon en cours et ceux déjà passés sont verrouillés. Les tronçons à venir restent modifiables.

### Informations affichées

- Le nom et la position du tronçon en cours (par exemple « Tronçon 2 · 2 sur 3 »).
- Le temps restant du tronçon, en grand.
- Le temps restant au total.
- Une barre segmentée, où chaque segment est proportionnel à la durée de son tronçon.
- Un journal des checkpoints avec le temps écoulé au moment de chaque passage.
- Le temps restant dans le titre de l'onglet, visible quand la page est en arrière-plan.

## Choix de conception

- **Aucun son.** Les passages de checkpoint sont uniquement visuels.
- **Interface épurée**, avec un mode sombre qui suit le réglage du système et le respect de la préférence « réduire les animations ».
- **Précision du décompte.** Le temps est calculé à partir d'horodatages (`performance.now()`) plutôt qu'en comptant des intervalles, ce qui évite la dérive. Si l'onglet est mis en veille par le navigateur, le temps continue d'être compté correctement et l'affichage se recale au retour.

## Limites

- La configuration n'est pas enregistrée : recharger la page rétablit les trois tronçons par défaut (1:30, 4:00, 2:00).
- Pas de notification système en fin de tronçon ; il faut garder un œil sur l'écran ou sur le titre de l'onglet.

## Personnalisation

Tout se trouve dans `timer-troncons.html` :

- **Tronçons par défaut** : tableau `segments` au début du script.
- **Couleurs et polices** : variables CSS dans `:root` (et dans le bloc `prefers-color-scheme: dark` pour le mode sombre).
- **Durée du flash de checkpoint** : valeur `900` (en millisecondes) dans la fonction `flash()`.

## Compatibilité

Tout navigateur récent : Chrome, Edge, Firefox, Safari, sur ordinateur comme sur mobile.
