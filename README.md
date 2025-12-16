# Rapport Technique : Conception et Analyse de Filtres EMI pour Convertisseurs à Découpage

## 1. Pourquoi la CEM devient critique en électronique de puissance
La Compatibilité Électromagnétique (CEM) n'est pas seulement une contrainte normative (CISPR, IEC), c'est une nécessité fonctionnelle. Un équipement doit :
* **Limiter ses émissions :** Pour ne pas polluer le réseau ou perturber les communications sans fil.
* **Garantir son immunité :** Pour rester stable face aux décharges électrostatiques (ESD) ou aux transitoires du réseau.

L'évolution vers des fréquences de commutation plus élevées réduit la taille des composants passifs mais déplace l'énergie harmonique vers le spectre des hautes fréquences (HF), rendant le filtrage et le layout du PCB prédominants sur le design du circuit lui-même.

---

## 2. Émissions Conduites vs Rayonnées : La Frontière Pratique

| Caractéristique | Émissions Conduites | Émissions Rayonnées |
| :--- | :--- | :--- |
| **Vecteur** | Câbles, pistes, fils de terre. | Champ électrique (E) et magnétique (H). |
| **Fréquences** | 150 kHz – 30 MHz (typiquement). | 30 MHz – 1 GHz et plus. |
| **Origine** | Courants de ripple et pics de commutation. | Antennes involontaires (câbles trop longs, boucles). |
| **Mesure** | LISN / AMN + Récepteur EMI. | Antenne en chambre anéchoïde. |

**Interdépendance :** Il est admis en ingénierie que si les émissions conduites sont mal maîtrisées sous 30 MHz, les câbles d'alimentation agiront comme des antennes dipôles, provoquant quasi systématiquement un échec aux tests d'émissions rayonnées à plus haute fréquence.

---

## 3. Origines du Bruit EMI dans un Convertisseur DC-DC

Le bruit provient de la commutation discontinue du courant. On distingue trois couplages dominants :
1.  **Couplage Capacitif ($dV/dt$) :** Le nœud de commutation change d'état très rapidement. Ce saut de tension injecte un courant de déplacement $i = C_{par} \cdot \frac{dv}{dt}$ à travers les capacités parasites (entre le drain du MOSFET et le dissipateur relié à la masse, par exemple).
2.  **Couplage Inductif ($dI/dt$) :** Les fortes variations de courant créent des champs magnétiques qui induisent des tensions parasites dans les boucles adjacentes ($e = -L \cdot \frac{di}{dt}$).
3.  **Impédance commune :** Lorsque le courant de puissance et le signal de commande partagent la même piste de retour (masse), le bruit de puissance "pollue" la référence du signal.

---

## 4. Mode Différentiel (MD) et Mode Commun (MC)

### 4.1 Physique des courants
* **Mode Différentiel (Differential Mode) :** Le courant de bruit circule en opposition de phase sur les conducteurs "aller" et "retour". C'est le bruit directement lié au fonctionnement du hacheur (courant de ripple).
    
* **Mode Commun (Common Mode) :** Le courant circule dans le même sens sur tous les conducteurs et revient par la terre ou le châssis via des capacités parasites. C'est souvent le mode le plus difficile à filtrer car il dépend de la construction mécanique du système.
    

### 4.2 Conversion de mode
Toute asymétrie d'impédance (pistes de longueurs différentes, condensateurs mal équilibrés) provoque une conversion de mode : une partie du bruit MD devient du bruit MC, ce qui rend le filtrage moins prévisible.

---

## 5. Le LISN/AMN : L'Interface de Mesure Normalisée
Le **LISN** (Line Impedance Stabilization Network) remplit trois fonctions critiques lors des tests de conformité :
1.  **Isolation :** Empêche le bruit provenant du réseau électrique extérieur de fausser la mesure.
2.  **Stabilisation :** Présente une impédance de charge constante de **50 $\Omega$** au dispositif testé (DUT), garantissant que les mesures sont répétables partout dans le monde.
3.  **Extraction :** Fournit un port de sortie 50 $\Omega$ pour brancher l'analyseur de spectre ou le récepteur EMI.

---

## 6. Atténuation Théorique vs Perte d'Insertion (Insertion Loss)
Il ne faut pas confondre la fonction de transfert idéale et la réalité du montage :
* **Atténuation :** Calculée sur papier avec des sources et charges idéales.
* **Insertion Loss (IL) :** Perte de puissance réelle mesurée. Elle est définie par :
    $$IL(f) = 20 \log_{10} \left| \frac{V_{sans\_filtre}}{V_{avec\_filtre}} \right|$$
