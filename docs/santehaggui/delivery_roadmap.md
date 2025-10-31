# Roadmap de livraison SanteHaggui

## Phase 0 – Préparation (M0-M1)
- Études terrain (patients, agents, médecins, assurances) et cadrage réglementaire.
- Définition MVP, charte d'expérience multilingue, maquettes UX.
- Mise en place CI/CD, pipelines de sécurité, environnement Dev.

## Phase 1 – MVP Famille & Agents (M2-M4)
- Authentification OTP, gestion des profils familiaux, carnet de santé numérique offline-first.
- Agenda santé (rappels traitements/vaccins) + notifications SMS.
- Interface USSD de base (consultation rendez-vous, confirmations prise médicaments).
- Tableau de bord agent communautaire (collecte constantes, synchronisation différée).
- QR code d'urgence hors-ligne.

## Phase 2 – Professionnels & Messagerie (M4-M6)
- Tableau de bord web pour professionnels (dossier patient, constantes, documents).
- Messagerie sécurisée E2E, gestion des threads et pièces jointes.
- Consentements numériques et journal d'audit complet.
- Intégration pharmacies de garde (localisation + disponibilité).

## Phase 3 – Téléconsultation & Assurance (M6-M8)
- Planification et réalisation de téléconsultations (WebRTC, paiements mobile money).
- Module assurance : vérification couverture, demandes de prise en charge.
- Export FHIR (Patient, Observation, Encounter) et API partenaires.

## Phase 4 – Optimisation & Scalabilité (M8-M12)
- Analytics et tableaux de bord santé publique (données anonymisées).
- IA légère pour détection d'anomalies (observations critiques).
- Extension multirégion, réplication des données et déploiement multi-cloud.
- Renforcement sécurité (FIDO2, DLP, bug bounty local).

## Indicateurs par phase
- **Phase 1** : ≥ 5 000 patients actifs, 95% synchronisation offline réussie.
- **Phase 2** : ≥ 200 professionnels actifs, SLA messagerie 99%.
- **Phase 3** : ≥ 1 000 téléconsultations/mois, 90% satisfaction.
- **Phase 4** : Temps de réponse < 500 ms, disponibilité 99.9%.

