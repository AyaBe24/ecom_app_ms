# Projet : Application E-commerce en Architecture Microservices

Ce projet est une application e-commerce complète, composée d’un backend basé sur une architecture microservices et d’une interface utilisateur frontend développée avec Angular.  
Il a été réalisé dans le cadre du cours JEE et Systèmes Avancés.

---

## Table des matières

1. [Architecture globale](#1-architecture-globale)  
2. [Stack technologique](#2-stack-technologique)  
3. [Guide de démarrage](#3-guide-de-démarrage)  
4. [Validation des composants de l’architecture](#4-validation-des-composants-de-larchitecture)  
5. [Présentation des endpoints et fonctionnalités](#5-présentation-des-endpoints-et-fonctionnalités)  


---

## 1. Architecture globale

L’application est constituée de plusieurs services indépendants qui collaborent entre eux :

- **config-server** : centralise la configuration de tous les services via un dépôt Git dédié.  
- **discovery-service** : annuaire des services (Eureka) permettant leur découverte dynamique.  
- **gateway-service** : point d’entrée unique (API Gateway) qui redirige les requêtes vers les services internes.  
- **customer-service, inventory-service, billing-service** : services métiers respectifs pour la gestion des clients, des produits et de la facturation.  

---

## 2. Stack technologique

- **Langage** : Java 21  
- **Frameworks** : Spring Boot, Spring Cloud (Config Server, Eureka, Gateway), Spring Data JPA/REST  
- **Communication inter-services** : OpenFeign  
- **Gestion des dépendances** : Maven  
- **Base de données** : H2 (en mémoire)  

---

## 3. Guide de démarrage

L’ordre recommandé pour lancer les services est :

1. discovery-service (port 8761)  
2. config-server (port 9999)  
3. inventory-service, customer-service  
4. gateway-service (port 8889)  
5. billing-service  

---

## 4. Validation des composants de l’architecture

### a. Configuration centralisée (Config Server)  
Le config-server (port 9999) fournit la configuration à chaque service via le dépôt Git. Par exemple, le customer-service charge cette configuration au démarrage, vérifiable via des endpoints de test affichant les paramètres globaux et spécifiques.

### b. Découverte de services (Eureka)  
Le tableau de bord Eureka (port 8761) montre que tous les services essentiels (CUSTOMER-SERVICE, GATEWAY-SERVICE, INVENTORY-SERVICE) sont bien enregistrés et en état UP.
<img width="1877" height="1440" alt="image" src="https://github.com/user-attachments/assets/13d1a37e-af99-47a7-bd7b-f482a947f1f2" />

---

## 5. Présentation des endpoints et fonctionnalités

### a. Endpoints des services individuels  
- Clients : `http://localhost:8081/customers`
  <img width="1004" height="902" alt="image" src="https://github.com/user-attachments/assets/cf6dbb80-7d09-4af3-aa83-115e99079e2e" />

- Produits : `http://localhost:8082/products`
  <img width="1043" height="895" alt="image" src="https://github.com/user-attachments/assets/3a039e73-4cdd-4cfb-b4e0-76e297415546" />
  

### b. Accès via l’API Gateway  
- Clients via gateway : `http://localhost:8889/CUSTOMER-SERVICE/customers`
  <img width="760" height="899" alt="image" src="https://github.com/user-attachments/assets/91d939b4-7be3-4fb2-82be-2488231d4e6d" />

- Produits via gateway : `http://localhost:8889/INVENTORY-SERVICE/products`
  <img width="866" height="901" alt="image" src="https://github.com/user-attachments/assets/c61a6564-1858-4319-b18e-4d7b84b45875" />


### d. Fonctionnalité avancée : Projections Spring Data REST  
Exemple de projection permettant de ne récupérer que l’email d’un client :  
`http://localhost:8081/customers/1?projection=email`
<img width="508" height="230" alt="image" src="https://github.com/user-attachments/assets/0b2d7014-0e07-43a6-8734-52f330cb18af" />


### e. Facture complète (orchestration inter-services)  
Le billing-service agrège les données des autres services pour construire une facture complète :  

- Liste des factures simples : `http://localhost:8889/BILLING-SERVICE/bills`
  <img width="711" height="982" alt="image" src="https://github.com/user-attachments/assets/f97dc058-b8e1-4d50-9efa-3ec3fd83382f" />
 - Détail d’une facture complète avec infos client et produits : 'http://localhost:8889/BILLING-SERVICE/bills/1'
   <img width="612" height="772" alt="image" src="https://github.com/user-attachments/assets/08f82480-0fc3-4bd9-b8bf-d76aa4ca9a34" />


---
