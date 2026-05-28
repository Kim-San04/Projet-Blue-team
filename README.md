<div align="center">

![Header](https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,100:00f2ff&height=120&section=header&text=&fontSize=0)

</div>

# Opération NetCore : L'Art de la Guerre Défensive

<div align="center">

![Cisco ASA](https://img.shields.io/badge/Cisco_ASA-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white) ![Active Directory](https://img.shields.io/badge/Active_Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white) ![VPN IPsec](https://img.shields.io/badge/VPN_IPsec-FF6600?style=for-the-badge&logo=wireguard&logoColor=white) ![Blue Team](https://img.shields.io/badge/Blue_Team-38BDF8?style=for-the-badge&logo=shield&logoColor=white) ![Zero Trust](https://img.shields.io/badge/Zero_Trust-00FF41?style=for-the-badge&logo=security&logoColor=black)

![Efrei](https://img.shields.io/badge/Efrei_Bordeaux-Mastère_Cybersécurité-purple?style=flat-square) ![Status](https://img.shields.io/badge/Status-Terminé-success?style=flat-square)

</div>

## 📋 Table des Matières
- [🌌 Vision du Projet](#-vision-du-projet)
- [🏗️ Architecture Réseau](#️-architecture-du-réseau--segmentation--zones-de-confiance)
- [🔒 Tunnel IPsec](#-focus-technique--le-tunnel-ipsec-site-à-site)
- [🍯 Honeypots](#-stratégie-de-déception--lusage-des-honeypots)
- [🛡️ Hardening](#️-hardening--le-durcissement-de-la-couche-2)
- [📜 Gouvernance](#-politiques--gouvernance-blue-team-playbook)
- [🧪 Simulation Red vs Blue](#-lépreuve-du-feu--red-team-vs-blue-team)


### *Conception d'une infrastructure "Zero-Trust" & Lead Blue Team*

## ð Vision du Projet

Dans un cyber-espace saturÃ© de menaces, la rÃ©silience n'est pas une option, c'est une nÃ©cessitÃ©. Ce projet simule l'infrastructure critique de **NetCore Solutions**, une entreprise de distribution technologique.

En tant que **Chef de l'Ã©quipe Blue Team**, j'ai supervisÃ© l'intÃ©gralitÃ© du cycle de vie sÃ©curitaire : de la segmentation rÃ©seau initiale au durcissement des Ã©quipements face Ã  une **Red Team** agressive, jusqu'Ã  la validation finale par un audit de conformitÃ©.

---

## ðï¸ Architecture du RÃ©seau : Segmentation & Zones de Confiance

L'infrastructure repose sur une sÃ©paration physique et logique stricte gÃ©rÃ©e par un pare-feu **Cisco ASA (v9.0.1)**.

| Zone | Niveau de SÃ©curitÃ© | Description |
| --- | --- | --- |
| **INSIDE** | 100 | Le sanctuaire : AD DS, Messagerie, Honeypots.
| **DMZ** | 70 | Zone tampon : Serveur Web public.
| **OUTSIDE** | 0 | Le pÃ©rimÃ¨tre non sÃ©curisÃ© (Internet / Agence).

<img width="634" height="318" alt="image" src="https://github.com/user-attachments/assets/1b11f51d-16c7-4eaa-8208-f20609fdd685" />

### ð Plan d'Adressage StratÃ©gique

* **SiÃ¨ge (Inside) :** `192.168.4.0/24`.


* **DMZ :** `192.168.1.0/24`.


* **Agence Distante :** `192.168.3.0/24`.


---

## ð Focus Technique : Le Tunnel IPsec Site-Ã -Site

Pour sÃ©curiser les flux entre le siÃ¨ge et l'agence, j'ai dÃ©ployÃ© un tunnel VPN IPsec robuste sur les routeurs R1 et R2, utilisant un chiffrement de classe militaire .

**Configuration des Politiques IKEv1 (Extraits) :**

```cisco
! -- Configuration sur R1 (SiÃ¨ge) --
crypto isakmp policy 10
 encr aes 256              ! [cite_start]Chiffrement AES-256 bits [cite: 331]
 authentication pre-share  ! [cite_start]Authentification par clÃ© partagÃ©e [cite: 332]
 group 14                  ! [cite_start]Groupe Diffie-Hellman 2048-bit [cite: 333]
 lifetime 3600             ! [cite_start]Renouvellement de clÃ© toutes les heures [cite: 334]

crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac ! [cite_start]IntÃ©gritÃ© des donnÃ©es [cite: 339]

! -- Application via Crypto Map --
crypto map CMAP 10 ipsec-isakmp
 set peer 192.168.2.2      ! [cite_start]Peer R2 [cite: 345]
 match address 101         ! [cite_start]Flux autorisÃ©s (ACL 101) [cite: 349]

```

---

## ð¯ StratÃ©gie de DÃ©ception : L'Usage des Honeypots

Au-delÃ  de la dÃ©fense passive, j'ai instaurÃ© une dÃ©fense active au sein de la zone **INSIDE**.

* **DÃ©ploiement :** Deux serveurs "leurres" (**HONEYPOT1 : 192.168.4.5** et **HONEYPOT2 : 192.168.4.2**) ont Ã©tÃ© configurÃ©s.


* **Objectif :** DÃ©tecter toute intrusion ayant franchi le pÃ©rimÃ¨tre ASA. Ces cibles faciles sont monitorÃ©es pour alerter l'Ã©quipe en cas de tentative de scan ou de connexion SSH non autorisÃ©e.



---

## ð¡ï¸ Hardening : Le Durcissement de la Couche 2

Un rÃ©seau est aussi faible que son maillon le plus bas. Nous avons sÃ©curisÃ© les commutateurs pour empÃªcher les attaques de proximitÃ©.

* **Port-Security :** Limitation Ã  5 adresses MAC par port avec apprentissage dynamique (*sticky*) pour bloquer tout branchement d'Ã©quipement pirate .


* **BPDU Guard :** DÃ©sactivation automatique du port si un switch non autorisÃ© est dÃ©tectÃ© (prÃ©vention de l'usurpation de Root Bridge).


* **Storm Control :** Limitation du trafic broadcast Ã  50% pour prÃ©venir le dÃ©ni de service (DoS).


* **DÃ©sactivation des services :** HTTP, Telnet et CDP ont Ã©tÃ© dÃ©sactivÃ©s globalement pour rÃ©duire la surface d'attaque .



---

## ð Politiques & Gouvernance (Blue Team Playbook)

La technique sans processus n'est rien. Nous avons rÃ©digÃ© et appliquÃ© 6 politiques majeures :

1. **Filtrage ASA :** Refus par dÃ©faut (*Deny Any*).


2. **Principe du Moindre PrivilÃ¨ge :** AccÃ¨s administratifs via SSH v2 uniquement.


3. **Gestion des IdentitÃ©s :** Mots de passe complexes (10+ caractÃ¨res, symboles) via Active Directory.


4. **Cycle de Patch :** Revue mensuelle et application sous 24h pour les failles critiques .


5. **Sauvegarde & Restauration :** Tests hebdomadaires de restauration des serveurs critiques .


6. **RÃ©ponse aux Incidents :** Protocole formel d'isolation et d'analyse post-mortem .



---

## ðº Vitrine : Interface NetCore Solution

Le projet intÃ¨gre une interface web dynamique pour la boutique de solutions informatiques de NetCore. Elle permet de gÃ©rer un catalogue d'Ã©quipements (MikroTik, pfSense, Cisco) et de simuler le flux transactionnel entre les zones DMZ et Inside.

<img width="1886" height="778" alt="image" src="https://github.com/user-attachments/assets/eb22e90e-5b4e-4b12-804e-822e0674271d" />


---

## ð§ª L'Ãpreuve du Feu : Red Team vs Blue Team

Le projet s'est conclu par une simulation d'attaque rÃ©elle :

* **Phase 1 (Reconnaissance) :** Tentatives de scans furtifs bloquÃ©s par l'ASA et journalisÃ©s.


* **Phase 2 (Intrusion) :** Simulation d'une compromission DMZ ; l'attaquant a Ã©tÃ© stoppÃ© par l'Ã©tanchÃ©itÃ© DMZ/Inside.


* **Phase 3 (Mouvement LatÃ©ral) :** Les Honeypots ont permis de lever une alerte immÃ©diate lors de la tentative de scan interne.



---

<div align="center">

### 🔗 Liens

[![Portfolio](https://img.shields.io/badge/Portfolio-00f2ff?style=for-the-badge&logo=firefox&logoColor=black)](https://kim-san04.github.io) [![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/hakim-sawadogo) [![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Kim-San04)

**Cheick Abdel Hadime Hakim SAWADOGO**
*Mastère Cybersécurité, Réseaux & Cloud — Efrei Bordeaux*
📧 cheick.sawadogo@efrei.net

![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:00f2ff,100:0d1117&height=80&section=footer)

</div>
