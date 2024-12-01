# **Mémoire Technique**

## **Introduction**

### **1. Introduction**

Ce mémoire technique présente une infrastructure réseau conçue pour un environnement multi-étages, en utilisant des équipements Huawei, conformément au DPGF fourni. L’objectif est de répondre aux besoins en connectivité, segmentation et sécurité réseau avec une couverture Wi-Fi 6E performante.

Ce document inclut :

- Le plan d’adressage IP (VLAN par service).
- Les choix d’équipements détaillés.
- Les vérifications PoE et bande passante.
- Une explication de la segmentation et de l’isolation des flux réseau.

### **2. Plan d'adressage IP**

Le réseau a été segmenté en VLANs selon les besoins opérationnels et les impératifs de sécurité. Cette segmentation permet d’isoler les différents services tout en offrant une interconnexion contrôlée là où nécessaire. Le plan d’adressage est défini comme suit :

| VLAN ID | Service                                            | Plage IP        | Accès Internet | Accessible par d'autres VLAN | A accès aux autres VLAN | Accès imprimantes |
| ------- | -------------------------------------------------- | --------------- | -------------- | ---------------------------- | ----------------------- | ----------------- |
| 10      | Ressources Humaines                                | 192.168.1.0/24  | Oui            | Non                          | Non                     | Oui               |
| 20      | Imprimantes                                        | 192.168.2.0/24  | Oui            | Oui                          | Non                     | Oui               |
| 30      | Direction                                          | 192.168.3.0/24  | Oui            | Non                          | Oui                     | Oui               |
| 40      | Données                                            | 192.168.4.0/24  | Oui            | Oui                          | Oui                     | Oui               |
| 50      | Invités Wi-Fi                                      | 192.168.5.0/24  | Oui            | Non                          | Non                     | Non               |
| 60      | Vidéosurveillance                                  | 192.168.6.0/24  | Non            | Non                          | Non                     | Non               |
| 70      | Sécurité incendie                                  | 192.168.7.0/24  | Oui            | Non                          | Non                     | Non               |
| 80      | Comptabilité                                       | 192.168.8.0/24  | Oui            | Non                          | Non                     | Oui               |
| 90      | Design                                             | 192.168.9.0/24  | Oui            | Non                          | Non                     | Oui               |
| 100     | Logistique                                         | 192.168.10.0/24 | Oui            | Non                          | Non                     | Oui               |
| 110     | Développement                                      | 192.168.11.0/24 | Oui            | Non                          | Oui                     | Oui               |
| 120     | Conception                                         | 192.168.12.0/24 | Oui            | Non                          | Non                     | Oui               |
| 130     | Contrôle d’accès, détection intrusion, vidéophonie | 192.168.13.0/24 | Non            | Non                          | Non                     | Non               |
| 140     | DSI                                                | 192.168.14.0/24 | Oui            | Non                          | Oui                     | Oui               |
| 150     | R&D                                                | 192.168.15.0/24 | Oui            | Non                          | Non                     | Oui               |
| 160     | Système audio                                      | 192.168.16.0/24 | Non            | Non                          | Non                     | Non               |

### Points clés de la segmentation VLAN

1. Isolation stricte pour les VLANs critiques :

   - Vidéosurveillance (VLAN 60), contrôle d’accès (VLAN 130) et système audio (VLAN 160) sont totalement isolés du réseau principal. Cela minimise les risques de compromission tout en respectant les impératifs de performances pour ces services critiques.

2. Accès aux ressources communes :

   - Les VLANs métiers comme Ressources Humaines (VLAN 10) et Comptabilité (VLAN 80) ont accès aux imprimantes via le VLAN dédié Imprimantes (VLAN 20).
   - Ces interconnexions sont limitées aux besoins fonctionnels pour réduire l’exposition inutile.

3. VLAN pour visiteurs :

   - Le réseau Invités Wi-Fi (VLAN 50) est entièrement isolé du réseau interne. Ce VLAN offre un accès internet tout en empêchant les visiteurs d’interagir avec les services ou données internes.

4. Direction et DSI :

   - Ces VLANs (30 - Direction, 140 - DSI) sont configurés pour accéder à certains VLANs selon leurs responsabilités. Par exemple, le VLAN Direction a accès au VLAN Données pour la supervision.

5. ACL pour la gestion des flux :

   - Des Access Control Lists (ACL) seront appliquées pour renforcer les restrictions et permettre uniquement les flux autorisés entre VLANs. Par exemple, seules certaines adresses IP des VLANs administratifs peuvent accéder au VLAN Données.

### **3. Choix des équipements**

#### **Switchs cœur de réseau**

Pour répondre aux exigences de robustesse, de performance et de redondance, le choix s’est porté sur les **switchs Huawei CloudEngine S6730-H24X6C**. Voici les raisons principales de cette sélection :

1. **Capacité de traitement et évolutivité :**

   - Le CloudEngine S6730-H24X6C offre des interfaces 10G et 40G, idéales pour les besoins croissants de bande passante dans un réseau central.
   - Il prend en charge les protocoles avancés comme **VXLAN**, utile pour des extensions futures dans des environnements virtualisés.

2. **Redondance et résilience :**

   - La prise en charge des topologies comme **ring** avec **STP** ou **MSTP** garantit une continuité de service en cas de panne.
   - Les ports haute densité et la compatibilité avec les fibres optiques (SFP+) renforcent sa fiabilité pour un backbone réseau.

3. **Gestion simplifiée :**
   - L’intégration de fonctionnalités d’automatisation et de gestion via des plateformes Huawei Cloud facilite le déploiement et la maintenance.

#### **Switchs de distribution**

Deux modèles ont été sélectionnés pour couvrir les besoins variés des étages :

