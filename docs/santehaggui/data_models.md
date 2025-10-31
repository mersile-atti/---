# Modèles de données & schémas

## Schéma relationnel principal (PostgreSQL)

### Tables clés
- **users** `(id, phone, email, password_hash, locale, role, status, created_at, updated_at)`
- **user_profiles** `(user_id FK, first_name, last_name, gender, birth_date, national_id, address, emergency_contacts JSONB)`
- **households** `(id, name, region, created_by)`
- **household_members** `(household_id FK, user_id FK, relationship, permissions JSONB)`
- **patient_records** `(id, user_id FK, blood_type, allergies JSONB, chronic_conditions JSONB, last_sync_at)`
- **vital_signs** `(id, patient_id FK, recorded_by FK users, type, value, unit, recorded_at, source)`
- **medical_documents** `(id, patient_id FK, file_key, file_type, title, uploaded_by FK users, uploaded_at, checksum)`
- **medications** `(id, patient_id FK, name, dosage, frequency, start_date, end_date, prescribing_provider FK users)`
- **appointments** `(id, patient_id FK, provider_id FK users, facility_id FK, scheduled_at, location, status, channel)`
- **care_plans** `(id, patient_id FK, plan JSONB, created_by FK users, expires_at)`
- **consents** `(id, patient_id FK, grantee_id FK users/organizations, scope, granted_at, expires_at, revoked_at, hash)`
- **messages** `(id, thread_id FK, sender_id FK users, payload JSONB, message_type, sent_at, read_at)`
- **threads** `(id, subject, context_type, context_id, created_at)`
- **teleconsultations** `(id, appointment_id FK, room_id, status, started_at, ended_at, recording_url)`
- **facilities** `(id, name, type, address, latitude, longitude, services JSONB, tariffs JSONB)`
- **pharmacies** `(id, name, opening_hours JSONB, delivery_zones JSONB, on_call BOOLEAN)`
- **insurance_providers** `(id, name, contact_info JSONB, coverage_details JSONB)`
- **coverage_plans** `(id, provider_id FK, name, plan_details JSONB)`
- **patient_coverages** `(id, patient_id FK, coverage_plan_id FK, policy_number, valid_from, valid_to, status)`
- **audit_logs** `(id, actor_id FK users, action, resource_type, resource_id, metadata JSONB, created_at)`
- **otp_codes** `(id, user_id FK, channel, code_hash, expires_at, consumed_at, context)`
- **emergency_qr** `(id, patient_id FK, qr_code_url, last_regenerated_at, offline_payload JSONB)`

### Documents chiffrés (S3)
- `medical_documents.file_key` pointe vers un objet chiffré client-side.
- Versions signées et horodatées via Metadata (signature PGP du professionnel).

## Modèle FHIR exportable
- **Patient** : mapping depuis `users`, `user_profiles`, `patient_records`.
- **Encounter** : `appointments`, `teleconsultations`.
- **Observation** : `vital_signs`.
- **MedicationStatement** : `medications`.
- **CarePlan** : `care_plans`.
- **Consent** : `consents`.
- **Coverage** : `patient_coverages` + `coverage_plans`.
- **Organization** : `facilities`, `insurance_providers`.

## Données pour USSD/SMS
- Stockage simplifié des menus dans `ussd_sessions` `(session_id, user_identifier, state, payload JSONB, expires_at)`.
- `sms_queue` `(id, recipient, message, status, sent_at, retries)` pour suivis et escalades.

## Stratégies de confidentialité
- Données sensibles chiffrées via champs `pgcrypto` (`national_id`, `emergency_contacts`).
- Pseudonymisation pour reporting : tables matérialisées anonymisées alimentant le data warehouse.
- Suppression contrôlée : rétention configurable, logs immuables stockés séparément.

