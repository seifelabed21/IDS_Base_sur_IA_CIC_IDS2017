# Système de détection d'intrusions basé sur l'IA

Projet de classification intelligente du trafic réseau à partir du jeu de données [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html). Le notebook entraîne un modèle de Machine Learning capable de distinguer le trafic normal de plusieurs catégories d'attaques réseau.

> **Nature du projet :** prototype académique de détection multi-classe. Les résultats dépendent du jeu de données et du protocole d'évaluation utilisés ; ils ne constituent pas une garantie de détection en environnement réel.

## 1. Objectifs

Ce projet explore l'utilisation de l'intelligence artificielle pour analyser des flux réseau et reconnaître des comportements malveillants. Il couvre les étapes suivantes :

- chargement et fusion des fichiers CSV de CIC-IDS2017 ;
- nettoyage des valeurs manquantes et infinies ;
- analyse exploratoire des classes et des caractéristiques réseau ;
- suppression des valeurs aberrantes ;
- encodage des étiquettes d'attaque ;
- standardisation des variables numériques ;
- entraînement d'un classifieur Random Forest ;
- évaluation avec des métriques de classification et une matrice de confusion ;
- simulation de prédictions sur de nouveaux flux réseau.

## 2. Jeu de données

CIC-IDS2017 est un jeu de données de trafic réseau développé par le Canadian Institute for Cybersecurity. Il contient des flux bénins et plusieurs scénarios d'attaque représentés sous forme de caractéristiques numériques et d'étiquettes.

Les catégories observées dans le notebook comprennent notamment :

- `BENIGN` ;
- attaques DoS et DDoS ;
- balayage de ports (`PortScan`) ;
- attaques par force brute FTP et SSH ;
- attaques web ;
- infiltration ;
- botnet et Heartbleed.

Les fichiers CSV ne sont pas inclus dans le dépôt GitHub en raison de leur taille. Pour reproduire le projet, télécharger les fichiers depuis la source officielle et les placer dans :

```text
CIC-IDS2017/
```

Le notebook charge automatiquement tous les fichiers `.csv` présents dans ce dossier.

## 3. Méthode

Le pipeline suit les étapes suivantes :

```text
CIC-IDS2017 (fichiers CSV)
            |
            v
Nettoyage et fusion des flux
            |
            v
Analyse exploratoire et traitement des valeurs aberrantes
            |
            v
Encodage des étiquettes + StandardScaler
            |
            v
Séparation entraînement / test
            |
            v
Random Forest multi-classe
            |
            v
Classification, matrice de confusion et simulation IDS
```

Le modèle utilise un `RandomForestClassifier` avec 100 arbres. Les données sont séparées avec `test_size=0.2` et `random_state=42`.

## 4. Résultats observés

Lors de l'exécution présente dans le notebook :

- **Données fusionnées avant nettoyage :** 2 830 743 flux ;
- **Données après nettoyage :** 2 827 876 flux ;
- **Données après traitement des valeurs aberrantes :** 328 770 flux ;
- **Précision globale observée :** 99,85 % ;
- **Matrice de confusion :** 15 classes.

La précision globale est élevée, mais les classes très rares disposent de peu d'exemples. Le rapport de classification montre notamment des performances plus faibles pour certaines catégories minoritaires. Une validation plus robuste devrait inclure une séparation par scénario ou par période, des métriques macro, et un protocole limitant les fuites entre flux similaires.

## 5. Organisation du dépôt

```text
.
|-- AI_Based_IDS.ipynb              Notebook complet du pipeline
|-- CIC-IDS2017/                    CSV téléchargés localement
|-- docs/
|   `-- Rapport_IDS_IA.pdf           Rapport du projet
|-- requirements.txt                 Dépendances Python
|-- .gitignore
`-- README.md
```

Les fichiers `.pkl` générés, les CSV du dataset et les checkpoints Jupyter sont volontairement exclus du dépôt. Le modèle et le scaler peuvent être régénérés à la fin du notebook avec `joblib`.

## 6. Prérequis

- Python 3.9 ou version ultérieure ;
- Jupyter Notebook ou JupyterLab ;
- les fichiers CSV de CIC-IDS2017 ;
- les dépendances listées dans `requirements.txt`.

Créer un environnement virtuel et installer les dépendances :

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Sous Linux ou macOS :

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

## 7. Exécuter le projet

1. Télécharger les CSV de CIC-IDS2017 et les placer dans `CIC-IDS2017/`.
2. Démarrer Jupyter depuis la racine :

   ```bash
   jupyter notebook
   ```

3. Ouvrir `AI_Based_IDS.ipynb`.
4. Exécuter les cellules dans l'ordre :
   - import et chargement des données ;
   - visualisation et nettoyage ;
   - encodage et standardisation ;
   - entraînement du modèle ;
   - évaluation ;
   - simulation IDS ;
   - sauvegarde du modèle et du scaler.

Les artefacts générés sont :

```text
ids_model.pkl
scaler.pkl
```

Ils restent locaux et ne doivent pas être versionnés.

## 8. Technologies

- Python ;
- Jupyter Notebook ;
- Pandas et NumPy ;
- Scikit-learn ;
- Matplotlib et Seaborn ;
- CIC-IDS2017 ;
- Random Forest.

## 9. Limites

- Le modèle est entraîné et évalué sur un jeu de données hors ligne.
- La précision globale est influencée par le déséquilibre important entre les classes.
- Les attaques rares disposent de peu d'exemples pour une évaluation fiable.
- Le protocole actuel ne représente pas nécessairement un trafic réseau réel ou inédit.
- Une utilisation opérationnelle nécessiterait une validation temporelle, des données récentes, un suivi de dérive et une analyse des faux positifs.

## 10. Documentation

Le rapport du projet est disponible dans [`docs/Rapport_IDS_IA.pdf`](docs/Rapport_IDS_IA.pdf).

## 11. Référence du dataset

Canadian Institute for Cybersecurity, **Intrusion Detection System (IDS) Dataset — CIC-IDS2017** :

<https://www.unb.ca/cic/datasets/ids-2017.html>
