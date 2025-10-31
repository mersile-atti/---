# Sécurité, Confidentialité & Conformité

## Cadre réglementaire
- Respect du RGPD pour traitements de données personnelles avec minimisation et droits d'accès.
- Conformité aux directives nationales (ex: loi togolaise sur la protection des données, NDPR Nigeria si extension régionale).
- Accord explicite patient pour partage transfrontalier des données (data residency).

## Gouvernance des données
- Politique de classification (Public, Interne, Sensible, Critique).
- Délégué à la protection des données (DPO) responsable audit & formation.
- Registre des traitements et DPIA pour chaque nouvelle fonctionnalité sensible.

## Protection technique
- Chiffrement TLS 1.3 systématique, HSTS, CSP strict.
- Données au repos chiffrées (PostgreSQL TDE, S3 SSE-KMS), clés gérées via HashiCorp Vault.
- Chiffrement côté client (E2EE) pour messagerie et documents sensibles.
- Gestion des secrets via Vault + rotation automatique.
- Authentification multi-facteurs pour personnels sensibles (médecins, administrateurs).
- Ségrégation des environnements (prod/staging/dev) et réseau Zero Trust (WireGuard).

## Gestion des consentements
- UI claire affichant portée, durée, acteurs impliqués.
- Journal d'audit consultable, exportable en PDF signé.
- Mécanisme de révocation instantané, propagation aux services partenaires.
- Notifications en temps réel aux patients lors d'un accès professionnel.

## Protection contre les attaques
- WAF (Cloudflare/AWS) et IDS/IPS.
- Détection comportementale (accès anormal, tentatives OTP multiples).
- Sauvegardes quotidiennes chiffrées, test de restauration trimestriel.
- Programme de sensibilisation utilisateurs (phishing, protection terminaux).

## Continuité & Résilience
- Redondance multi-région (actif-passif) avec failover automatisé.
- Plan de reprise (RTO 4h, RPO 15 min).
- Monitoring 24/7, astreinte locale, canaux de communication d'urgence.

