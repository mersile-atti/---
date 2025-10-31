# SanteHaggui Product Blueprint

## Vision and Context
SanteHaggui ("Ma Santé" en arabe local) est une plateforme de dossier de santé personnel, partagé et connecté qui accompagne les familles, aidants et professionnels de santé, y compris en zones rurales à connectivité limitée. La solution combine des canaux mobiles, web et SMS/USSD afin d'offrir un suivi médical continu, sécurisé et inclusif dans les écosystèmes de soins africains.

### Principes directeurs
- **Inclusivité multicanal** : accès par application mobile React Native, interface web React.js, portail USSD/SMS et agents communautaires.
- **Continuité des soins** : carnet de santé numérique hors-ligne synchronisé, messagerie sécurisée, téléconsultations et partage contrôlé des données.
- **Interopérabilité et standards** : export FHIR, compatibilité HL7, intégration avec réseaux d'assurances et pharmacies.
- **Sécurité & consentement** : chiffrement des données, gestion des rôles, OTP multi-canaux, traçabilité des accès et consentements explicites.

## Personas cibles
| Persona | Besoins principaux | Canal privilégié |
|---------|-------------------|------------------|
| Parent en zone rurale | Carnet vaccinal de la famille, rappels d'agenda, accès hors-ligne | Application mobile + SMS |
| Aidant d'EHPAD | Consultation rapide des dossiers, alertes, partage avec médecins | Web + tablette |
| Médecin de clinique urbaine | Historique patient, messagerie sécurisée, téléconsultation | Web |
| Agent de santé communautaire | Remplir carnets numériques lors des visites à domicile | USSD/Android léger |
| Pharmacien | Vérifier ordonnances, gérer stocks et livraisons | Web |
| Compagnie d'assurance | Accès aux données sous consentement, gestion tiers-payant | Web + API |

## Fonctionnalités majeures
### Patients & Aidants
- **Agenda santé intelligent** : rappels de traitements, vaccins, visites préventives, notifications multilingues (FR/AR/langues locales) via push/SMS.
- **Dossier santé personnel** : ordonnances, constantes, résultats, documents (PDF/Images), capture par smartphone, indexation et taggage.
- **Accès d'urgence** : QR code hors-ligne contenant informations critiques (groupe sanguin, allergies, contacts), actualisé automatiquement.
- **Télé-échanges** : messagerie sécurisée chiffrée E2E, envoi de photos/document, triage automatisé pour aiguillage.
- **Carnet de santé numérique** : disponible en cache local chiffré, synchronisation delta dès retour de connectivité.
- **Authentification OTP** : connexion via numéro de téléphone ou email, OTP SMS, voix ou WhatsApp fallback, support PIN secondaire.

### Professionnels & Établissements
- **Tableau de bord patient** : timeline clinique, constantes vitales, alertes, courbes graphiques.
- **Messagerie sécurisée** : canaux individuels, groupes pluridisciplinaires, archivage légal.
- **Consentements numériques** : gestion des mandats, durée limitée, journal d'audit consultable.
- **Comptes-rendus et ordonnances** : éditeur structuré, modèles standards, signature numérique.
- **Export FHIR/HL7** : sélection de ressources, export JSON/XML/CSV, webhooks pour intégrations.
- **Module assurance** : gestion des couvertures, validation des prises en charge, facturation tiers-payant.

### Fonctionnalités inspirées du Togo
- **Téléconsultation** : planification, paiement mobile money, salle virtuelle, notes partagées.
- **Localisation pharmacies de garde** : carte et liste offline, commande en ligne, suivi de livraison.
- **Accès SMS/USSD** : vérification de rendez-vous, ajout de constantes, demandes d'urgence via menus courts.
- **Agents communautaires** : application terrain avec formulaires simplifiés, synchronisation quand connectivité disponible.
- **Localisation hôpitaux/labos** : par distance, disponibilité des services, tarifs indicatifs.
- **Module assurance** : réseau de prestataires, numérisation des cartes, suivi des remboursements.

## Parcours utilisateurs clés
1. **Création de dossier familial** : inscription via OTP → ajout des membres → import carnet papier → partage avec médecin.
2. **Téléconsultation** : prise de RDV via app ou USSD → paiement mobile money → consultation vidéo/audio → prescription envoyée → commande pharmacie.
3. **Visite agent communautaire** : agent se connecte en offline → remplit constantes → synchronise dès couverture réseau → notifications envoyées aux médecins.
4. **Situation d'urgence** : secouriste scanne QR → accède aux informations critiques (mode lecture seule) → envoie alerte aux contacts d'urgence.

## Accessibilité & Langues
- Interface multilingue (Français, Arabe, Ewe/Kabyè selon régions) avec bascule facile.
- Mode contraste élevé, taille de police ajustable, voix de synthèse pour SMS vocaux.
- Tutoriels audio et visuels, assistance communautaire et call-center intégré.

## Indicateurs clés de succès
- Taux d'adoption des familles en zone rurale.
- Taux de synchronisation des carnets offline.
- Nombre de téléconsultations réussies par mois.
- Satisfaction des professionnels (NPS) et temps de réponse moyen.
- Conformité aux référentiels de protection des données (RGPD, lois locales).

