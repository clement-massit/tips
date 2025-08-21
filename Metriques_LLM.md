# Metriques LLM

## 1.Perplexity (PPL)  

Mesure à quel point le modèle est "surpris" par les données de validation/test.


Formule : $$PPL = \left|e^{Loss}\right|$$  

Plus la perplexité est basse, mieux le modèle prédit les tokens du dataset. PPL = 10 >> PPL = 50  

Limite : une perplexité faible est différent de toujours meilleure qualité de génération (elle mesure la prédiction de tokens, pas la __cohérence sémantique !__ )


## 2. Exact Match (EM)  

Pour une tâche de QA ou Classification, pourcentage de réponses exactement identiques à la vérité  

_Example_ :  
* Label attendu = "42"
* Réponse modèle = "42" → ✅
* Réponse modèle = "the answer is 42" → ❌

Utile pour des tâches fermées (code, QA court, classification).  

## 3. F1 Score  

Mesure l'équilibre entre précision et rappel.


### Précision

Mesure de la proportion de "bonne" prédiction parmi toutes les prédictions faites.

Formule : $$Précision = \frac{TP}{TP + FP}$$

### Rappel (ou Sensibilité)

Mesure de la proportion tous les résultats positifs réels qui ont été correctement prédits.

Formule : $$Rappel = \frac{TP}{TP + FN}$$

### F1 Score

Moyenne harmonique pondérée de précision et de rappel.

Formule : $$F1 = \frac{2 * Précision * Rappel}{Précision + Rappel}$$  

utile en __extraction d’entités__, __classification multi-label__.

S'il y a une augmentation de l'un des deux scores, c'est que l'autre score diminue.

Note : un score de 1 signifie que les deux scores sont parfaits (ou qu'il y a aucune prédiction ni réponse réelle).

Un score de 0.5 est une mauvaise performance.

Un score de 0.66 serait une performance moyenne.   


## 4. BLEU / ROUGE / METEOR (pour le texte génératif)  

### BLEU  
_BiLingual Evaluation Understudy_: compare les __ngrams__ (séquences de n mots consécutifs) générés vs. référence (très utilisé en traduction).  
 Il calcule la précision en tenant compte du nombre de n-grammes du texte généré qui correspondent à ceux du ou des textes de référence. Le score de précision est ensuite modifié par une pénalité de brièveté afin d’éviter de favoriser les traductions plus courtes.  
Score compris entre 0 et 1

Interprétation :

| Score | Interprétation |
| --- | --- |
| ﹤0.1 | Presque inutile |
| 0.1-0.19 | Difficile de saisir l'essentiel |
| 0.2-0.29 | L'essentiel est clair, mais comporte des erreurs grammaticales significatives. |
| 0.3-0.39 | Compréhensible pour de bonnes traductions |
| 0.4-0.49 | Des traductions de haute qualité |
| 0.5-0.59 | Des traductions de très haute qualité, adéquates et fluides |
| ≥0.6 | Une qualité souvent supérieure à celle des humains |


### ROUGE
_Recall Oriented Understudy for Gisting Evaluation_ : compare les recouvrements de phrases / phrases clés (souvent utilisé en résumé automatique).  

Principalement utilisé pour comparer un résumé généré d'un texte de référence (généralement humain). 

Pareil score entre 0 et 1  


### METEOR  
_Metric for Evaluation of Translation with Explicit Ordering_ : évalue la qualité du texte généré basé sur l'alignement entre le texte généré et le texte de référence.  La mesure est basée sur une moyenne harmonique d'un unigramme précision et rappel avec une pondération de rappel plus élevée que celle de précision.  

Même si la principale différence entre ROUGE et BLEU est que le score de ROUGE est centré sur le rappel, la mesure METEOR a été conçue pour résoudre les problèmes rencontrés dans les mesures BLEU et ROUGE les plus populaires et également pour produire une bonne corrélation avec la jugement humain au niveau de la phrase ou du segment.

## 5. BERTScore  

Compare les embeddings du texte généré avec ceux de la référence, en utilisant un modèle pré-entraîné (BERT par exemple).  

Avantages : capture mieux la proximité sémantique que BLEU et ROUGE. (proximité et distance cosine)  

Idéale pour des tâches de résumé, de génération de texte.  

## 6. Human Evaluation (Subjective)

Evaluation par des humains, notations par annotateurs humains (score 1-5 pour la cohérence, fluidité, pertinence, etc.). Pairwise comparison (A ou B ?).


---  

Classification → Accuracy, F1, AUC.

Génération de code → Exact Match + exécution du code (taux de réussite).

Résumé/QA → ROUGE, BERTScore.

Dialogue → Win-rate contre un modèle baseline.