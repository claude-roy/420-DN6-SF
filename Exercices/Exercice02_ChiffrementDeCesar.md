# Exercice 2 – Chiffrement de César

## Informations

**Évaluation** : formative  
**Type de travail** : individuel  
**Durée** : 2 heures  
**Système d’exploitation** : Windows, Linux ou MAC  
**Environnement** : python  

## Objectifs

Cet exercice a pour objectifs :

- D’une première approche a programmé un algorithme de chiffrement.  
- Comprendre le chiffrement de César.

Dans cet exercice, vous allez programmer votre premier algorithme de chiffrement, l’algorithme de chiffrement de César.

### Partie 1 : Implémentation du chiffrement de César

Pour implémenter un algorithme de chiffrement, on doit penser au chiffrement, au déchiffrement et à la clé. Vous devez comprendre qu’il y a plusieurs solutions à un problème surtout en programmation, donc je vais vous présenter une de ces solutions.

#### Étape 1 : Le chiffrement

1. Nous allons nous créer une fonction de chiffrement. Cette fonction aura besoin d’une clé de mappage, on commence par créer une fonction qui génère une clé.

Nous allons passer à cette fonction un entier qui est en fait la vraie clé. L’entier peut avoir la valeur que l’on veut, mais, dû au fait de la fonction cyclique de l’alphabète, la clé sera entre 0 et 25.

Également, le chiffrement de César utilise seulement des lettres majuscules (condition pour notre test).

```python
def generate_key(n):
	"""
	generate_key(n)
	Fonction qui génère une clé pour le chiffrement
	On passe un entier qui est en fait la vraie clé.
	Paramètres:
	n (int) : entier, clé de chiffrement
	
	Return
	Dictionnaire : la clé de mappage
	"""
	# Lettres utilisées pour le mappage
	letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
	
	# Nous allons utiliser un dictionnaire pour faire le mappage
	key = {}
	cnt = 0
	
	# Génère la clé
	for c in letters:
		# Le modulo permet de gérer un nombre qui déborde du nombre de lettre
		# Le modulo permet de redémarrer au début si la valeur de c est
		# plus grande que 25
		key[c] = letters[(cnt + n) % len(letters)]
		cnt += 1
	
	return key

# Vérifions que notre clé est bien générée
key = generate_key(3)
print(key)
``` 

Exécutez votre programme pour vérifier que vous avez bien une clé.

2. La clé « mappage » fonctionne, donc nous pouvons créer notre fonction de chiffrement.

Le message à mapper peut contenir des espaces ou d’autres caractères qui ne sont pas dans notre alphabète (les 26 lettres majuscules), il faut donc en tenir compte.

Ajouter la fonction de chiffrement à votre code (après la fonction <code>generate\_key(n)</code>), ainsi que le code pour la vérifier.

```python
def encrypt(key, message):

	"""
	encrypt(key, message)
	Fonction qui chiffre le message.
	
	Paramètres :
	key (dict): clé de mappage.
	message (string): mesage à chiffrer

	Return :
	string : le message chiffré
	"""
	
	# Va contenir le message chiffré
	secret = ""
	
	# Vous devez créer une boucle for qui vérifie si le caractère est dans
	# dans la clé de mappage. Si oui, on le substitue. Sinon, on le
	# le remet tel quel.
	--- code à ajouter ---
	
	return secret

# Vérifions que notre clé est bien générée
key = generate_key(3)
print(key)

# Vérifions que le chiffrement fonctionne
message = "AINSI VA LA VIE"
secret = encrypt(key, message)
print(secret)
``` 

Exécutez votre code. Utiliser la roue de chiffrement (<https://inventwithpython.com/cipherwheel/>) en ligne pour vérifier votre message chiffré.

#### Étape 2 : Le déchiffrement

1. Nous allons commencer par voir s’il est possible de déchiffrer le message avec la clé de mappage. Donc, chiffrer de nouveau le message.

Ajoutez le code suivant à votre code :

```python
message = encrypt(key, secret)
print (message)
```

Le message est-il déchiffré ?

2. Nous devons générer une clé de mappage différent pour déchiffrer. Nous savons que notre alphabète à 26 lettres, donc essayons de soustraire la clé du nombre de caractères dans notre alphabète.

Modifiez votre code comme suit :

```python
dkey = generate_key(26-3)
message = encrypt(dkey, secret)
print (message)
```

Le message est-il déchiffré ?

3. Même si la méthode précédente fonctionne, il est préférable d’avoir une fonction de déchiffrement : ça va vous aider pour les autres algorithmes. Nous allons générer une clé de déchiffrement.

Ajoutez une fonction <code>generate\_dkey(key)</code> au début de votre code qui inverse les paires clé:valeur de toutes les entrées de la clé de mappage.

Après avoir créé la fonction <code>generate\_dkey(key)</code>, modifiez votre code comme suit :

```python
dkey = generate_dkey(key)
message = encrypt(dkey, secret)
print (message)
``` 

Le message est-il déchiffré ?

### Partie 2 : Attaque sur le chiffrement de César

Avec le chiffrement de César, l’important est de garder l’algorithme secret. Si l’attaquant découvre l’algorithme, comme il n’y a que 26 clés et il connait la langue du message, il lui sera facile de « casser » le chiffrement.

Le principe de Kerckhoff : le principe est qu'un cryptosystème doit être sécurisé, même si tout ce qui concerne le système, à l'exception de la clé, est de notoriété publique.[[1]](#footnote-1) Donc, un attaquant ne doit pas pouvoir découvrir le message chiffré même s’il connait l’algorithme.

#### Étape 1 : Casser le chiffrement de César

1. Pour casser le chiffrement de César, on sait d’avance que nous avons seulement 26 clés possibles, donc il suffit de les essayer tous. Ce qui est assez facile en programmation.

Remplacer la dernière partie de votre code, celle qui déchiffre le message chiffré et la remplacer par le code d’attaque suivant.

```python
# Attaque sur le chiffrement de César
print("####################################")
print("Attaque sur le chiffrement de César")
for i in range(26):
	dkey = generate_key(i)
	message = encrypt(dkey, secret)
	print(message)
print("####################################")
``` 

Pouvez-vous trouver le message original ?

Compétences développées

| **FW19** – Distinguer les éléments de la cryptographie et de la cryptanalyse. | **FW19 # 1** – Reconnaître les étapes historiques de la cryptographie.<br>  **FW19 # 2** – Comparer les différentes méthodes actuelles de cryptographie.<br>  **FW19 # 3** – Expliquer les éléments de la cryptanalyse. |
| :--- | :--- |

**Note** : les compétences sont développées en partie.

## Références

1. <https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle> [↑](#footnote-ref-1)