- **Huawei S5735-L24T4X** (24 ports)
- **Huawei S5735-L48T4X** (48 ports)

Ces choix sont justifiés par les points suivants :

1. **Modularité :**

   - La coexistence de deux modèles avec des densités de ports différentes permet d’optimiser les coûts selon le nombre d’équipements connectés par étage.

2. **PoE+ pour les périphériques :**

   - Ces switchs prennent en charge la norme **PoE+**, nécessaire pour alimenter les bornes Wi-Fi et autres équipements critiques sans nécessiter de câblage électrique supplémentaire.

3. **Fonctionnalités avancées :**

   - Ils incluent le support des VLANs, QoS et ACL pour segmenter les flux et prioriser les services essentiels.

4. **Compatibilité ascendante :**
   - Les ports uplink 10G permettent une connexion fluide au cœur de réseau, éliminant les goulots d’étranglement dans la distribution.

#### **Bornes Wi-Fi 6E**

Le modèle **Huawei AirEngine 5761-21** a été choisi pour sa compatibilité avec la norme Wi-Fi 6E. Ce choix repose sur plusieurs facteurs :

1. **Performance dans des environnements exigeants :**

   - Le Wi-Fi 6E étend la couverture sur la bande 6 GHz, réduisant la congestion dans les espaces à forte densité.

2. **Support des services professionnels :**

   - Ces bornes intègrent des fonctionnalités telles que le MU-MIMO et l’OFDMA, permettant une meilleure gestion des flux pour les environnements multi-utilisateurs.

3. **Gestion centralisée :**

   - La possibilité de gérer ces bornes via une interface centralisée garantit une configuration et un monitoring simplifiés.

4. **Conception robuste :**
   - Ces bornes sont adaptées à des environnements intérieurs denses, offrant une couverture homogène et fiable dans les espaces de travail.

## 4. **Vérification du PoE et de la bande passante**

#### **Vérification du PoE**

Le PoE (Power over Ethernet) est crucial pour alimenter les équipements réseau tels que les bornes Wi-Fi et les caméras de surveillance, sans avoir besoin d'une alimentation électrique séparée. Il est essentiel de vérifier que les switchs de distribution peuvent fournir une puissance suffisante pour tous les équipements qui nécessitent du PoE.

#### **Calcul du PoE Total par Étage**

Le calcul du PoE est effectué pour chaque étage en fonction du nombre d'équipements connectés. Voici la répartition de la consommation en PoE par étage :

| **Étage**                | **PoE en W**  |
| ------------------------ | ------------- |
| Étages type 1 (sup)      | 163,33 W      |
| Étages type 2 (RDC)      | 654 W         |
| Étages type 3 (Sous-sol) | 100,6 W       |
| R-1                      | 167 W         |
| R-2                      | 164 W         |
| R-3                      | 170 W         |
| R-4                      | 169 W         |
| R-5                      | 143 W         |
| R-6                      | 167 W         |
| RDC                      | 654 W         |
| SS1                      | 268 W         |
| SS2                      | 80 W          |
| SS3                      | 76 W          |
| SS4                      | 57 W          |
| SS5                      | 22 W          |
| **Total**                | **3054,93 W** |

#### **Vérification de la Bande Passante**

La bande passante est un autre facteur crucial pour garantir une performance optimale du réseau. La bande passante doit être suffisante pour supporter les flux de données générés par les équipements, tels que les bornes Wi-Fi, les caméras de vidéosurveillance et les stations de travail.

#### **Calcul de la Bande Passante Totale par Étage**

La bande passante nécessaire est calculée en fonction des équipements et des services utilisés dans chaque étage. Voici la répartition de la bande passante par étage :

| **Étage**                | **Bande Passante en Mbps** |
| ------------------------ | -------------------------- |
| Étages type 1 (sup)      | 50,47 Mbps                 |
| Étages type 2 (RDC)      | 198,47 Mbps                |
| Étages type 3 (Sous-sol) | 48,9 Mbps                  |
| R-1                      | 112,8 Mbps                 |
| R-2                      | 39 Mbps                    |
| R-3                      | 39,1 Mbps                  |
| R-4                      | 40 Mbps                    |
| R-5                      | 32,8 Mbps                  |
| R-6                      | 39,1 Mbps                  |
| RDC                      | 198,47 Mbps                |
| SS1                      | 147,9 Mbps                 |
| SS2                      | 41,5 Mbps                  |
| SS3                      | 24,1 Mbps                  |
| SS4                      | 20 Mbps                    |
| SS5                      | 11 Mbps                    |
| **Total**                | **1043,60 Mbps**           |

#### Validation des Capacités des Switchs

Pour garantir une performance optimale, chaque switch de distribution doit être capable de supporter la bande passante nécessaire pour chaque étage, en particulier pour les équipements qui consomment beaucoup de bande passante, comme les bornes Wi-Fi et les caméras IP.

Les switchs **Huawei S5735-L24T4X** (24 ports) et **Huawei S5735-L48T4X** (48 ports) choisis pour la distribution sont capables de supporter des débits élevés grâce à leurs ports 10G uplink et à leur gestion du PoE, ce qui permet de maintenir une bande passante fluide même sous des charges importantes.

Les switchs de cœur de réseau (Huawei **CloudEngine S6730-H24X6C**) et les switchs de distribution (Huawei **S5735-L24T4X** et **S5735-L48T4X**) sont bien dimensionnés pour gérer les besoins en PoE et en bande passante du réseau. La consommation totale de PoE de **3054,93 W** et la bande passante totale de **1043,60 Mbps** sont compatibles avec les équipements choisis, garantissant ainsi une infrastructure réseau fiable et performante.
