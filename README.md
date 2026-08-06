# Infrastructure d'Entreprise Virtualisée (VMware ESXi & Active Directory)

Projet d'infrastructure réseau et virtualisation visant à concevoir un environnement d'entreprise cloisonné, administré et sécurisé.

---

## Table des Matières
1. [Architecture de l'Infrastructure](#-architecture-de-l'infrastructure)
2. [Étapes de Déploiement (Pas à Pas)](#-étapes-de-déploiement-pas-à-pas)
3. [Phase de Tests et Validation](#-phase-de-tests-et-validation)

---

## 1. Architecture de l'Infrastructure
L'environnement repose sur deux hyperviseurs VMware ESXi (`esxi1` et `esxi2`) interconnectés et segmentés via des groupes de ports virtuels :
* **DC01** : Contrôleur de Domaine (Active Directory, DNS, DHCP).
* **FILE01** : Serveur de fichiers avec arborescence et partages sécurisés.
* **WEB01** : Serveur Web Apache personnalisé (page institutionnelle).
* **CLIENT01 à 04** : Postes clients intégrés au domaine avec limitations de bande passante.

---

## 2. Étapes de Déploiement (Pas à Pas)

### Étape 1 : Virtualisation et Configuration des ESXi
* Déploiement des deux hôtes ESXi.
* Configuration des FQDN (`esxi1-TP2.bb.ca` et `esxi2-TP2.bb.ca`), des paramètres réseau et activation du service SSH.
* Création des commutateurs virtuels et des groupes de ports dédiés (`PG-SERVEURS`, `PG-WEB`, `PG-CLIENT01 à 04`).

### Étape 2 : Déploiement des Services d'Infrastructure (DC01)
* Installation et configuration du système pour le rôle de Contrôleur de Domaine Active Directory (`TP2.BB.CA`).
* Mise en place du service DNS (zones directes et inverses) et du service DHCP pour l'attribution dynamique des adresses IP des clients.

### Étape 3 : Mise en place des Serveurs (FILE01 et WEB01)
* Déploiement des machines virtuelles Linux/Windows.
* Jonction au domaine Active Directory.
* Configuration du serveur de fichiers (`FILE01`) avec les permissions de partage.
* Configuration du serveur web (`WEB01`) sous Apache avec personnalisation de la page HTML (nom, initiales, serveur, date).

### Étape 4 : Politiques de Sécurité et Traffic Shaping
* Application des limites de bande passante sur les groupes de ports ESXi (ex: `PG-WEB` à 3 Mbps, `PG-CLIENT04` à 10 Mbps).

---

## 3. Phase de Tests et Validation
*(Vous pouvez faire pointer cette section vers votre dossier de tests)*
* **Test du Traffic Shaping :** Validation des débits bridés via transferts ou outils de mesure.
* **Migration de machine virtuelle :** Déplacement à chaud/froid du `CLIENT04` de `esxi1` vers `esxi2` avec vérification de la persistance de la connectivité réseau et du domaine.

*Voir le dossier [Tests/](./Tests/) pour retrouver l'ensemble des captures d'écran justificatives.*
