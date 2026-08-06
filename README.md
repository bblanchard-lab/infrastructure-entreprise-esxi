# TP2 - Infrastructure Virtuelle et Sécurisation d'un Environnement d'Entreprise

Projet académique réalisé dans le cadre du programme de Réseau et Cybersécurité, visant à concevoir, déployer et administrer une infrastructure virtualisée multi-hôtes sous VMware ESXi, intégrée à un domaine Active Directory, avec des mesures de contrôle de trafic et de services réseau.

## Aperçu du Projet
Ce projet simule l'infrastructure informatique d'une PME répartie sur deux hôtes de virtualisation. Il met en œuvre des mécanismes de cloisonnement réseau, de services d'infrastructure centralisés (DNS, DHCP, Active Directory, Partage de fichiers) et de services web sécurisés.

## Outils et Technologies Utilisés
* **Hyperviseur :** VMware ESXi
* **Systèmes d'exploitation :** Windows Server (Active Directory / DNS), Linux Ubuntu Server (Apache, SSSD), Windows 10/11 (Clients)
* **Réseau :** TCP/IP, DNS, DHCP, Kerberos, Traffic Shaping ESXi

---

## Architecture et Composants de l'Infrastructure

L'environnement repose sur deux hyperviseurs VMware ESXi gérant plusieurs machines virtuelles segmentées par des groupes de ports (Port Groups) dédiés :

* **Contrôleur de Domaine (`DC01`) :** Gestion active Directory (AD DS), service DNS principal et service DHCP.
* **Serveur de Fichiers (`FILE01`) :** Partage de fichiers d'entreprise structuré par départements avec gestion des permissions d'accès basées sur le domaine.
* **Serveur Web (`WEB01`) :** Serveur HTTP (Apache) hébergeant la page de présentation institutionnelle du projet, joint au domaine via SSSD/Kerberos.
* **Postes Clients (`CLIENT01` à `CLIENT04`) :** Postes de travail intégrés au domaine, utilisés pour les tests de connectivité, de routage et de performance.

---

## Réalisations Techniques Clés

### 1. Virtualisation et Haute Disponibilité (VMware ESXi)
* Déploiement et configuration de deux hôtes ESXi (`esxi1` et `esxi2`) avec attribution de FQDN propres (`esxi1-TP2.bb.ca`, `esxi2-TP2.bb.ca`) et activation des accès d'administration sécurisés (SSH).
* Configuration des commutateurs virtuels et des groupes de ports isolés (`PG-SERVEURS`, `PG-WEB`, `PG-CLIENT01` à `04`).
* Migration à chaud / à froid d'une machine virtuelle (`CLIENT04`) d'un hôte ESXi à un autre avec validation de la continuité de service.

### 2. Contrôle de Bande Passante (Traffic Shaping)
Mise en place de politiques de limitation de débit au niveau des groupes de ports ESXi pour simuler des contraintes réseau réalistes :
* **`PG-WEB` :** Limité à 3 Mbps (~375 Ko/s)
* **`PG-CLIENT01` à `04` :** Politiques échelonnées de 1 Mbps à 10 Mbps (ex: `CLIENT04` à 10 Mbps / ~1250 Ko/s).

### 3. Intégration au Domaine et Sécurité
* Jonction des serveurs Linux (`WEB01`, `FILE01`) au domaine Active Directory (`TP2.BB.CA`) via l'authentification Kerberos et SSSD.
* Configuration des enregistrements DNS (directs et inverses) sur le DC pour assurer la résolution FQDN de l'ensemble des services.
