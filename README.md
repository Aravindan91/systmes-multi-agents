# 🐜 Approches en Essaim pour le Problème de Patrouille Multi-Agents

[![NetLogo](https://img.shields.io/badge/NetLogo-7.0.3-brightgreen.svg)](https://ccl.northwestern.edu/netlogo/)
[![Master](https://img.shields.io/badge/Master-CNS%20%7C%20Syst%C3%A8mes%20Autonomiques-blue)](https://www.univ-evry.fr/)
[![Academic Year](https://img.shields.io/badge/Ann%C3%A9e-2025%2F2026-orange)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Ce projet implémente et compare deux approches d'**intelligence en essaim** basées sur la **stigmergie** pour résoudre le problème de patrouille continue dans des environnements contraints. Ce travail s'appuie sur l'article scientifique de référence :
> **Glad, A., Buffet, O., Ponty, J. L., Schneider, F., & Simonin, O. (2010).** *Swarm approaches for the patrolling problem.* IEEE SASO 2010.

---

## 📌 Présentation du Projet

Le **problème de patrouille** consiste à déployer une flotte d'agents (robots/drones) afin de visiter continuellement et le plus équitablement possible l'ensemble des cases libres d'un environnement. Contrairement aux méthodes centralisées qui souffrent d'explosion combinatoire, les approches décentralisées garantissent :
* **Scalabilité :** Efficacité conservée de 1 à 32 agents.
* **Robustesse :** Tolérance aux pannes sans point de défaillance unique.
* **Flexibilité :** Adaptation dynamique aux topologies complexes.

---

## 🧠 Algorithmes Implémentés

Les deux modèles reposent sur la métrique d'**oisiveté (*idleness*)** : chaque case incrémente son horloge interne à chaque tick, réinitialisée à 0 dès qu'un agent la traverse.

| Caractéristique | EVAP (*Evaporative Pheromone*) | CLInG (*Continuous Local Idleness Gradient*) |
| :--- | :--- | :--- |
| **Paradigme** | **Répulsion** (Descente de gradient) | **Attraction** (Montée de gradient) |
| **Principe** | L'agent dépose une trace phéromonale et fuit les zones récemment visitées. | Les cases propagent leur oisiveté dans l'espace (effet « radar » longue distance). |
| **Équation clé** | $q_{n+1} = q_n \times (1 - \rho)$ | $OP_i = \max [ O_i, \max(OP_j - \alpha - \beta \cdot I(j)) ]$ |
| **Points forts** | Très léger en calcul, évite les congestions, excellent rendement global. | Évite l'abandon de zones isolées, excellent avec peu d'agents. |
| **Optimisation** | Module de **Momentum** ($p = 0.8$) pour éviter les oscillations/demi-tours. | Système de **Double Buffering** pour fiabiliser la propagation synchrone. |

---

## 📊 Métriques d'Évaluation

1. **IGI (*Instantaneous Graph Idleness*) :** Moyenne de l'oisiveté sur toute la carte. Mesure le **rendement / l'efficacité globale**.
2. **IWI (*Instantaneous Worst Idleness*) :** Oisiveté maximale constatée sur une case. Mesure la **sécurité** (garantit l'absence de zone oubliée).
3. **Temps CPU :** Durée d'exécution réelle mesurée via le timer NetLogo.

---

## 🗺️ Cartes & Topologies Testées

Toutes les simulations sont exécutées sur un tore discret de **33 × 33 cases** (1 089 patches) :
* **Map A :** Espace ouvert (sans obstacle).
* **Map B :** Spirale continue (chemin unique, long couloir forcé).
* **Map C :** Obstacles aléatoires (densité 20% avec validation de connexité).
* **Map D :** Couloir central desservant 4 salles symétriques.
* **Map E :** Labyrinthe imbriqué à accès étroits.

---

## 📈 Synthèse des Résultats

D'après le plan d'expérience (100 simulations de 4 000 ticks lissées sur 5 runs statistiques) :

* **Rendement moyen (IGI) :** **EVAP l'emporte dans 17 cas sur 20**. La répulsion locale suffit à disperser efficacement les agents sans calcul superflu.
* **Sécurité & Oubli (IWI) :** **CLInG domine dans 17 cas sur 20**. À 1 seul agent, EVAP plafonne à 4 000 ticks (zones entières oubliées), tandis que CLInG maintient un IWI stable entre 1 300 et 1 700 ticks grâce à son appel à longue portée.
* **Coût computationnel :** EVAP est plus rapide dans **75% des cas** (complexité purement locale vs propagation itérative de CLInG).

---

## 🗂️ Structure du Dépôt

```bash
.
├── code_comm_v5.nlogox         # Modèle NetLogo interactif et automatisé
├── Rapport_Projet_SMA.pdf       # Rapport de recherche complet (13 pages, LaTeX)
├── README.md                   # Documentation du projet

