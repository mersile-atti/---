# Design API & Services SanteHaggui

## Principes
- RESTful versionné (`/api/v1`), prêt pour GraphQL (fédération Apollo) et streaming SSE pour notifications légères.
- Authentification via JWT d'accès (15 min) + refresh tokens (rotating) + OTP initial.
- Autorisation basée sur rôles et attributs (patients, aidants, professionnels, agents, admin, assurance).
- Toutes les réponses incluent `request_id`, `locale`, `sync_token` pour clients offline.

## Endpoints clés
### Authentification
- `POST /api/v1/auth/request-otp` (body: phone/email, channel)
- `POST /api/v1/auth/verify-otp` → renvoie tokens + état du profil
- `POST /api/v1/auth/refresh`
- `POST /api/v1/auth/logout`

### Gestion des dossiers
- `GET /api/v1/patients/me` → dossier du patient connecté
- `GET /api/v1/patients/:id` (RBAC + consentement)
- `POST /api/v1/patients` (création membre famille)
- `PATCH /api/v1/patients/:id`
- `GET /api/v1/patients/:id/vitals`
- `POST /api/v1/patients/:id/vitals`
- `POST /api/v1/patients/:id/documents`
- `GET /api/v1/patients/:id/care-plans`

### Agenda & Rappels
- `GET /api/v1/appointments`
- `POST /api/v1/appointments`
- `PATCH /api/v1/appointments/:id`
- `POST /api/v1/appointments/:id/confirm`
- `POST /api/v1/medication-adherence`

### Messagerie
- `GET /api/v1/threads`
- `POST /api/v1/threads`
- `POST /api/v1/threads/:id/messages`
- WebSocket `/ws/messages` avec auth token.

### Téléconsultation
- `POST /api/v1/teleconsultations` (planification)
- `POST /api/v1/teleconsultations/:id/start`
- `POST /api/v1/teleconsultations/:id/end`
- `GET /api/v1/teleconsultations/:id/session-token`

### Consentements
- `GET /api/v1/consents`
- `POST /api/v1/consents`
- `PATCH /api/v1/consents/:id/revoke`
- `GET /api/v1/consents/:id/audit`

### Assurance & Facturation
- `GET /api/v1/insurance/coverages`
- `POST /api/v1/insurance/claims`
- `GET /api/v1/insurance/claims/:id`

### Localisation & Pharmacies
- `GET /api/v1/facilities?lat=..&lng=..&radius=..`
- `GET /api/v1/pharmacies/on-call`
- `POST /api/v1/pharmacies/:id/orders`

### USSD/SMS Webhooks
- `POST /api/v1/channels/ussd` (validé par signature Africa's Talking)
- `POST /api/v1/channels/sms`
- `POST /api/v1/channels/voice`

### Export FHIR
- `GET /api/v1/fhir/Patient/:id`
- `GET /api/v1/fhir/$bulk-export?resourceType=Patient,Observation`
- Webhook `POST /api/v1/integrations/fhir/subscriptions`

## Notifications & Agenda
- Worker BullMQ `agenda:reminders` → envoi multi-canaux.
- Worker `medication:adherence` → suivi et escalade.
- Worker `sync:offline` → traite journaux CRDT.

## Contrôles de sécurité
- Rate limiting basé sur IP et device fingerprint.
- Hachage OTP (bcrypt) + expiration 5 minutes, throttling 5 tentatives.
- Journalisation `audit_logs` pour chaque endpoint critique.
- Signatures HMAC des payloads sortants (pharmacies, assurances).

