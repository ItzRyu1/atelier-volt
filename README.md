# L'Atelier Volt

Le carnet d'entretien gratuit de ta trottinette électrique : interventions, rappels, suivi GPS,
navigation, itinéraires, classements (publics et privés), badges, bilan annuel.

> Projet privé. **Tous droits réservés** (voir [LICENSE](LICENSE)).

## Fonctionnalités

- **Carnet d'entretien** : interventions, coûts, rappels par km et par durée, assurance, carnet PDF, fiche de revente.
- **Trajets** : suivi GPS (courbe de vitesse, tracé), itinéraires (boucle ou A → B), navigation sans voix, partage par lien, météo, lieux utiles.
- **Communauté** : classements public (GPS / déclaré) et privés par invitation, signalement, objectifs mensuels, série hebdomadaire, badges, bilan annuel.
- **Compte** : email + mot de passe, Google, passkey, lien ou code par email, double authentification (TOTP), export et suppression des données (RGPD).
- **Administration** : panneau réservé (statistiques, signalements, avertissements, suspensions, messages), protégé par la double authentification.
- **Installable** sur l'écran d'accueil (PWA légère).

## Architecture

| Élément | Rôle |
|---|---|
| `index.html` | Tout le site (HTML, CSS, JavaScript), sans étape de build |
| `manifest.webmanifest`, `*.png` | Installation sur l'écran d'accueil |
| **Vercel** | Hébergement du site, déploiement automatique depuis ce dépôt |
| **Supabase** | Comptes, base de données (RLS), fonctions SQL, Edge Functions |
| **MailerSend** | Envoi des emails (SMTP pour les emails de compte, API pour les rappels et l'administration) |

Services tiers facultatifs (avec consentement) : OpenStreetMap, Photon, FOSSGIS (itinéraires), Open-Meteo, Overpass.

## Mise en place (résumé)

1. **Supabase** : lancer dans l'ordre `setup.sql`, `update.sql`, `update2.sql` (éditeur SQL).
2. **Identifiants** : renseigner l'URL du projet et la clé **publique** (anon / publishable) en haut du script de `index.html`, ainsi que les informations légales de l'éditeur.
3. **Edge Functions** (facultatif) : `send-reminders.ts` (rappels, avec `rappels-cron.sql`) et `admin-actions.ts` (administration).
4. **Emails** : modèles du dossier `emails/` à coller dans *Authentication → Emails → Templates*.
5. **Vercel** : importer ce dépôt, sans réglage particulier.

## Sécurité

- La clé publique (anon / publishable) est faite pour être visible. **Ne jamais** publier la clé `service_role`, le jeton MailerSend ni les secrets des fonctions.
- Toutes les tables ont la sécurité par ligne (RLS) : chaque compte ne voit que ses propres données.
- Les fonctions d'administration exigent un compte enregistré dans la table `admins` **et** une session avec double authentification.

## Licence

© 2026 MONTIGNAC MAXENCE. Tous droits réservés. Les composants tiers restent sous leurs licences
(voir la page « Licences et crédits » du site).
