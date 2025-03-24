# 🎛️ Optimiser le Type de Processeur des VMs Proxmox

## 🧠 Contexte

Ce guide s'adresse aux utilisateurs de **Proxmox VE** disposant d’un processeur **AMD Ryzen Threadripper 3970X** (ou tout autre CPU basé sur l’architecture **Zen 2**), souhaitant optimiser la configuration de leurs machines virtuelles (VM).

Par défaut, Proxmox propose plusieurs types de processeurs virtuels, mais tous ne sont pas adaptés à ton matériel. Utiliser un type mal adapté peut :

- Réduire les performances
- Empêcher une VM de démarrer
- Créer des problèmes de compatibilité avec certaines instructions

---

## ✅ Pourquoi utiliser `x86-64-v3` ?

Le profil `x86-64-v3` correspond à un niveau d’instructions modernes, **pleinement supportées par le CPU Zen 2** (comme le Threadripper 3970X).

### Avantages du type `x86-64-v3` :

- Compatible avec toutes les instructions importantes : **AVX2, FMA, BMI1/2, ADX, etc.**
- Excellente **performance** sans sur-exposer le CPU physique
- Bonne **portabilité** entre nœuds Proxmox similaires
- Évite les problèmes liés au profil `x86-64-v4` (qui exige **AVX-512**, non pris en charge par Ryzen/Threadripper)

---

## 🔍 Comparaison des types de processeurs Proxmox

| Type de CPU | Performances | Compatibilité | Commentaire |
|-------------|--------------|----------------|-------------|
| `host` | ⭐⭐⭐⭐ | ⭐ | Expose toutes les instructions du CPU physique. Idéal sur un seul nœud. |
| `x86-64-v3` | ⭐⭐⭐ | ⭐⭐⭐ | Excellent compromis entre perfs et compatibilité |
| `x86-64-v4` | ⭐⭐⭐⭐ | ❌ | Non compatible avec Zen 2 (AVX-512 requis) |
| `kvm64` | ⭐ | ⭐⭐⭐⭐ | Très compatible, mais très limité niveau perfs |

---

## 🧪 Support des instructions du Threadripper 3970X

CPU : **AMD Ryzen Threadripper 3970X** (32 cœurs / 64 threads, Zen 2)

| Instruction | Supporté ? |
|-------------|------------|
| SSE / SSE2 / SSE3 / SSSE3 / SSE4.1 / SSE4.2 | ✅ |
| AVX / AVX2 | ✅ |
| FMA (Fused Multiply-Add) | ✅ |
| BMI1 / BMI2 | ✅ |
| MOVBE | ✅ |
| ADX | ✅ |
| CLMUL | ✅ |
| POPCNT | ✅ |
| **AVX-512** | ❌ (absent sur Ryzen/Threadripper) |

---

## ⚠️ Avant de modifier une VM

> **Important :**
>
> - Éteignez la VM avant modification
> - Créez une **sauvegarde ou snapshot**
> - Vérifiez ensuite le bon fonctionnement (OS, services, performances)

---

## 🛠️ Modifier le type de processeur

### Option 1 : Interface Web Proxmox

1. Éteindre la VM
2. Accéder à **`VM → Hardware → Processor`**
3. Cliquer sur l’icône ✏️ (modifier)
4. Dans le champ **"Type"**, choisir : `x86-64-v3`
5. Enregistrer, redémarrer la VM

---

### Option 2 : Ligne de commande

```bash
qm set <vmid> -cpu x86-64-v3
