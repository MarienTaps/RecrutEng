# RecrutEng
```markdown
# 🚀 Recrut'Eng — Plateforme Fullstack de Recrutement pour Ingénieurs

![Java](https://img.shields.io/badge/Java-21-orange?style=for-the-badge&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3-brightgreen?style=for-the-badge&logo=springboot)
![Angular](https://img.shields.io/badge/Angular-18-red?style=for-the-badge&logo=angular)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-blue?style=for-the-badge&logo=postgresql)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Recrut'Eng** est une application web SaaS conçue pour simplifier la mise en relation entre des **ingénieurs / consultants indépendants** et des **recruteurs / entreprises**. 

Ce projet sert d'application fil rouge d'ingénierie logicielle couvrant la modélisation relationnelle de données (JPA Joined Inheritance), le développement d'APIs REST sécurisées (Spring Security + JWT), une interface utilisateur dynamique réactive (Angular 18 Standalone & Signals), et la conteneurisation locale (Docker Compose).

---

## 📋 Table des Matières

- [Fonctionnalités Clés](#-fonctionnalités-clés)
- [Architecture & Stack Technique](#-architecture--stack-technique)
- [Modèle de Données (MLCD)](#-modèle-de-données-mlcd)
- [Prérequis](#-prérequis)
- [Démarrage Rapide (Docker Compose)](#-démarrage-rapide-docker-compose)
- [Installation Manuel (Développement)](#-installation-manuel-développement)
- [Endpoints API REST](#-endpoints-api-rest)
- [Structure du Projet](#-structure-du-projet)
- [Contribution & Licence](#-contribution--licence)

---

## ✨ Fonctionnalités Clés

### 🔐 Authentification & Sécurité
- Inscription et connexion sécurisées avec attribution explicite d'un rôle (`ROLE_CONSULTANT` ou `ROLE_RECRUTEUR`).
- Gestion de session **stateless** basée sur des jetons **JSON Web Tokens (JWT)**.
- Protection des routes API via Spring Security et des pages front-end via les Guards Angular.

### 👤 Espace Consultant
- **Gestion de Profil :** Mise à jour du Taux Journalier Moyen (TJM), du CV, et du statut de disponibilité dynamique (`Disponible` / `Indisponible`).
- **Gestion des Compétences :** Association de compétences techniques répertoriées avec les années d'expérience associées.
- **Suivi des Candidatures :** Tableau de bord pour suivre l'état des postulations (`EN_ATTENTE`, `ENTRETIEN`, `VALIDÉE`, `REFUSEE`).

### 👔 Espace Recruteur
- **Publication d'Offres :** Création, modification et fermeture de demandes de missions.
- **Moteur de Sourcing :** Recherche dynamique multicritère de profils d'ingénieurs disponibles par compétence et TJM.
- **Gestion des Candidats :** Consultation des profils des postulants et gestion du workflow de validation des candidatures.

---

## 🛠️ Architecture & Stack Technique

### Backend
- **Langage :** Java 21 (LTS)
- **Framework :** Spring Boot 3.3+ (Spring Data JPA, Spring Security, Spring Web)
- **Sécurité :** JWT (jjwt) & BCrypt Password Encoding
- **Base de Données :** PostgreSQL 16+
- **Build Tool :** Maven 3.9+

### Frontend
- **Framework :** Angular 18 (Standalone Components, Signals, RxJS)
- **UI & Styles :** Angular Material & Tailwind CSS
- **HTTP Client :** HttpClient avec Intercepteur JWT réactif

### DevOps & Outils
- **Conteneurisation :** Docker & Docker Compose
- **Versionning :** Git & GitHub Workflow

---

## 🗄️ Modèle de Données (MLCD)

Le projet s'appuie sur une stratégie d'héritage JPA `@Inheritance(strategy = InheritanceType.JOINED)` :

- **`UTILISATEUR`** *(Abstract)* `(id, email, mot_de_passe, role, creation_date)`
  - ├── **`CONSULTANT`** `(utilisateur_id [FK], nom, prenom, tjm, disponibilite, cv_url)`
  - └── **`RECRUTEUR`** `(utilisateur_id [FK], nom, prenom, entreprise_nom)`
- **`COMPETENCE`** `(id, libelle)`
- **`CONSULTANT_COMPETENCE`** `(consultant_id [FK], competence_id [FK], annees_experience, date_obtention)`
- **`MISSION`** `(id, titre, description, date_debut, statut, recruteur_id [FK])`
- **`CANDIDATURE`** `(id, mission_id [FK], consultant_id [FK], date_postulation, statut_candidature)`

---

## ⚙️ Prérequis

Avant de commencer, assurez-vous de disposer de :
- [Git](https://git-scm.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) *(pour l'exécution conteneurisée)*
- [JDK 21](https://www.oracle.com/java/technologies/downloads/#java21) *(pour le dev local)*
- [Node.js 20+ & npm](https://nodejs.org/) *(pour le dev local)*

---
🌐 Frontend Angular : http://localhost:4200

⚙️ Backend Spring Boot API : http://localhost:8080/api/v1

🗄️ Base de données PostgreSQL : localhost:5432 (db: recruteng_db)

Installation Manuel (Développement Local)

1. Base de Données (PostgreSQL)
Démarrez un conteneur PostgreSQL local :
Bash
docker run --name postgres-recruteng -e POSTGRES_DB=recruteng_db -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:16
2. Backend (Spring Boot)
Bash

cd backend
# Compiler et lancer l'application
./mvnw spring-boot:run
L'API sera disponible sur http://localhost:8080.

3. Frontend (Angular 18)

Bash
cd frontend
# Installer les dépendances
npm install

# Lancer le serveur de développement Angular
ng serve
Accédez à l'application sur http://localhost:4200

Endpoints API REST (Principaux)

Auth (/api/v1/auth)
Méthode	| Endpoint	  | Description                                |	Accès
POST	  |register     | consultant	Inscription profil Consultant  |	Public
POST	  | register    | recruteur	Inscription profil Recruteur	   |  Public
POST	  | login	      | Authentification & génération JWT	         |  Public

Consultants (/api/v1/consultants)
Méthode	Endpoint	Description	Accès
GET	/	Liste des consultants disponibles	Recruteur
GET	/recherche?skill=...	Filtrer par compétence	Recruteur
GET	/me	Consulter mon profil	Consultant
PUT	/me	Mettre à jour mon profil	Consultan


Missions (/api/v1/missions)
Méthode	Endpoint	Description	Accès
GET	/	Liste des missions ouvertes	Authentifié
POST	/	Créer une nouvelle mission	Recruteur
GET	/recruteur/mes-missions	Consulter mes offres	Recruteur


Candidatures (/api/v1/candidatures)
Méthode	Endpoint	Description	Accès
POST	/	Postuler à une mission	Consultant
GET	/consultant/me	Consulter mes candidatures	Consultant
PATCH	/{id}/statut	Modifier le statut d'une candidature	Recruteur

recruteng/
├── docker-compose.yml          # Chef d'orchestre Docker
├── README.md                   # Documentation du projet
├── backend/                    # Application Spring Boot
│   ├── src/main/java/com/recruteng/
│   │   ├── config/             # Spring Security & JWT Config
│   │   ├── controller/         # API REST Endpoints
│   │   ├── dto/                # Data Transfer Objects
│   │   ├── model/              # JPA Entities (Utilisateur, Consultant, Mission...)
│   │   ├── repository/         # Spring Data JPA Repositories
│   │   └── service/            # Business Logic Layer
│   └── pom.xml
└── frontend/                   # Application Angular 18
    ├── src/app/
    │   ├── core/               # Guards, Interceptors, Services
    │   ├── features/           # Modules métier (Auth, Consultant, Recruteur, Missions)
    │   └── shared/             # Composants réutilisables & Material
    └── angular.json





