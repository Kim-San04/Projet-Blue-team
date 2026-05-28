<div align="center">

![Header](https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,100:00f2ff&height=120&section=header&text=&fontSize=0)

</div>

# 🛡️ Opération NetCore — Défense Zero-Trust

<div align="center">

![Cisco ASA](https://img.shields.io/badge/Cisco_ASA-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white) ![Active Directory](https://img.shields.io/badge/Active_Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white) ![VPN IPsec](https://img.shields.io/badge/VPN_IPsec-FF6600?style=for-the-badge&logo=wireguard&logoColor=white) ![Blue Team](https://img.shields.io/badge/Blue_Team-38BDF8?style=for-the-badge&logo=shield&logoColor=white) ![Zero Trust](https://img.shields.io/badge/Zero_Trust-00FF41?style=for-the-badge&logo=security&logoColor=black)

![Efrei](https://img.shields.io/badge/Efrei_Bordeaux-Mastère_Cybersécurité-purple?style=flat-square) ![Status](https://img.shields.io/badge/Status-Terminé-success?style=flat-square)

</div>

> Conception d'une infrastructure Zero-Trust & Lead Blue Team

## 📋 Table des Matières
- [Vision du Projet](#-vision-du-projet)
- [Architecture Réseau](#-architecture-du-réseau)
- [Tunnel IPsec](#-tunnel-ipsec-site-à-site)
- [Honeypots](#-stratégie-honeypots)
- [Hardening](#-hardening-couche-2)
- [Gouvernance](#-politiques--gouvernance)
- [Red vs Blue](#-simulation-red-vs-blue)

---

## 🌌 Vision du Projet

Ce projet simule l'infrastructure critique de **NetCore Solutions**, une entreprise de distribution technologique, dans un cyber-espace saturé de menaces.

En tant que **Chef de l'équipe Blue Team**, j'ai supervisé l'intégralité du cycle de vie sécuritaire : de la segmentation réseau initiale au durcissement des équipements face à une **Red Team** agressive, jusqu'à la validation finale par un audit de conformité.

---

## 🏗️ Architecture du Réseau

L'infrastructure repose sur une séparation physique et logique stricte gérée par un pare-feu **Cisco ASA (v9.0.1)**.

| Zone | Niveau de Sécurité | Description |
| :--- | :---: | :--- |
| **INSIDE** | 100 | Le sanctuaire : AD DS, Messagerie, Honeypots |
| **DMZ** | 70 | Zone tampon : Serveur Web public |
| **OUTSIDE** | 0 | Le périmètre non sécurisé (Internet / Agence) |

### Plan d'Adressage

* **Siège (Inside) :** `192.168.4.0/24`
* **DMZ :** `192.168.1.0/24`
* **Agence Distante :** `192.168.3.0/24`

---

## 🔒 Tunnel IPsec Site-à-Site

Pour sécuriser les flux entre le siège et l'agence, un tunnel VPN IPsec robuste a été déployé sur les routeurs R1 et R2, utilisant un chiffrement de classe militaire.

**Configuration des Politiques IKEv1 (Extraits) :**

```cisco
! -- Configuration sur R1 (Siège) --
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 14
 lifetime 3600

crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac
```

---

## 🍯 Stratégie Honeypots

Trois honeypots ont été déployés stratégiquement dans la zone INSIDE pour détecter les mouvements latéraux :

* **Honeypot #1** : Faux serveur de fichiers (port 445/SMB)
* **Honeypot #2** : Fausse imprimante réseau (port 9100)
* **Honeypot #3** : Faux contrôleur de domaine secondaire

---

## 🛡️ Hardening — Couche 2

Mesures de sécurisation appliquées sur les switches Cisco :

* **Port Security** : limitation à 2 adresses MAC par port
* **BPDU Guard** : protection contre les attaques STP
* **DAI (Dynamic ARP Inspection)** : protection contre l'ARP spoofing
* **DHCP Snooping** : filtrage des serveurs DHCP rogue

---

## 📜 Politiques & Gouvernance

**Blue Team Playbook** — Procédures opérationnelles définies :

1. **Gestion des accès** : principe du moindre privilège, MFA obligatoire
2. **Surveillance** : corrélation des logs via SIEM
3. **Gestion des incidents** : isolation immédiate, analyse post-mortem
4. **Cycle de Patch** : revue mensuelle, application sous 24h pour les failles critiques
5. **Sauvegarde & Restauration** : tests hebdomadaires de restauration des serveurs critiques
6. **Réponse aux Incidents** : protocole formel d'isolation et d'analyse post-mortem

---

## 🧪 Simulation Red vs Blue

Le projet s'est conclu par une simulation d'attaque réelle :

* **Phase 1 (Reconnaissance)** : Tentatives de scans furtifs bloquées par l'ASA et journalisées
* **Phase 2 (Intrusion)** : Simulation d'une compromission DMZ — l'attaquant a été stoppé par l'étanchéité DMZ/Inside
* **Phase 3 (Mouvement Latéral)** : Les honeypots ont permis de lever une alerte immédiate lors de la tentative de scan interne

---

<div align="center">

[![Portfolio](https://img.shields.io/badge/Portfolio-00f2ff?style=for-the-badge&logo=firefox&logoColor=black)](https://kim-san04.github.io) [![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/hakim-sawadogo) [![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Kim-San04)

![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:00f2ff,100:0d1117&height=80&section=footer)

</div>
