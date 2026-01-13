# 🛡️ Opération NetCore : L'Art de la Guerre Défensive

### *Conception d'une infrastructure "Zero-Trust" & Lead Blue Team*

## 🌌 Vision du Projet

Dans un cyber-espace saturé de menaces, la résilience n'est pas une option, c'est une nécessité. Ce projet simule l'infrastructure critique de **NetCore Solutions**, une entreprise de distribution technologique.

En tant que **Chef de l'équipe Blue Team**, j'ai supervisé l'intégralité du cycle de vie sécuritaire : de la segmentation réseau initiale au durcissement des équipements face à une **Red Team** agressive, jusqu'à la validation finale par un audit de conformité.

---

## 🏗️ Architecture du Réseau : Segmentation & Zones de Confiance

L'infrastructure repose sur une séparation physique et logique stricte gérée par un pare-feu **Cisco ASA (v9.0.1)**.

| Zone | Niveau de Sécurité | Description |
| --- | --- | --- |
| **INSIDE** | 100 | Le sanctuaire : AD DS, Messagerie, Honeypots.
| **DMZ** | 70 | Zone tampon : Serveur Web public.
| **OUTSIDE** | 0 | Le périmètre non sécurisé (Internet / Agence).

<img width="634" height="318" alt="image" src="https://github.com/user-attachments/assets/1b11f51d-16c7-4eaa-8208-f20609fdd685" />

### 📍 Plan d'Adressage Stratégique

* 
**Siège (Inside) :** `192.168.4.0/24`.


* 
**DMZ :** `192.168.1.0/24`.


* 
**Agence Distante :** `192.168.3.0/24`.



---

## 🔒 Focus Technique : Le Tunnel IPsec Site-à-Site

Pour sécuriser les flux entre le siège et l'agence, j'ai déployé un tunnel VPN IPsec robuste sur les routeurs R1 et R2, utilisant un chiffrement de classe militaire .

**Configuration des Politiques IKEv1 (Extraits) :**

```cisco
! -- Configuration sur R1 (Siège) --
crypto isakmp policy 10
 encr aes 256              ! [cite_start]Chiffrement AES-256 bits [cite: 331]
 authentication pre-share  ! [cite_start]Authentification par clé partagée [cite: 332]
 group 14                  ! [cite_start]Groupe Diffie-Hellman 2048-bit [cite: 333]
 lifetime 3600             ! [cite_start]Renouvellement de clé toutes les heures [cite: 334]

crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac ! [cite_start]Intégrité des données [cite: 339]

! -- Application via Crypto Map --
crypto map CMAP 10 ipsec-isakmp
 set peer 192.168.2.2      ! [cite_start]Peer R2 [cite: 345]
 match address 101         ! [cite_start]Flux autorisés (ACL 101) [cite: 349]

```

---

## 🍯 Stratégie de Déception : L'Usage des Honeypots

Au-delà de la défense passive, j'ai instauré une défense active au sein de la zone **INSIDE**.

* 
**Déploiement :** Deux serveurs "leurres" (**HONEYPOT1 : 192.168.4.5** et **HONEYPOT2 : 192.168.4.2**) ont été configurés.


* **Objectif :** Détecter toute intrusion ayant franchi le périmètre ASA. Ces cibles faciles sont monitorées pour alerter l'équipe en cas de tentative de scan ou de connexion SSH non autorisée.



---

## 🛡️ Hardening : Le Durcissement de la Couche 2

Un réseau est aussi faible que son maillon le plus bas. Nous avons sécurisé les commutateurs pour empêcher les attaques de proximité.

* 
**Port-Security :** Limitation à 5 adresses MAC par port avec apprentissage dynamique (*sticky*) pour bloquer tout branchement d'équipement pirate .


* 
**BPDU Guard :** Désactivation automatique du port si un switch non autorisé est détecté (prévention de l'usurpation de Root Bridge).


* 
**Storm Control :** Limitation du trafic broadcast à 50% pour prévenir le déni de service (DoS).


* 
**Désactivation des services :** HTTP, Telnet et CDP ont été désactivés globalement pour réduire la surface d'attaque .



---

## 📜 Politiques & Gouvernance (Blue Team Playbook)

La technique sans processus n'est rien. Nous avons rédigé et appliqué 6 politiques majeures :

1. 
**Filtrage ASA :** Refus par défaut (*Deny Any*).


2. 
**Principe du Moindre Privilège :** Accès administratifs via SSH v2 uniquement.


3. 
**Gestion des Identités :** Mots de passe complexes (10+ caractères, symboles) via Active Directory.


4. 
**Cycle de Patch :** Revue mensuelle et application sous 24h pour les failles critiques .


5. 
**Sauvegarde & Restauration :** Tests hebdomadaires de restauration des serveurs critiques .


6. 
**Réponse aux Incidents :** Protocole formel d'isolation et d'analyse post-mortem .



---

## 📺 Vitrine : Interface NetCore Solution

Le projet intègre une interface web dynamique pour la boutique de solutions informatiques de NetCore. Elle permet de gérer un catalogue d'équipements (MikroTik, pfSense, Cisco) et de simuler le flux transactionnel entre les zones DMZ et Inside.

> **Regarder la présentation vidéo :** [Lien vers WhatsApp Video 2026-01-12 at 11.18.01.mp4]

---

## 🧪 L'Épreuve du Feu : Red Team vs Blue Team

Le projet s'est conclu par une simulation d'attaque réelle :

* 
**Phase 1 (Reconnaissance) :** Tentatives de scans furtifs bloqués par l'ASA et journalisés.


* 
**Phase 2 (Intrusion) :** Simulation d'une compromission DMZ ; l'attaquant a été stoppé par l'étanchéité DMZ/Inside.


* 
**Phase 3 (Mouvement Latéral) :** Les Honeypots ont permis de lever une alerte immédiate lors de la tentative de scan interne.

