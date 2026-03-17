# VPN IPSec Site-to-Site – GNS3

## Vue d'ensemble du projet

Ce projet démontre la conception, la configuration et la validation d'un tunnel VPN IPSec site-to-site sécurisé entre deux réseaux locaux distants. La solution est implémentée via des routeurs Cisco dans l'environnement GNS3, simulant une interconnexion sécurisée sur un réseau public.

### Garanties de sécurité IPSec

- ✓ **Confidentialité** : Chiffrement des données en transit
- ✓ **Intégrité** : Vérification de l'authenticité des échanges
- ✓ **Authentification** : Validation des équipements distants

---

## Objectifs du projet

1. Comprendre l'architecture et le fonctionnement d'un VPN IPSec site-to-site
2. Mettre en œuvre les phases IKE (Phase 1 et Phase 2)
3. Configurer les politiques de sécurité IPSec
4. Utiliser une authentification basée sur des clés RSA
5. Vérifier et valider un tunnel VPN fonctionnel
6. Identifier les limites d'un environnement de simulation

---

## Architecture réseau

### Topologie

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  Site ABIDJAN              Réseau INTERNET             Site HONGKONG
│  10.10.0.0/16         [Routeur INTERNET]               192.168.2.0/24
│      │                      │                               │
│      │                      ├─ IPSec VPN (ESP) ─────────────┤
│      │                      │                               │
│      │                      │                               │
└─────────────────────────────────────────────────────────┘
```

### Configuration des sites

| Site         | Réseau local         | Passerelle       |
| ------------ | -------------------- | ---------------- |
| **ABIDJAN**  | 10.10.0.0/16         | Routeur ABIDJAN  |
| **HONGKONG** | 192.168.2.0/24       | Routeur HONGKONG |
| **INTERNET** | Réseau intermédiaire | Routeur INTERNET |

### Technologies de sécurité

- **Protocole IPSec** : Encapsulation Sécurisée Utile (ESP)
- **Échange de clés** : IKE Phase 1 et Phase 2
- **Authentification** : RSA (RSA Asymmetric Key Cryptography)

---

## Outils et technologies

| Composant            | Description                          |
| -------------------- | ------------------------------------ |
| **Simulateur**       | GNS3 (Graphical Network Simulator-3) |
| **Équipements**      | Routeurs Cisco IOS, IOU L3           |
| **Protocole VPN**    | IPSec (IKE/ISAKMP)                   |
| **Chiffrement**      | ESP (Encapsulating Security Payload) |
| **Authentification** | RSA avec clés publiques              |
| **Routage**          | Statique                             |

---

## Étapes de mise en œuvre

### 1. Configuration IP et routage

- Attribution des adresses IP aux routeurs et aux hôtes
- Configuration des routes statiques
- Vérification de la connectivité de base entre les sites

### 2. Configuration du VPN IPSec

#### Phase 1 – Établissement de l'association de sécurité IKE

```
2.1.1 Définition des politiques ISAKMP
      • Choix des algorithmes de chiffrement (AES, 3DES)
      • Sélection de la méthode d'authentification (RSA)
      • Configuration du domaine

2.1.2 Génération des clés RSA
      • Création des paires de clés publique/privée
      • Échange des clés publiques entre routeurs

2.1.3 Application des configurations
      • Activation des politiques ISAKMP
      • Configuration de l'association de sécurité IKE
```

**Note** : Certaines commandes avancées (ex : `crypto isakmp keepalive`) peuvent ne pas être disponibles selon la version de GNS3.

#### Phase 2 – Configuration IPSec

```
2.2.1 Définition des transform-sets
      • Spécification du chiffrement ESP
      • Configuration du protocole d'authentification
      • Définition des algorithmes (AES-CBC, SHA-256)

2.2.2 Activation du Perfect Forward Secrecy (PFS)
      • Configuration de la génération de clés éphémères

2.2.3 Création et gestion des associations de sécurité (SA)
      • Établissement des canaux de communication sécurisés
      • Configuration des durées de vie des SA
```

### 3. Vérification et validation

#### Commandes de diagnostic

```bash
# Vérification de l'état IKE
show crypto isakmp sa

# Vérification de l'état IPSec
show crypto ipsec sa

# Affichage des crypto maps
show crypto map

# Test de connectivité de bout en bout
ping <adresse_destination>
```

#### Indicateurs de succès

- ✓ État IKE en `QM_IDLE` (Phase 2 active)
- ✓ Augmentation des compteurs de paquets encapsulés/décapsulés
- ✓ Réussite des tests de ping de bout en bout
- ✓ Validation complète des phases IKE et IPSec

---

## Résultats obtenus

- ✓ Tunnel VPN IPSec opérationnel entre les deux sites
- ✓ Chiffrement effectif du trafic sur le réseau public
- ✓ Communication sécurisée entre les réseaux locaux distants
- ✓ Validation complète des phases IKE et IPSec

---

## Organisation du dépôt

```
IPSec-VPN-GNS3-TP/
│
├── README.md                          # Documentation principale
├── rapport/                           # Rapports détaillés
│   └── ...
│
├── topology.gns3/                    # Fichier de topologie GNS3
│   ├── configs/
│   │   ├── ios_base_startup-config.txt
│   │   ├── ios_etherswitch_startup-config.txt
│   │   ├── iou_l2_base_startup-config.txt
│   │   ├── iou_l3_base_startup-config.txt
│   │   └── vpcs_base_config.txt
│
├── screenshots/                       # Captures d'écran
│   ├── isakmp_sa.png
│   ├── ipsec_sa.png
│   └── ping_test.png
│
└── docs/                             # Documentation supplémentaire
    ├── schema_reseau.png
    └── guide_configuration.pdf
```

---

## Limitations

- Certaines fonctionnalités IPSec avancées ne sont pas disponibles dans GNS3
- Absence de mécanisme de keepalive réel (selon les versions)
- Comportement différent d'un environnement de production réel
- Performance simulée limitée par les ressources système

---

## Informations académiques

| Champ                | Valeur                                                    |
| -------------------- | --------------------------------------------------------- |
| **Projet**           | Sécurité des réseaux – VPN IPSec                          |
| **Auteur**           | Saïh Piely Uriel Loïc                                     |
| **Niveau**           | Élève ingénieur 2e année STIC Informatique                |
| **Établissement**    | Institut National Polytechnique Houphouët-Boigny (INP-HB) |
| **Année académique** | 2025–2026                                                 |

---

##  Conclusion

Ce projet démontre avec succès la mise en œuvre d'un tunnel VPN IPSec site-to-site sécurisé dans un environnement de simulation. Il fournit une base solide pour :

- Comprendre les mécanismes de sécurité réseau
- Maîtriser la configuration de solutions VPN professionnelles
- Transitionner vers des environnements de production réels

### Prochaines étapes recommandées

- Migration vers un environnement GNS3 plus complet
- Déploiement sur des équipements physiques Cisco
- Intégration avec d'autres protocoles de sécurité (DMVPN, FlexVPN)
- Implémentation de mécanismes de failover et de résilience

---

**Dernière mise à jour** : Mars 2026
