# Flots USSD & SMS SanteHaggui

## Principes de conception
- Menus courts (< 160 caractères) en français simplifié + langue locale configurable.
- Sessions USSD ≤ 3 niveaux pour limiter le temps et la latence.
- OTP vocal/SMS déclenché en cas d'actions sensibles (partage dossier, consentement).
- Fallback SMS push avec codes de commande (ex: `SH RDV 12345`).

## Menu USSD principal (`*123*45#`)
1. **1. Mon dossier**
   - 1: Voir prochains RDV
   - 2: Dernière ordonnance
   - 3: Carnet enfant (sélection membre)
2. **2. Agenda & Rappels**
   - 1: Confirmer prise médicament
   - 2: Demander rappel (heure personnalisée)
3. **3. Assistance**
   - 1: Contacter agent santé (callback)
   - 2: Urgence (envoi SMS aux contacts)
4. **4. Assurance**
   - 1: Vérifier couverture
   - 2: Demande prise en charge
5. **5. Paramètres**
   - 1: Changer langue
   - 2: Mettre à jour numéro aidant

## Flux SMS standard
- `SH HELP` → renvoie liste des commandes disponibles.
- `SH RDV` → retourne les trois prochains rendez-vous.
- `SH MED {code}` → confirme prise de médicament, déclenche mise à jour du plan de soins.
- `SH SOS` → envoie coordonnées et profil d'urgence aux contacts autorisés + centre d'appels.

## Gestion des agents communautaires
- Connexion via USSD sécurisé (PIN). Menu spécial :
  1. Enregistrer visite → saisie constantes (température, tension, poids)
  2. Noter symptômes → choix parmi listes configurables
  3. Synchroniser plus tard → stocke localement dans `ussd_sessions`
- Remontée d'indisponibilité réseau → envoi SMS à un numéro court pour traitement différé.

## Sécurité & Consentement
- Chaque accès dossier via USSD requiert OTP (SMS ou appel vocal) si données sensibles.
- Les logs d'accès sont réconciliés avec l'audit trail central.
- Codes de session aléatoires pour éviter le "session hijacking".

