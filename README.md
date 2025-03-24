# 🎛️ Optimiser le Type de Processeur des VMs Proxmox avec `x86-64-v3`

## 🧠 Contexte

Ce guide s'adresse aux utilisateurs de **Proxmox** disposant d'un processeur **AMD Ryzen Threadripper 3970X (Zen 2)** ou équivalent.  
Le but est d’optimiser les performances et la compatibilité des machines virtuelles en utilisant le type de processeur **`x86-64-v3`**.

## ✅ Pourquoi utiliser `x86-64-v3` ?

Le profil `x86-64-v3` correspond à un ensemble d'instructions CPU modernes, **pleinement supportées** par les CPU AMD Zen 2 comme le Threadripper 3970X.

### Avantages :
- Bonnes **performances** et **optimisations** (AVX2, FMA, BMI, etc.)
- Meilleure **portabilité** entre nœuds Proxmox à CPU similaires
- **Moins de risques** que `x86-64-v4`, qui exige AVX-512 (non pris en charge par Zen 2)

## ⚠️ Précautions

- Éteignez la VM avant de faire ce changement.
- Faites un **snapshot ou backup** complet par sécurité.
- Après changement, **vérifiez le bon fonctionnement** de la VM (système, services, etc.).

---

## 🛠️ Étapes pour modifier une VM

### Depuis l’interface graphique (GUI) :

1. Éteindre la VM
2. Aller dans `VM → Hardware → Processor`
3. Cliquer sur l’icône ✏️ (Edit)
4. Modifier le champ **"Type"** en : `x86-64-v3`
5. Sauvegarder, puis redémarrer la VM

---

### Depuis la ligne de commande (CLI) :

```bash
qm set <vmid> -cpu x86-64-v3
