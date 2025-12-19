# Rapport Technique  
## Conception et Analyse des Filtres EMI

---

## 1. Quelle est la différence entre les émissions conduites et les émissions rayonnées, et pourquoi est-ce important pour la conception des filtres EMI ?

Les **émissions conduites** sont des perturbations électromagnétiques qui se propagent le long des conducteurs, comme les câbles d’alimentation ou les pistes du circuit imprimé. Elles sont dues aux courants de commutation et sont mesurées principalement à basse fréquence (de l’ordre de quelques centaines de kHz à quelques dizaines de MHz).

Les **émissions rayonnées** se propagent dans l’espace sous forme de champs électromagnétiques. Elles apparaissent lorsque les câbles ou les boucles de courant se comportent comme des antennes.

Cette distinction est importante car un bruit conduit mal filtré peut facilement se transformer en bruit rayonné. En réduisant les émissions conduites à la source grâce à un filtre EMI, on limite également les émissions rayonnées et on facilite la conformité CEM.

---

## 2. Qu’est-ce que le bruit en mode commun (MC) et en mode différentiel (MD) ?

Le **bruit en mode différentiel (MD)** correspond à un courant parasite qui circule entre deux conducteurs actifs (par exemple phase et neutre), dans des directions opposées. Il est directement lié au fonctionnement normal du convertisseur, notamment aux courants de ripple.

Le **bruit en mode commun (MC)** correspond à un courant parasite qui circule dans le même sens sur tous les conducteurs et qui revient par la terre ou le châssis via des capacités parasites. Il est souvent généré par les variations rapides de tension dans le circuit.

Ces deux modes sont différents par leur chemin de propagation et doivent être traités séparément.

---

## 3. Pourquoi est-il nécessaire de filtrer à la fois les interférences en mode commun et en mode différentiel ?

Il est nécessaire de filtrer à la fois le mode commun et le mode différentiel car ils ne se propagent pas de la même manière et ne sont pas atténués par les mêmes composants.

Si un seul mode est filtré, l’autre peut rester suffisamment important pour provoquer des perturbations ou un non-respect des normes CEM. De plus, des asymétries dans le circuit peuvent transformer une partie du bruit différentiel en bruit commun, ce qui rend indispensable un filtrage complet des deux modes.

---

## 4. Expliquez la différence entre fonction de transfert / atténuation et perte d’insertion d’un filtre EMI

La **fonction de transfert** (ou atténuation) décrit le comportement théorique du filtre, souvent calculé avec des composants idéaux et des conditions simplifiées.

La **perte d’insertion** correspond à l’atténuation réellement mesurée lorsque le filtre est inséré dans un système réel, en comparant le niveau de bruit avec et sans le filtre. Elle dépend des impédances de la source et de la charge et représente la grandeur utilisée lors des essais CEM.

---

## 5. Quels sont les composants typiques utilisés dans les filtres EMI et quel est leur rôle ?

Les filtres EMI utilisent généralement :

- des **selfs de mode commun**, qui bloquent le bruit en mode commun tout en laissant passer le courant utile ;
- des **condensateurs X**, placés entre les lignes, pour filtrer le bruit en mode différentiel ;
- des **condensateurs Y**, placés entre les lignes et la terre, pour évacuer le bruit en mode commun ;
- parfois des **inductances série** pour renforcer le filtrage différentiel.

Chaque composant agit sur un type précis de perturbation.

---

## 6. Comment les éléments parasites des composants réels affectent-ils les performances d’un filtre EMI ?

Les composants réels ne sont pas idéaux et possèdent des éléments parasites, comme une inductance associée à un condensateur ou une capacité associée à une inductance.

À haute fréquence, ces parasites peuvent réduire fortement l’efficacité du filtre, voire inverser le comportement attendu du composant. C’est pourquoi le choix des composants et leur implantation sur le circuit imprimé sont essentiels pour garantir un filtrage efficace.

---

## 7. Comment l’insertion d’un filtre EMI affecte-t-elle le reste du système ?

L’ajout d’un filtre EMI modifie le comportement électrique de la chaîne d’alimentation. Si le filtre est mal conçu, il peut provoquer des oscillations ou des instabilités dans le fonctionnement du convertisseur.

Il est donc important de s’assurer que le filtre EMI ne perturbe pas le fonctionnement normal du système et qu’il reste compatible avec le convertisseur qu’il protège.

---

## 8. Quels compromis un ingénieur doit-il considérer lors de la conception d’un filtre EMI ?

Lors de la conception d’un filtre EMI, plusieurs compromis doivent être pris en compte :

- la performance de filtrage ;
- le volume et le coût des composants ;
- les pertes électriques et l’échauffement ;
- les contraintes de sécurité, notamment liées aux condensateurs reliés à la terre.

Un bon filtre EMI est donc un compromis entre efficacité, sécurité et faisabilité pratique.

---

## 9. Quelles méthodologies de test sont utilisées pour évaluer les émissions conduites ?

Les émissions conduites sont mesurées à l’aide d’un montage de test normalisé comprenant un **LISN** et un récepteur EMI ou un analyseur de spectre.

Le dispositif testé est alimenté via le LISN, ce qui permet de mesurer uniquement les perturbations générées par l’équipement, dans des conditions reproductibles et conformes aux normes.

---

## 10. Qu’est-ce qu’un LISN et quel est son rôle dans un essai en émissions conduites ?

Le **LISN (Line Impedance Stabilization Network)** est un réseau utilisé lors des tests CEM pour :

- stabiliser l’impédance vue par l’équipement testé ;
- isoler le réseau électrique des perturbations générées par l’appareil ;
- permettre la mesure du bruit conduit à l’aide d’un récepteur EMI.

Il garantit la fiabilité et la répétabilité des mesures.

---

## 11. Expliquez la différence d’approche entre la conception d’un filtre EMI et un filtre analogique classique

Un filtre analogique classique est conçu pour transmettre un signal utile avec le moins de distorsion possible, en cherchant souvent une adaptation d’impédance.

Un filtre EMI, au contraire, a pour objectif de bloquer ou détourner les signaux parasites. Il exploite volontairement des différences d’impédance afin d’empêcher la propagation du bruit, sans se soucier de la transmission d’un signal utile.

---

## 12. Quel rôle l’impédance de charge du système joue-t-elle dans la conception du filtre ?

L’impédance de charge influence l’efficacité du filtre EMI et son interaction avec le système. Une charge mal prise en compte peut réduire l’atténuation du filtre ou provoquer des instabilités.

Il est donc nécessaire de concevoir le filtre en tenant compte du type de charge et du comportement global du système.

---

## 13. Décrivez les topologies de filtre EMI passives de base

Les topologies de filtres EMI passifs les plus courantes sont :

- le **filtre LC**, qui combine une inductance et un condensateur ;
- le **filtre en π (C-L-C)**, qui offre une meilleure atténuation lorsque les impédances sont élevées ;
- le **filtre en T (L-C-L)**, plus adapté aux faibles impédances.

Ces filtres agissent en bloquant les courants parasites et en les dérivant vers la masse ou la terre.
