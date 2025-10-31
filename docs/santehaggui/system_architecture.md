# Architecture Système SanteHaggui

## Vue d'ensemble
La plateforme SanteHaggui repose sur une architecture micro-services légère autour d'un noyau Node.js/Express exposant des API REST sécurisées et extensibles vers GraphQL. Les clients mobiles, web et USSD interagissent avec ce backend via des passerelles adaptées à la connectivité et au type de terminal.

```
React Native / React Web / Portail Admin
          |            |            \
      API Gateway (Express, Rate limiting, Auth)
               |
        Services métier Node.js
  ├─ Service Dossiers & Consentements
  ├─ Service Messagerie Sécurisée (WebSocket + MQTT fallback)
  ├─ Service Téléconsultation (WebRTC + TURN)
  ├─ Service Agenda & Notifications (BullMQ + Redis)
  ├─ Service USSD/SMS (Webhook Africa's Talking/Twilio)
  ├─ Service Assurance & Facturation
  └─ Service Intégrations (FHIR/HL7, Mobile Money, Pharmacies)
               |
      Base de données principale (PostgreSQL) + Data Lake (S3-compatible chiffré)
               |
       Pipelines d'analyse et reporting (Metabase/Superset)
```

## Composants principaux
### Frontends
- **App mobile React Native** : offre carnet de santé, agenda, télémédecine, synchronisation hors-ligne via SQLite + WatermelonDB.
- **Web React.js** : tableau de bord professionnels, administration, édition des comptes-rendus.
- **Interface USSD/SMS** : menus dynamiques gérés par flows configurables, fallback SMS push/pull.

### Backend Node.js
- **Express API** : endpoints REST versionnés, documentation OpenAPI, validation via Zod/Joi.
- **Gestion des identités** : AuthN/AuthZ via JWT courts, refresh tokens stockés en Redis, OTP SMS/Email/WhatsApp, authentification des agents avec FIDO2 optionnel.
- **Consentements** : Smart contracts légers (hash des autorisations) stockés en PostgreSQL, audit trail immuable (append-only).
- **Fichier médical** : stockage structuré (PostgreSQL) + documents lourds chiffrés (S3 compatible type Wasabi/Supabase Storage).
- **Notifications** : BullMQ + Redis pour la planification, connecteurs SMS, push, email, appels vocaux.
- **Téléconsultation** : service WebRTC orchestré par Janus/LiveKit auto-hébergé, enregistrement chiffré.
- **Messagerie** : WebSocket + fallback MQTT (emqx) pour résilience, chiffrement de bout en bout côté client.

### Données & Sécurité
- **Base de données** : PostgreSQL partitionné par région, chiffré au repos (TDE) et en transit (TLS).
- **Cache & queues** : Redis pour OTP, sessions, files de tâches.
- **Observabilité** : Prometheus + Grafana, log management via ELK/Opensearch, alerting PagerDuty.
- **Sécurité** : rotation des clés, chiffrement AES-256 pour les données patient, KMS (HashiCorp Vault) pour secrets, RBAC basé sur attributs.

### Intégrations externes
- **Mobile Money** : Orange Money, MTN, Flooz pour paiements téléconsultations.
- **SMS/USSD** : Africa's Talking, Twilio ou Korba selon pays.
- **FHIR/HL7** : exporter les ressources Patient, Encounter, Observation, Medication, Coverage ; webhook vers systèmes hospitaliers.
- **Assurances** : APIs partenaires pour validation de couverture et reporting.
- **Cartographie** : OpenStreetMap + Mapbox Offline pour localisation de structures de santé.

## Résilience & Connectivité limitée
- Synchronisation delta : chaque modification locale génère un journal (CRDT léger) synchronisé dès que possible.
- Mode offline-first : caches chiffrés, workers de synchronisation, gestion des conflits par priorité médicale (plus récent + rôle autorisé).
- Compression des payloads (gzip/brotli) et pagination systématique.
- Support 2G/Edge : formats légers (USSD/SMS) avec codes courts, transmissions batch réduites.

## Déploiement & DevOps
- **Environnements** : Dev (Railway), Staging (Render), Production (multi-région Supabase/Railway ou Kubernetes géré).
- **CI/CD** : GitHub Actions, tests unitaires, lint, SAST (Semgrep), déploiement progressif.
- **Infrastructure as Code** : Terraform pour ressources cloud, Ansible pour configurations spécifiques.

## Gouvernance & Conformité
- Respect du RGPD, NDPR, lois locales sur les données de santé.
- Hébergement régionalisé, consentement explicite avant transfert transfrontalier.
- Journalisation des accès (who/when/why), export pour audit.
- Politique de gestion des incidents et plan de continuité (PCA/PRA) incluant sauvegardes chiffrées multi-sites.

