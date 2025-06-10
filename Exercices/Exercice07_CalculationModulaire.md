# Exercice 7 – Calculs modulaires

## Informations  

**Évaluation** : formative  
**Type de travail** : individuel  
**Durée** : 1 heure  
**Système d’exploitation** : Windows, Linux ou MAC  
**Environnement** : python  

## Objectifs

Cet exercice a pour objectifs :

* Comprendre le calcul modulaire.
* Implémenter un générateur.

Dans cet exercice, nous allons faire une implémentation simple de calcul modulaire pour mieux le comprendre. Nous allons également faire une implémentation d’un générateur.

### Partie 1 : Implémentation d’un calcul modulaire

De base, un ordinateur travaille avec du calcul modulaire, car il n’a pas une capacité de représenter des nombres entiers à l’infini. Le « stockage » mémoire est toujours limité dans un ordinateur, donc il doit toujours « tronquer » des nombres.

#### Étape 1 : Additions modulaires

1. Créer un nouveau document `CalculsModulaires.py`.

Nous allons commencer par un simple calcul modulaire.

```python
# CalculsModulaires.py

# On veut faire calcul 4 + 7 mod 12
val = 4 + 7 % 12
print("Notre résultat est : ", val)
```  

Exécutez votre code.

Quel est le résultat ?

<details>
<summary markdown="span">Réponse :</summary>

```bash
$ python3 CalculsModulaires.py
Notre résultat est :  11

```

</details>

Est-ce un résultat normal ?

<details>
<summary markdown="span">Réponse :</summary>

Oui.
</details>  

La question que l’on doit se poser c’est comment l’équation est-elle évaluée ?

Fait-il 4+7 et après le modulo. Ou, 7mod12 et après l’addition.

Ajoutez l’équation suivante à votre code et imprimez le résultat :

```python
val = 4 + 15 % 12
print("Notre résultat 2 est : ", val)
```  

Selon le résultat, le modulo est fait avant l’addition ou après ?

<details>
<summary markdown="span">Réponse :</summary>

Avant.
</details>  

Pour éviter l’ambiguïté, il est toujours préférable de mettre des parenthèses.

Ajoutez des parenthèses à votre addition, car c’est ce que nous désirons accomplir.

Exécutez votre code et vérifiez le résultat obtenu.

Vous devriez obtenir 5.

On peut essayer un dernier calcul :

```python
val = (4 \* 5) % 5
print("Notre résultat 3 est : ", val)
```  

### Partie 2 : Générateurs

Nous avons parlé de générateurs, regardons comment implémenter ça.

Étape 1 : implémenter un générateur

1. Créer un nouveau document `Generators.py`.

Voici ce qu’un générateur à l’air.

```python
# Generators.py

g = 2
for i in range(10):
	#On afficher i pour comparer les valeurs.
	print(i, (g**i) % 5)
```  

Vous remarquez que nous avons **généré** une séquence de nombres qui se répète, mais nous n’avons jamais un 0.

Vous pouvez essayer avec plus de valeurs, changer la valeur 10 pour 20.

Avec notre modulo et notre générateur, nous allons résoudre le problème de distribution de clé et la guerre dans le monde. OK! Peut-être pas la guerre dans le monde.

Compétences développées

| **FW19** – Distinguer les éléments de la cryptographie et de la cryptanalyse. | **FW19 # 1** – Reconnaître les étapes historiques de la cryptographie.<br>**FW19 # 2** – Comparer les différentes méthodes actuelles de cryptographie.<br>**FW19 # 3** – Expliquer les éléments de la cryptanalyse. |
| :--- | :--- |

**Note** : les compétences sont développées en partie.

Références

<https://www.geeksforgeeks.org/python-operators/>