**Règle d'or :** Un filtre n'est efficace que s'il y a un **déséquilibre d'impédance** (mismatch). Une inductance (haute impédance) doit être placée face à une source/charge basse impédance. Un condensateur (basse impédance) doit faire face à une haute impédance.

---

## 7. Composants du Filtre EMI et Rôles Réels

### 7.1 Self de Mode Commun (CMC)
Elle consiste en deux bobinages sur un même noyau (souvent ferrite ou nanocristallin).
* **Action sur le MC :** Les flux s'additionnent, créant une forte impédance qui bloque le bruit.
* **Action sur le MD :** Les flux s'annulent, laissant passer le courant de puissance sans saturer le noyau.
* **Inductance de fuite :** Une CMC réelle possède une inductance de fuite qui peut être utilisée comme un filtre MD "gratuit".

### 7.2 Condensateurs de sécurité (X et Y)
* **Condensateurs X :** Placés entre les lignes. Ils shuntent le bruit en mode différentiel.
* **Condensateurs Y :** Placés entre les lignes et la terre. Ils dérivent le bruit de mode commun vers le châssis. 
    * *Sécurité :* Leur valeur est limitée (souvent < 4.7nF) pour limiter le courant de fuite à 50/60 Hz et éviter l'électrocution en cas de rupture de terre.

---

## 8. Impact des Éléments Parasites
Les composants ne sont jamais idéaux. À haute fréquence, le schéma équivalent change :
* **Inductance réelle :** Possède une capacité parasite entre spires ($C_p$). Au-delà de sa **Fréquence de Résonance Propre (SRF)**, l'inductance devient un condensateur et ne filtre plus rien.
* **Condensateur réel :** Possède une inductance série (ESL) due à ses pattes et ses connexions. Au-delà de sa SRF, il devient une inductance.
* **Conseil layout :** Pour un condensateur Y, 1 cm de piste PCB ajoute environ 10 nH d'inductance, ce qui peut rendre le composant totalement inefficace au-delà de 10 MHz.



---

## 9. Interaction Filtre-Convertisseur : Stabilité (Middlebrook)
Un convertisseur DC-DC régulé présente une **impédance d'entrée négative** ($R_{in} \approx -V^2/P$). Si l'impédance de sortie du filtre EMI présente un pic de résonance trop élevé, le système devient instable (oscillations, effondrement de tension).

**Critère de stabilité :**
$$|Z_{out\_filtre}| \ll |Z_{in\_convertisseur}|$$
Pour respecter ce critère, on ajoute un **réseau d'amortissement (Damping)** : souvent un condensateur électrolytique (à forte ESR) ou un circuit RC en parallèle pour "écraser" le pic de résonance du filtre.



---

## 10. Topologies de Filtre et Choix Stratégique
* **Filtre LC :** Topologie de base. Attention à la résonance.
* **Filtre en $\pi$ (C-L-C) :** Très performant si la source et la charge ont des impédances élevées.
* **Filtre en T (L-C-L) :** Préférable si la source et la charge sont à basse impédance (ex: batteries, gros convertisseurs).
* **Multi-étages :** Nécessaire pour des atténuations extrêmes (> 60 dB), mais complexe à stabiliser.

---

## 11. Compromis de l'Ingénieur et Méthodologie de Design
Le design EMI est une gestion de compromis :
1.  **Volume vs Performance :** Plus l'atténuation demandée est basse en fréquence, plus les inductances sont volumineuses.
2.  **Coût :** Les noyaux hautes performances (Nanocristallin) sont chers.
3.  **Échauffement :** La résistance série (DCR) des selfs provoque des pertes Joule ($P = R \cdot I^2$).

**Méthode préconisée :**
* Mesure initiale "à nu" pour localiser les raies de bruit.
* Séparation MD/MC pour dimensionner spécifiquement les condos X/Y et la CMC.
* Vérification de la stabilité sous charge maximale et tension minimale.

---

## 12. Conclusion
Un filtre EMI efficace n'est pas simplement une collection de composants ajoutés à la hâte. C'est un système qui doit être conçu en tenant compte de la physique des couplages, des limites des composants réels et de la dynamique de la charge. Une attention particulière au layout (pistes courtes, séparation entrée/sortie) est souvent plus rentable que l'achat de composants coûteux.
