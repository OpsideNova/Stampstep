# Stampstep pour Android

L'application Android est une enveloppe (Capacitor) autour de votre serveur Stampstep.
Elle affiche le même carnet, avec deux différences qui comptent :

- **le traceur GPS continue écran éteint**, grâce à un service de premier plan qu'Android
  signale par une notification persistante « Stampstep · traceur actif » ;
- l'icône et l'écran de démarrage aux couleurs de Stampstep.

Toutes les données restent sur votre serveur : mettre à jour le serveur met à jour
l'application, sans nouvel APK (sauf pour changer l'icône, les permissions ou l'adresse).

## Prérequis

Le téléphone doit joindre le serveur **en HTTPS**, depuis Internet ou le réseau local :
par exemple avec Cloudflare Tunnel (voir le README principal). Une adresse
`http://192.168…` ne suffit pas.

## Obtenir l'APK sans rien installer (GitHub, gratuit)

1. Créez un dépôt **privé** sur github.com et envoyez-y tout le dossier du projet
   (avec `mobile/` et `.github/`). Le plus simple : GitHub Desktop.
2. Dans le dépôt : **Settings → Secrets and variables → Actions → Variables →
   New repository variable**
   `STAMPSTEPS_URL` = `https://votre-adresse.ch`
3. Onglet **Actions → Application Android → Run workflow**. Compter 5 à 8 minutes.
4. En bas de la page du résultat, section **Artifacts** : téléchargez
   `StampSteps-android`, décompressez, vous avez `StampSteps.apk`.

Chaque modification poussée dans `mobile/` relance la compilation.

## Ou avec Android Studio

```
cd mobile
npm install
npx cap sync android
npx cap open android
```

Puis dans Android Studio : **Build → Build App Bundle(s) / APK(s) → Build APK(s)**.
Avant, remplacez `ADRESSE-DE-VOTRE-SERVEUR` dans `capacitor.config.json`.

## Installer sur le téléphone

Envoyez `StampSteps.apk` sur le téléphone (mail, câble, Drive), ouvrez-le, et
autorisez l'installation depuis cette source quand Android le demande.

Au premier démarrage du traceur, Android demande :
1. la position — choisir **« Lorsque vous utilisez l'appli »** suffit : le service de
   premier plan garde l'accès écran éteint ;
2. les notifications — les accepter, sinon le suivi en arrière-plan est interrompu
   plus vite par certains fabricants.

Sur les téléphones Samsung, Xiaomi, Huawei ou OnePlus, désactivez l'optimisation de
batterie pour Stampstep (Réglages → Applications → Stampstep → Batterie →
Non restreinte), sinon le système peut couper le traceur au bout de quelques minutes.

## Avant de publier sur le Play Store

L'APK produit ici est une version de **test**, signée avec une clé de développement :
parfaite pour vous et vos testeurs, refusée par le Play Store. Pour la boutique, il faudra
une clé de signature personnelle et un paquet AAB (`./gradlew bundleRelease`), un compte
développeur Google (25 $ une fois), et une justification de l'usage de la position en
arrière-plan dans la fiche de l'application. Google examine ce point de près.

## Réglages

| Fichier | Rôle |
|---|---|
| `capacitor.config.json` | adresse du serveur, identifiant `ch.stampsteps.app` |
| `assets/` | images sources de l'icône et de l'écran de démarrage (`npm run icones` pour régénérer) |
| `www/erreur.html` | page affichée quand le serveur ne répond pas |
| `android/app/src/main/res/values/strings.xml` | texte de la notification du traceur |
