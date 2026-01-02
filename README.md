# Energy-Demand-Forecast

##  Description du Projet

Ce projet implémente un modèle LSTM (Long Short-Term Memory) pour prédire la consommation électrique horaire d'un réseau intelligent (Smart Grid) géré par la start-up **EcoVolt**.

L'objectif est d'anticiper la consommation de l'heure suivante (t+1) à partir des 24 heures précédentes, en tenant compte du mix énergétique (nucléaire, solaire, éolien, etc.).

---

##  Objectifs Pédagogiques

- Comprendre le fonctionnement des réseaux de neurones récurrents (RNN)
- Implémenter un modèle LSTM multivarié sur séries temporelles
- Maîtriser le preprocessing spécifique aux données temporelles
- Justifier les choix d'architecture et d'évaluation

---

##  Dataset

Le dataset contient des mesures horaires avec les colonnes suivantes :

### Variable cible
- **Consumption** : Consommation électrique totale (MW)

### Variables explicatives
- **Production** : Production totale
- **Nuclear** : Production nucléaire
- **Wind** : Production éolienne
- **Solar** : Production solaire
- **Hydroelectric** : Production hydroélectrique
- **Coal** : Production charbon
- **Oil and Gas** : Production pétrole et gaz
- **Biomass** : Production biomasse

### Métadonnées
- **DateTime** : Date et heure de la mesure (granularité horaire)

---

## 🛠️ Technologies Utilisées

```python
- Python 3.x
- TensorFlow / Keras
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
```

---

##  Structure du Projet

```
EcoVolt-LSTM-Prediction/
│
├── dataset/
│   └── energy_consumption.csv
│
├── notebooks/
│   └── EcoVolt_LSTM_Prediction.ipynb
│
├── README.md
└── requirements.txt
```

---

##  Installation et Exécution





 **Cloner le repository** :
   ```bash
   git clone https://github.com/elhidarinouhayla/Energy-Demand-Forecast.git
   cd Energy-Demand-Forecast
   ```


---

##  Méthodologie

### 1. Préparation des Données
- Conversion de `DateTime` en index temporel
- Tri chronologique des données (pas de shuffle !)
- Normalisation avec `MinMaxScaler` (échelle [0,1])

### 2. Création des Séquences
- Fenêtre glissante de **24 heures**
- Format 3D : (samples, timesteps, features)
- Exemple : (10000, 24, 9) = 10000 séquences de 24h avec 9 variables

### 3. Split Train/Test Temporel
- **80%** pour l'entraînement (données passées)
- **20%** pour le test (données futures)
- Pas de mélange aléatoire pour éviter la fuite de données

### 4. Architecture LSTM


### 5. Entraînement
- **Optimizer** : Adam
- **Loss** : MSE (Mean Squared Error)
- **Epochs** : 30
- **Batch size** : 32
- **Validation split** : 20%

---

## Résultats

### Performances du Modèle

| Métrique | Valeur |
|----------|--------|
| **Train Loss (final)** | 4.21e-04 |
| **Val Loss (final)** | 2.79e-04 |
| **Temps d'entraînement** | ~8-9 minutes (30 epochs) |

### Évolution de l'Entraînement

```
Epoch 1/30  : loss: 0.0191 - val_loss: 0.0027
Epoch 10/30 : loss: 6.96e-04 - val_loss: 4.32e-04
Epoch 20/30 : loss: 4.79e-04 - val_loss: 3.73e-04
Epoch 30/30 : loss: 4.21e-04 - val_loss: 2.92e-04
```

### Observations
**Convergence rapide** : Loss divisée par 45 en 30 epochs  
**Bonne généralisation** : Val_loss légèrement inférieur au train_loss  
**Légères oscillations** : Entre epochs 11-28 (peut bénéficier d'Early Stopping)

### Visualisation

Le graphique **Réel vs Prédit** montre une superposition quasi-parfaite des courbes, indiquant que le modèle capture excellemment :
- Les cycles journaliers de consommation
- Les pics de demande
- Les variations liées au mix énergétique
