# NeuraGreenMobile

Application mobile **Expo / React Native (JavaScript)** pour mesurer et visualiser l'impact environnemental des requêtes d'IA générative : énergie consommée, émissions de CO₂, consommation d'eau et coût, selon le modèle utilisé et le nombre de tokens.

## Objectif

NeuraGreenMobile aide à prendre conscience du coût environnemental et financier des usages d'IA. L'application estime l'impact de chaque requête, agrège ces métriques dans un tableau de bord, et alerte lorsque un budget carbone quotidien approche 80 % de sa limite.

## Fonctionnalités

- **Tableau de bord** : métriques dynamiques (énergie, CO₂, eau, coût) mises à jour en temps réel.
- **Moteur de calcul d'impact** : fonction `calculateImpact` qui estime l'impact par modèle et par nombre de tokens (coefficients de remplacement en attendant des données plus précises).
- **Gestion d'état globale** : store [Zustand](https://github.com/pmndrs/zustand) avec persistance locale.
- **Stockage local offline-first** : `@react-native-async-storage/async-storage`.
- **Gestion sécurisée des clés API** : clés OpenAI / Gemini stockées chiffrées sur l'appareil via `expo-secure-store`.
- **Appels API OpenAI** : la demande principale déclenche un appel API et alimente le flux d'activité.
- **Notifications locales « Eco-Budget »** : planifiées via `expo-notifications` lorsque 80 % du budget carbone est atteint (avec anti-spam).
- **Graphiques** : visualisation des émissions de CO₂ par modèle (composant `Co2Chart`).
- **Interactions** : retours haptiques (`expo-haptics`) et sons (`expo-av`).

## Stack technique

| Brique | Technologie |
| --- | --- |
| Framework | [Expo](https://expo.dev) SDK 54 |
| UI | React Native 0.81 |
| Langage | JavaScript (configuration TypeScript disponible) |
| Navigation | `expo-router` (onglets + stack) |
| État global | Zustand 5 |
| Stockage local | AsyncStorage |
| Stockage sécurisé | `expo-secure-store` |
| Notifications | `expo-notifications` |
| Graphiques | `victory-native` |
| Builds | EAS Build (`eas.json` : profils development / preview / production) |

## Contraintes connues

1. **État de ce dépôt** : le commit initial n'inclut que la configuration du projet (package.json, app.json, eas.json, tsconfig.json, etc.) et les notes de développement. Les dossiers sources de l'application (`app/`, `components/`, `store/`, `utils/`, `services/`, `assets/`) ne sont pas encore inclus ; ils seront ajoutés lors d'un prochain commit.
2. **MMKV et Expo Go** : historiquement, `react-native-mmkv` (module natif basé sur NitroModules) empêchait le lancement sous Expo Go. La migration vers `@react-native-async-storage/async-storage` a été réalisée pour retrouver la compatibilité Expo Go.
3. **Graphique CO₂** : le composant `Co2Chart` (`victory-native`) est temporairement désactivé (commenté) pour éviter des problèmes de compatibilité de types lors de la compilation sous Expo Go.
4. **Notifications avancées** : `expo-notifications` exige un **Development Build** (les notifications push ont été retirées d'Expo Go). Utiliser EAS Build (`eas build --profile development --platform android|ios`) puis `npx expo start --dev-client`.
5. **Avertissement de route** : le warning « Too many screens defined. Route "modal" is extraneous. » est mineur (route superflue à nettoyer).

## Démarrage

```bash
npm install
npx expo start
```

Puis au choix :

- scanner le QR code avec **Expo Go** (limité — voir contraintes) ;
- ouvrir sur un **émulateur** Android/iOS ;
- lancer en **web** : `npx expo start --web` ;
- utiliser un **Development Build** (recommandé pour les fonctionnalités natives) :

```bash
# installer EAS CLI si besoin
npm install -g eas-cli
eas build --profile development --platform android   # ou ios
npx expo start --dev-client
```

Autres scripts : `npm run android`, `npm run ios`, `npm run lint`.

## Licence

MIT © 2026 j0ltdevv — voir [LICENSE](LICENSE).
