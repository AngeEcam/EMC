# Rapport Technique : Conception et Analyse de Filtres EMI pour Convertisseurs à Découpage

## Résumé
Les alimentations à découpage et convertisseurs DC-DC génèrent des perturbations électromagnétiques dues aux commutations rapides ($dV/dt$, $dI/dt$) et aux résonances parasites. Ce rapport analyse les mécanismes de propagation, les topologies de filtrage passif et les contraintes d'intégration (stabilité, parasites, normes).

---

## 1. Fondamentaux de la CEM en Énergie
La Compatibilité Électromagnétique (CEM) repose sur deux piliers :
* **Émissions :** Limiter les perturbations générées par l'équipement.
* **Immunité :** Garantir le fonctionnement malgré les agressions externes.

Dans les systèmes de puissance modernes (GaN, SiC), la réduction des temps de montée ($t_r$) déplace l’énergie parasite vers des fréquences très élevées, rendant le filtrage et le layout critiques.

---

## 2. Émissions Conduites vs Rayonnées
| Type | Propagation | Gamme de fréquence (Typique) | Méthode de mesure |
| :--- | :--- | :--- | :--- |
| **Conduites** | Câbles et pistes | 150 kHz – 30 MHz | LISN + Récepteur EMI |
| **Rayonnées** | Ondes EM (Air) | 30 MHz – 1 GHz+ | Antenne + Chambre anéchoïde |

> **Note :** La frontière de 30 MHz est normative. En réalité, un mauvais filtrage conduit se transforme souvent en émissions rayonnées car les câbles agissent comme des antennes.

---

## 3. Origines du Bruit et Couplages
Le bruit provient principalement des commutations :
1.  **Capacitif ($dV/dt$) :** Courants de déplacement via les capacités parasites vers le châssis.
2.  **Inductif ($dI/dt$) :** Tensions induites par les boucles de courant.
3.  **Impédance commune :** Chute de tension dans une masse partagée.

---

## 4. Modes de Propagation : Commun (MC) vs Différentiel (MD)

### 4.1 Définitions
* **Mode Différentiel (MD) :** Le bruit circule entre les lignes (Phase/Neutre). Il est lié au courant de ripple du convertisseur.
* **Mode Commun (MC) :** Le bruit circule dans le même sens sur tous les conducteurs et revient par la terre/châssis. Il est lié aux capacités parasites des composants de puissance.



### 4.2 Conversion de mode
Toute dissymétrie dans le layout ou les composants transforme une partie du MD en MC, complexifiant la mise en conformité.

---

## 5. Le LISN (Line Impedance Stabilization Network)
Le LISN (ou AMN) est indispensable pour :
1.  **Standardiser l'impédance :** Présenter 50 $\Omega$ au dispositif testé (DUT).
2.  **Filtrer le réseau :** Isoler le bruit venant de la prise secteur.
3.  **Mesurer :** Prélever le signal RF pour le récepteur.

---

## 6. Atténuation vs Perte d'Insertion (Insertion Loss)
L'**Insertion Loss (IL)** est la mesure réelle de l'efficacité du filtre une fois inséré entre une source et une charge.
$$IL(f) = 20 \log_{10} \left| \frac{V_{sans\_filtre}}{V_{avec\_filtre}} \right|$$
Elle dépend directement du **déséquilibre d'impédance** (mismatch) : un filtre est d'autant plus efficace que ses impédances internes sont opposées à celles du système.

---

## 7. Composants du Filtre EMI

### 7.1 Self de Mode Commun (CMC)
Bloque le MC grâce à un flux additif, mais laisse passer le MD (flux soustractifs qui s'annulent).
* **Matériaux :** Ferrite (MnZn) pour les basses fréquences, Nanocristallin pour une perméabilité élevée sur large bande.

### 7.2 Condensateurs X et Y
* **X (Ligne-Ligne) :** Shunte le mode différentiel.
* **Y (Ligne-Terre) :** Shunte le mode commun. 
    * *Contrainte :* Valeur limitée (souvent < 4.7 nF) pour respecter les normes de sécurité sur le courant de fuite.



---

## 8. Éléments Parasites et Résonances
Les composants réels ont des limites :
* **Inductances :** Capacité parasite entre spires ($C_p$) → Fréquence de Résonance Propre (SRF). Au-delà de la SRF, la self devient capacitive.
* **Condensateurs :** Inductance série (ESL) → Au-delà de sa SRF, le condo devient inductif.

---

## 9. Stabilité et Critère de Middlebrook
Un convertisseur DC-DC se comporte comme une **résistance négative** à basse fréquence. L'ajout d'un filtre d'entrée peut provoquer des oscillations si l'impédance de sortie du filtre $|Z_{out,f}|$ s'approche de l'impédance d'entrée du convertisseur $|Z_{in,c}|$.

**Condition de stabilité :**
$$|Z_{out,f}| \ll |Z_{in,c}|$$
Il est souvent nécessaire d'ajouter un **réseau d'amortissement (damping)** (ex: condensateur électrolytique en parallèle ou circuit RC).

---

## 10. Topologies Passives
* **LC :** Simple, nécessite un bon amortissement.
* **$\pi$ (C-L-C) :** Très efficace pour les sources et charges à haute impédance.
* **T (L-C-L) :** Idéal pour les basses impédances.

---

## 11. Compromis et Méthodologie
La conception est un équilibre entre :
* **Performance :** Atténuation en dB.
* **Volume/Coût :** Les CMC sont volumineuses et coûteuses.
* **Thermique :** Pertes par effet Joule dans les selfs (DCR).

**Approche pré-compliance :**
1. Mesure sans filtre pour identifier les pics.
2. Simulation avec modèles de composants réels (incluant ESR/ESL).
3. Itération sur prototype avec LISN de table.

---

## 12. Conclusion
Le design d'un filtre EMI n'est pas une science isolée du reste du système. La réussite dépend autant du choix des composants que de la qualité du layout et de la prise en compte des interactions dynamiques avec le convertisseur.
