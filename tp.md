# Évaluation d’une infrastructure Cloud pédagogique – École Heptagone

## 1. Contexte

L’école Heptagone souhaite mettre en place une **infrastructure Cloud on-premise** afin d’héberger des environnements pédagogiques pour des cours DevOps et Cloud.

Deux classes de **20 étudiant·es** doivent pouvoir déployer simultanément, chacun·e, **quatre machines virtuelles Debian GNU/Linux** :

| Rôle | RAM | CPU |
|------|-----|-----|
| Bastion SSH | 512 MiB | 1 vCPU |
| Reverse proxy NGINX | 1 GiB | 1 vCPU |
| Serveur Web Apache + PHP | 2 GiB | 1 vCPU |
| Serveur MariaDB | 2 GiB | 1 vCPU |

**Total par étudiant·e** :  
- **RAM** : 5,5 GiB  
- **CPU** : 4 vCPU  
- **VM** : 4 instances  

---

## 2. Dimensionnement global

Pour **40 étudiant·es simultanés** :

| Ressource | Total estimé |
|-----------|--------------|
| Machines virtuelles | **160** |
| RAM | **≈ 220 GiB** |
| vCPU | **≈ 160 vCPU** |
| Stockage (OS + données) | **≈ 3 To SSD** |

Une marge de sécurité de **20 %** est recommandée pour éviter la saturation.

---

## 3. Solution on-premise avec cluster Proxmox

### 3.1 Hypothèse d’architecture

Infrastructure minimale redondante :

- **2 serveurs physiques**
  - ~24–32 cœurs CPU
  - **256 GiB RAM** chacun
  - **SSD NVMe en RAID**
- **Stockage partagé** (NAS/SAN SSD)
- **Switch réseau Gigabit ou 10 GbE**

Cette configuration permet :

- haute disponibilité pédagogique
- tolérance de panne simple
- performances suffisantes pour 160 VM légères

---

### 3.2 Estimation des coûts matériels (CAPEX)

| Équipement | Quantité | Coût unitaire estimé | Total |
|------------|----------|----------------------|-------|
| Serveurs physiques 256 GiB RAM | 2 | 2 500 € | 5 000 € |
| NAS/SAN SSD (~6 × 2 To RAID) | 1 | 1 200 € | 1 200 € |
| Switch + câblage + rack | – | – | 650 € |

**Total matériel initial** : **≈ 6 850 €**

---

### 3.3 Coûts annuels (OPEX)

| Poste | Coût annuel estimé |
|-------|--------------------|
| Support Proxmox (optionnel) | ~800 € |
| Électricité | ~400 € |
| Maintenance / temps technique | ~2 000 € |

**OPEX annuel** : **≈ 3 200 €**

---

### 3.4 Coût global

- **Première année** : ~**10 000 €**
- **Années suivantes** : ~**3 200 €/an**

➡️ Solution **économique à long terme**, mais nécessitant :

- compétences internes
- gestion matérielle
- supervision locale

---

## 4. Alternative : serveurs dédiés externalisés

### 4.1 Dimensionnement équivalent

Pour atteindre :

- ~160 vCPU
- ~220 GiB RAM
- ~3 To SSD

Il faut environ **4 serveurs dédiés haut de gamme**.

---

### 4.2 Estimation de coût annuel

| Ressource | Prix mensuel estimé | Total annuel |
|-----------|---------------------|--------------|
| 4 serveurs dédiés | ~600 €/mois | **≈ 7 200 €/an** |

Avantages :

- pas d’investissement initial
- maintenance matérielle incluse
- montée en charge simple

Inconvénients :

- coût récurrent plus élevé
- dépendance fournisseur
- latence réseau externe

---

## 5. Comparaison synthétique

| Critère | On-premise | Serveurs dédiés |
|---------|------------|-----------------|
| Investissement initial | Élevé | Nul |
| Coût annuel | Faible | Élevé |
| Scalabilité | Limitée | Forte |
| Maintenance | Interne | Fournisseur |
| Contrôle pédagogique | Total | Partiel |

**Lecture** :

- **Court terme** : externalisation plus simple  
- **Long terme** : on-premise plus rentable  

---

## 6. Autres stacks Cloud open source envisageables

En complément d’un cluster Proxmox :

| Solution | Type | Complexité | Usage pédagogique |
|----------|------|------------|-------------------|
| OpenStack | IaaS complet | Élevée | Avancé / recherche |
| CloudStack | IaaS | Moyenne | Universités / PME |
| OpenNebula | IaaS léger | Moyenne | Enseignement |
| oVirt | Virtualisation datacenter | Moyenne | Admin système |
| Kubernetes + KubeVirt | Conteneurs + VM | Élevée | Cloud-native |

**Orientation recommandée** :

- **Proxmox** pour démarrage rapide
- **OpenStack** pour cursus avancé Cloud
- **Kubernetes** pour DevOps moderne

---

## 7. Conclusion

Pour un usage pédagogique simultané de **40 étudiant·es** :

- L’option **on-premise** est la plus **économique sur la durée** et offre un **contrôle total**.
- L’option **serveurs dédiés externalisés** apporte **simplicité et élasticité**, mais à un **coût annuel supérieur**.
- Une stratégie réaliste consiste à :
  - démarrer **on-premise avec Proxmox**
  - compléter par du **cloud externe ponctuel**
  - introduire progressivement **OpenStack ou Kubernetes** dans les enseignements.

Cette approche équilibre **coût, pédagogie et évolutivité**.
