# Exercice 3 – Chiffrement de Substitution et analyse de fréquence

## Informations

**Évaluation** : formative  
**Type de travail** : individuel  
**Durée** : 3 heures  
**Système d’exploitation** : Windows, Linux ou MAC  
**Environnement** : python

## Objectifs

Cet exercice a pour objectifs :

* De programmer un algorithme de chiffrement.
* Comprendre le chiffrement de substitution.
* Une première approche à la cryptanalyse.

Dans cet exercice, vous allez programmer un algorithme de chiffrement, l’algorithme de chiffrement de Substitution. Vous allez également faire une première approche à de la cryptanalyse ou à une attaque sur du chiffrement.

## Partie 1 : Implémentation du chiffrement de Substitution

Le chiffrement de César était sécuritaire, seulement si l’algorithme n’était pas connu. Si l’algorithme est connu, la clé n’est pas assez complexe pour être sécuritaire. L’algorithme de chiffrement de substitution est semblable au chiffrement de César. Pour l’implémenter, on doit penser au chiffrement, au déchiffrement et à la clé. Cependant, on voudra une clé un peu plus complexe.

Dans le chiffrement de substitution, on recherche la possibilité d’un grand espace clé. Les possibilités de permutation croient de manière exponentielle avec la longueur de la clé. Par exemple, si l’on utilise l’alphabet comme clé, nous avons 26! clés possibles ou 403 291 461 126 605 635 584 000 000 permutations.

Note : Vous devez comprendre qu’il y a plusieurs solutions à un problème surtout en programmation, donc je vais vous présenter une de ces solutions.

### Étape 1 : Le chiffrement

1. De base, le chiffrement de César est un algorithme de substitution, donc nous allons utiliser ce que nous connaissons du chiffrement de César. Pour générer une clé, nous allons utiliser un nombre aléatoire. Python possède une fonction pour générer un nombre aléatoire : <https://docs.python.org/3/library/random.html?highlight=random#module-random>.  

**Note :** un « vrai » nombre aléatoire est un point important en cryptographie, la page de la fonction <code>random()</code> nous donne un avertissement sur ce point. Par contre, pour notre exercice, on peut se permettre d’utiliser cette fonction, car le chiffrement de substitution possède d’autres faiblesses.

Comme pour notre exemple du chiffrement de César, nous allons utiliser seulement des lettres majuscules (condition pour notre test).

```python
# SubstitutionCypher1.py

import random

def generate_key():
	"""
	generate_key()
	Fonction qui génère une clé pour le chiffrement
	On ne passe aucun paramètre, car nous allons utiliser
	une fonction aléatoire pour la générer.

	Paramètres:
	aucun
	
	Return
	Dictionnaire : la clé de mappage
	"""
	
	# Lettres utilisées pour le mappage
	letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
	
	# Une liste de nos lettres pour utiliser avec
	# la partie aléatoire.
	cletters = list(letters)
	
	# La clé est toujours un dictionnaire
	key = {}
	
	# Nous allons faire un mappage, mais plus intelligent.
	# Nous allons utiliser un mappage plus aléatoire.
	# Pour chaque lettre, nous allons utiliser une autre
	# lettre aléatoire de la liste cletters
	
	for c in letters:
		key[c] = cletters.pop(random.randint(0, len(cletters) - 1))
	return key

# Vérifions que notre clé est bien générée
key = generate_key()
print(key)
``` 

Exécutez votre programme pour vérifier que vous avez bien une clé.  

<details>
	<summary markdown="span">Résultat de l'exécution.</summary>
	
```bash
$ python3  SubstitutionCypher1.py
{'A': 'M', 'B': 'A', 'C': 'H', 'D': 'I', 'E': 'Z', 'F': 'P', 'G': 'Q', 'H': 'B', 'I': 'X', 'J': 'V', 'K': 'L', 'L': 'U', 'M': 'N', 'N': 'G', 'O': 'W', 'P': 'J', 'Q': 'R', 'R': 'C', 'S': 'Y', 'T': 'D', 'U': 'S', 'V': 'K', 'W': 'O', 'X': 'E', 'Y': 'F', 'Z': 'T'}

```  

</details>
Avez-vous une clé de mappage aléatoire ?  

<details>
	<summary markdown="spam">Réponse :</summary>
	Oui.
</details>


Que se passe-t-il lorsque vous exécutez de nouveau votre code ?  

<details>
	<summary markdown="spam">Réponse :</summary>
	Une nouvelle clé de mappage est générée.
</details>


2. La clé « mappage » fonctionne, donc nous pouvons créer notre fonction de chiffrement.

Le chiffrement est le même que celui de César : le chiffrement est un chiffrement par substitution.

Ajouter la fonction de chiffrement à votre code, ainsi que le code pour la vérifier.

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
	
	# Vous devez créer une boucle for qui vérifie si le
	# caractère est dans
	# dans la clé de mappage. Si oui, on le substitue. Sinon,
	# on le remet tel quel.
	for c in message:
		# On mappe seulement les caractères
		# de notre alphabète
		if c in key:
			secret += key[c]
		else:
			secret += c
	return secret

# Vérifions que le chiffrement fonctionne
message = "AINSI VA LA VIE"
secret = encrypt(key, message)
print(secret)
``` 

Exécutez votre code.  

Votre message est-il chiffré ?

<details>
	<summary markdown="spam">Réponse :</summary>
	Oui!

```bash
$ python3 SubstitutionCypher2.py
{'A': 'A', 'B': 'M', 'C': 'X', 'D': 'P', 'E': 'T', 'F': 'Y', 'G': 'E', 'H': 'G', 'I': 'O', 'J': 'Q', 'K': 'N', 'L': 'I', 'M': 'C', 'N': 'H', 'O': 'Z', 'P': 'K', 'Q': 'S', 'R': 'B', 'S': 'D', 'T': 'F', 'U': 'R', 'V': 'L', 'W': 'J', 'X': 'V', 'Y': 'W', 'Z': 'U'}
AOHDO LA IA LOT
```  

</details>  

Que se passe-t-il lorsque vous exécutez de nouveau votre code ? Pourquoi?  

<details>
	<summary markdown="spam">Réponse :</summary>
	Une nouveau message chiffré. Car, la clé de mappage change à chaque fois.
</details>  

### Étape 2 : Le déchiffrement

1. Comme pour le chiffrement, le déchiffrement sera identique à celui de César.  

Ajoutez la fonction <code>generate_dkey(key)</code>, de votre chiffrement de César, au début de votre code.

```python
### Code à ajouter par vous.
```  

Ajoutez à la fin la partie pour vérifier le déchiffrement :  

```python
dkey = generate_dkey(key)
message = encrypt(dkey, secret)
print (message)
```  

Le message est-il déchiffré ?

<details>
	<summary markdown="spam">Réponse :</summary>
	Oui.

```python3
$ python3 SubstitutionCypher3.py
{'A': 'O', 'B': 'N', 'C': 'M', 'D': 'G', 'E': 'R', 'F': 'K', 'G': 'B', 'H': 'W', 'I': 'I', 'J': 'C', 'K': 'V', 'L': 'E', 'M': 'Q', 'N': 'F', 'O': 'J', 'P': 'P', 'Q': 'A', 'R': 'L', 'S': 'U', 'T': 'Y', 'U': 'H', 'V': 'D', 'W': 'S', 'X': 'X', 'Y': 'Z', 'Z': 'T'}
OIFUI DO EO DIR
AINSI VA LA VIE
``` 

</details>  


## Partie 2 : Cryptanalyse du chiffrement de substitution et analyse par fréquence

Au premier abord, il semble que le chiffrement de substitution est sécuritaire : ça nous donne une clé de 88 bits, 288 est 309 485 009 821 345 068 724 781 056 ce qui est proche de 26!. Avec une attaque de force brute, on parle de 100 millions d’années pour casser un texte avec une clé de 88 bits. Pourquoi alors n’est-il pas considéré comme sécuritaire ?

Voici un exemple d’un texte chiffré avec le chiffrement de substitution.  

```bash
LRVMNIR BPR SUMVBWVR JX BPR LMIWV YJERYRKBI JX QMBM WI
BPR XJVNI MKD YMIBRUT JX IRHX WI BPR RIIRKVR JX
YMBINLMTMIPW UTN QMUMBR DJ W IPMHH BUT BJ RHNVWDMBR BPR
YJERYRKBI JX BPR QMBM MVVJUDWKO BJ YT WKBRUSURBMBWJK
LMIRD JK XJUBT TRMUI JX IBNDT
WB WI KJB MK RMIT BMIQ BJ RASHMWK RMVP YJERYRKB MKD WBI
IWOKWXWVMKVR MKD IJYR YNIB URYMWK NKRASHMWKRD BJ OWER M
VJYSHRBR RASHMKMBWJK JKR CJNHD PMER BJ LR FNMHWXWRD MKD
WKISWURD BJ INVP MK RABRKB BPMB PR VJNHD URMVP BPR IBMBR
JX RKHWOPBRKRD YWKD VMSMLHR JX URVJOKWGWKO IJNKDHRII
IJNKD MKD IPMSRHRII IPMSR W DJ KJB DRRY YTIRHX BPR XWKMH
MNBPJUWBT LNB YT RASRUWRKVR CWBP QMBM PMI HRXB KJ DJNLB
BPMB BPR XJHHJCWKO WI BPR SUJSRU MSSHWVMBWJK MKD
WKBRUSURBMBWJK W JXXRU YT BPRJUWRI WK BPR PJSR BPMB BPR
RIIRKVR JX JQWKMCMK QMUMBR CWHH URYMWK WKBMVB
```  

À première vue, ce texte ne nous dit pas grand-chose. Cependant, on sait que le texte est écrit en anglais et, plus on en connaît sur un texte chiffré, plus il est facile à casser. On sait également que dans une langue écrite, certaines lettres sont utilisées plus souvent que d’autres. Peut-on utiliser cette information pour nous aider à casser ce texte ?  

On sait que l’utilisation des lettres dans un texte n’est pas égale et l’algorithme de substitution ne fait que substituer un caractère pour un autre. L’annexe 1 nous donne la fréquence d’utilisation des lettres dans l’écriture anglaise. On peut voir que les lettres E, T et A sont les plus utilisées.  

### Étape 1 : Analyse du texte chiffré

1. Pour débuter notre analyse, regardons ce que nous connaissons de ce texte.  

Nous savons que c’est le chiffrement de substitution : aujourd’hui, on connaît l’algorithme, c’est la clé que l’on garde secrète. Nous savons que la langue est l’anglais : souvent, on connaît la source et la destination, donc on connaît la langue de communication. Finalement, nous savons que l’utilisation des lettres dans un texte n’est pas égale.

Nous allons écrire un script qui va analyser l’utilisation des lettres dans notre texte et nous pourrons alors le comparer à l’annexe 1 pour trouver des équivalences.

Utiliser le fichier [Exercice03_AnalyseDeFrequence.py](extra/Exercice03_AnalyseDeFrequence.py) et ajouter le code suivant.

```python
# On utilise un dictionnaire pour enregistrer
# la fréquence de nos lettres
freq = {}

# On va utiliser un compteur pour compter les lettres
# de notre alphabet dans le texte.
# On va mettre le compteur à 0 pour chacune des lettres
for c in alphabet:
	freq[c] = 0

# On doit également connaître le nombre
# de caractères dans le texte
letter_count = 0

# Nous allons compter la fréquence de chacun
# des caractères dans le texte et le
# nombre de caractères dans le texte
for c in secret:
	if c in freq:
		freq[c] += 1
		letter_count += 1

# On doit maintenant connaître le pourcentage
# d'utilisation de chacun des caractères
for c in freq:
	freq[c] = round(freq[c]/letter_count, 4)

# On imprime le résultat sur 3 colonnes
new_line_count = 0
for c in freq:
	print(c, ":", freq[c], " ", end='')
	if new_line_count % 3 == 2:
		print()
	new_line_count += 1
``` 

En consultant l’annexe 1, pouvez-vous identifier certaines lettres plus utilisées ?

<details>
	<summary markdown="spam">Réponse :</summary>
	Oui. Le caractère R est le plus utilisé, donc il devrait être le E. Le deuxième le plus utilisé est le caractère B, donc il devrait être le T. Le troisième est M, donc il devrait être A.

```python3
$ python3 AnalyseDeFrequence1.py
A : 0.0077  B : 0.1053  C : 0.0077  
D : 0.0356  E : 0.0077  F : 0.0015  
G : 0.0015  H : 0.0356  I : 0.0635  
J : 0.0743  K : 0.0759  L : 0.0124  
M : 0.096  N : 0.0263  O : 0.0108  
P : 0.0464  Q : 0.0108  R : 0.13  
S : 0.0263  T : 0.0201  U : 0.0372  
V : 0.0341  W : 0.0728  X : 0.031  
Y : 0.0294  Z : 0.0  %   

``` 

</details>  

2. Il est possible de faire la comparaison du résultat précédent avec l’annexe 1 manuellement, mais pourquoi le faire lorsque nous avons un ordinateur?  

On veut comparer la différence entre les fréquences d’utilisations des caractères et notre tableau de l’annexe 1. Cependant, nous n’avons pas toujours une équivalence parfaite.  

Comme notre code peut resservir, pourquoi ne pas restructurer notre code en une classe.  

Dans un nouveau document Python, reproduire le code suivant :  

```python
# AnalyseDeFrequence2.py

secret = """LRVMNIR BPR SUMVBWVR JX BPR LMIWV YJERYRKBI JX QMBM WI
BPR XJVNI MKD YMIBRUT JX IRHX WI BPR RIIRKVR JX
YMBINLMTMIPW UTN QMUMBR DJ W IPMHH BUT BJ RHNVWDMBR BPR
YJERYRKBI JX BPR QMBM MVVJUDWKO BJ YT WKBRUSURBMBWJK
LMIRD JK XJUBT TRMUI JX IBNDT
WB WI KJB MK RMIT BMIQ BJ RASHMWK RMVP YJERYRKB MKD WBI
IWOKWXWVMKVR MKD IJYR YNIB URYMWK NKRASHMWKRD BJ OWER M
VJYSHRBR RASHMKMBWJK JKR CJNHD PMER BJ LR FNMHWXWRD MKD
WKISWURD BJ INVP MK RABRKB BPMB PR VJNHD URMVP BPR IBMBR
JX RKHWOPBRKRD YWKD VMSMLHR JX URVJOKWGWKO IJNKDHRII
IJNKD MKD IPMSRHRII IPMSR W DJ KJB DRRY YTIRHX BPR XWKMH
MNBPJUWBT LNB YT RASRUWRKVR CWBP QMBM PMI HRXB KJ DJNLB
BPMB BPR XJHHJCWKO WI BPR SUJSRU MSSHWVMBWJK MKD
WKBRUSURBMBWJK W JXXRU YT BPRJUWRI WK BPR PJSR BPMB BPR
RIIRKVR JX JQWKMCMK QMUMBR CWHH URYMWK WKBMVB
"""

class Attaque:
	"""
	Classe Attaque
	Classe pour analyse cryptographique (attaque) d'un
	texte chiffré par chiffrement de substitution
	"""
	def __init__(self):
		# On doit initialiser notre alphabet et
		# la fréquence
		self.alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
		
		# On utilise un dictionnaire pour enregistrer
		# la fréquence de nos lettres
		self.freq = {}
	
	def calculate_freq(self, secret):
		"""
		calculate_freq(self, secret)
		Méthode qui calcule la fréquence d'un
		caractère dans le texte
		
		Paramètres:
		self : notre objet d'Attaque
		secret (string) : le message secret
		
		Return
		Dictionnaire : la clé de mappage
		"""
		
		# On va utiliser un compteur pour compter les lettres
		# de notre alphabet dans le texte.
		# On va mettre le compteur à 0 pour chacune des lettres
		for c in self.alphabet:
			self.freq[c] = 0
		
		# On doit également connaître le nombre
		# de caractères dans le texte
		letter_count = 0
		
		# Nous allons compter la fréquence de chacun
		# des caractères dans le texte et le
		# nombre de caractères dans le texte
		for c in secret:
			if c in self.freq:
				self.freq[c] += 1
				letter_count += 1
		
		# On doit maintenant connaître le pourcentage
		# d'utilisation de chacun des caractères
		for c in self.freq:
			self.freq[c] = round(self.freq[c]/letter_count, 4)
		
	def print_freq(self):
		"""
		print_freq(self)
		Méthode qui imprime le résultat
		sur 3 colonnes
		
		Paramètres:
		self : notre objet d'Attaque
		
		Return
		Aucun
		"""
		
		# On imprime le résultat sur 3 colonnes
		new_line_count = 0
		for c in self.freq:
			print(c, ":", self.freq[c], " ", end='')
			if new_line_count % 3 == 2:
				print()
			new_line_count += 1

# Créer un objet d'attaque
pirate = Attaque()
# Calcul la fréquence de caractères
pirate.calculate_freq(secret)
pirate.print_freq()
```  

Exécutez votre code.

Avez-vous le même résultat ?

<details>
	<summary markdown="spam">Réponse :</summary>
	Oui.

```python3
$ python3 AnalyseDeFrequence2.py
A : 0.0077  B : 0.1053  C : 0.0077  
D : 0.0356  E : 0.0077  F : 0.0015  
G : 0.0015  H : 0.0356  I : 0.0635  
J : 0.0743  K : 0.0759  L : 0.0124  
M : 0.096  N : 0.0263  O : 0.0108  
P : 0.0464  Q : 0.0108  R : 0.13  
S : 0.0263  T : 0.0201  U : 0.0372  
V : 0.0341  W : 0.0728  X : 0.031  
Y : 0.0294  Z : 0.0  %   

``` 

</details>  

Sinon, dépanner!

3. Nous voulons trouver quels caractères dans le texte chiffré correspondent le mieux à notre liste de caractère de l’annexe 1.  

On commence par ajouter un dictionnaire pour garder la correspondance que nous allons trouver. Ajoutez un attribut « mappings » et un attribut de pourcentage de correspondance à l’objet Attaque.  

```python
class Attaque:
	"""
	Classe Attaque
	Classe pour analyse cryptographique (attaque) d'un
	texte chiffré par chiffrement de substitution
	"""
	
	def __init__(self):
		# On doit initialiser notre alphabet et
		# la fréquence
		self.alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

		# On utilise un dictionnaire pour enregistrer
		# la fréquence de nos lettres
		self.freq = {}
		
		# On utilise un dictionnaire pour la
		# correspondance de nos caractères
		self.mappings = {}
		
		# Notre référence de correspondance des
		# caractères
		self.freq_eng = {'A':0.08167, 'B':0.01492, 'C':0.02782, 'D':0.04253, 'E':0.12702, 'F':0.02228, 'G':0.02015, 'H':0.06094, 'I':0.06966, 'J':0.00153, 'K':0.00772, 'L':0.04025, 'M':0.02406, 'N':0.06749, 'O':0.07507, 'P':0.01929, 'Q':0.00095, 'R':0.05987, 'S':0.06327, 'T':0.09056, 'U':0.02758, 'V':0.00978, 'W':0.02360, 'X':0.00150, 'Y':0.01974, 'Z':0.00074}
```  

Ajoutez une méthode à l’objet Attaque pour trouver la probabilité de correspondance d’un caractère de notre alphabet dans le texte chiffré. On veut pour chacun de nos caractères, la probabilité qu’il corresponde à un caractère du texte chiffré.  

```python
	def calculate_matches(self):
		"""
		calculate_matches(self)
		Calcul la correspondance de chacun de nos
		caractères de notre alphabet dans le texte
		chiffré. Le pourcentage le plus petit
		indique la plus haute probabilité.
		
		Paramètres:
		self : notre objet d'Attaque
		
		Return
		Aucun
		"""
		
		for secret_char in self.alphabet:
			# On veut trouver les probabilités de
			# la correspondance de tous
			# les caractères dans notre alphabet
			# dans le texte chiffré.

			# On met la correspondance dans un dictionnaire
			map = {}
			
			for plain_char in self.alphabet:
				# On veut trouver la différence de probabilité
				# qu'un caractère de notre alphabet se trouve dans le
				# texte secret. Si la différence est petite, ça
				# peut être le caractère
				map[plain_char] = round(abs(self.freq[secret_char] - self.freq_eng[plain_char]), 4)
			
			# On veut trier la liste par fréquence d'utilisation
			self.mappings[secret_char] = sorted(map.items(), key=operator.itemgetter(1))
```  

Ajouter au début de votre code l’importation pour pouvoir utiliser <code>operator</code>. Il va nous permettre de faire le tri sur le 2e item de notre dictionnaire <code>map</code>.

```python
import operator
```

Ajouter à la fin de votre fichier, le code pour exécuter la méthode.

```python
# Calcul les meilleures correspondances
pirate.calculate_matches()

# Un saut de ligne avant d'imprimer le reste
print()

# Imprime pour chacun des caractères
# la probabilité de correspondance.
for c in pirate.mappings:
    print(c, pirate.mappings[c])
```  

On peut voir les probabilités de correspondance pour chaque caractère, une probabilité faible indique une très bonne correspondance.

Quel est le problème avec nos résultats ?

<details>
	<summary markdown="span">Réponse :</summary>
	Plusieurs caractères ont le même caractère comme probabilité faible.
	
```python
$ python3 AnalyseDeFrequence3.py
A : 0.0077  B : 0.1053  C : 0.0077  
D : 0.0356  E : 0.0077  F : 0.0015  
G : 0.0015  H : 0.0356  I : 0.0635  
J : 0.0743  K : 0.0759  L : 0.0124  
M : 0.096  N : 0.0263  O : 0.0108  
P : 0.0464  Q : 0.0108  R : 0.13  
S : 0.0263  T : 0.0201  U : 0.0372  
V : 0.0341  W : 0.0728  X : 0.031  
Y : 0.0294  Z : 0.0  
A [('K', 0.0), ('V', 0.0021), ('J', 0.0062), ('X', 0.0062), ('Q', 0.0067), ('Z', 0.007), ('B', 0.0072), ('P', 0.0116), ('Y', 0.012), ('G', 0.0125), ('F', 0.0146), ('W', 0.0159), ('M', 0.0164), ('U', 0.0199), ('C', 0.0201), ('L', 0.0326), ('D', 0.0348), ('R', 0.0522), ('H', 0.0532), ('S', 0.0556), ('N', 0.0598), ('I', 0.062), ('O', 0.0674), ('A', 0.074), ('T', 0.0829), ('E', 0.1193)]
B [('T', 0.0147), ('E', 0.0217), ('A', 0.0236), ('O', 0.0302), ('I', 0.0356), ('N', 0.0378), ('S', 0.042), ('H', 0.0444), ('R', 0.0454), ('D', 0.0628), ('L', 0.065), ('C', 0.0775), ('U', 0.0777), ('M', 0.0812), ('W', 0.0817), ('F', 0.083), ('G', 0.0852), ('Y', 0.0856), ('P', 0.086), ('B', 0.0904), ('V', 0.0955), ('K', 0.0976), ('J', 0.1038), ('X', 0.1038), ('Q', 0.1043), ('Z', 0.1046)]
C [('K', 0.0), ('V', 0.0021), ('J', 0.0062), ('X', 0.0062), ('Q', 0.0067), ('Z', 0.007), ('B', 0.0072), ('P', 0.0116), ('Y', 0.012), ('G', 0.0125), ('F', 0.0146), ('W', 0.0159), ('M', 0.0164), ('U', 0.0199), ('C', 0.0201), ('L', 0.0326), ('D', 0.0348), ('R', 0.0522), ('H', 0.0532), ('S', 0.0556), ('N', 0.0598), ('I', 0.062), ('O', 0.0674), ('A', 0.074), ('T', 0.0829), ('E', 0.1193)]
D [('L', 0.0047), ('D', 0.0069), ('C', 0.0078), ('U', 0.008), ('M', 0.0115), ('W', 0.012), ('F', 0.0133), ('G', 0.0154), ('Y', 0.0159), ('P', 0.0163), ('B', 0.0207), ('R', 0.0243), ('H', 0.0253), ('V', 0.0258), ('S', 0.0277), ('K', 0.0279), ('N', 0.0319), ('I', 0.0341), ('J', 0.0341), ('X', 0.0341), ('Q', 0.0347), ('Z', 0.0349), ('O', 0.0395), ('A', 0.0461), ('T', 0.055), ('E', 0.0914)]
E [('K', 0.0), ('V', 0.0021), ('J', 0.0062), ('X', 0.0062), ('Q', 0.0067), ('Z', 0.007), ('B', 0.0072), ('P', 0.0116), ('Y', 0.012), ('G', 0.0125), ('F', 0.0146), ('W', 0.0159), ('M', 0.0164), ('U', 0.0199), ('C', 0.0201), ('L', 0.0326), ('D', 0.0348), ('R', 0.0522), ('H', 0.0532), ('S', 0.0556), ('N', 0.0598), ('I', 0.062), ('O', 0.0674), ('A', 0.074), ('T', 0.0829), ('E', 0.1193)]
F [('J', 0.0), ('X', 0.0), ('Q', 0.0006), ('Z', 0.0008), ('K', 0.0062), ('V', 0.0083), ('B', 0.0134), ('P', 0.0178), ('Y', 0.0182), ('G', 0.0186), ('F', 0.0208), ('W', 0.0221), ('M', 0.0226), ('U', 0.0261), ('C', 0.0263), ('L', 0.0387), ('D', 0.041), ('R', 0.0584), ('H', 0.0594), ('S', 0.0618), ('N', 0.066), ('I', 0.0682), ('O', 0.0736), ('A', 0.0802), ('T', 0.0891), ('E', 0.1255)]
G [('J', 0.0), ('X', 0.0), ('Q', 0.0006), ('Z', 0.0008), ('K', 0.0062), ('V', 0.0083), ('B', 0.0134), ('P', 0.0178), ('Y', 0.0182), ('G', 0.0186), ('F', 0.0208), ('W', 0.0221), ('M', 0.0226), ('U', 0.0261), ('C', 0.0263), ('L', 0.0387), ('D', 0.041), ('R', 0.0584), ('H', 0.0594), ('S', 0.0618), ('N', 0.066), ('I', 0.0682), ('O', 0.0736), ('A', 0.0802), ('T', 0.0891), ('E', 0.1255)]

--Sortie tronquée--

```  

</details>

4. On a trouvé quel caractère de notre alphabet a le plus de probabilité de correspondre à quel caractère du texte chiffré.  

Cependant, certains caractères du texte chiffré correspondent à plusieurs caractères de notre alphabet.  

On va essayer de trouver une clé qui pourrait correspondre en essayant des caractères possibles. La clé ne sera pas nécessairement parfaite, mais on va travailler à l’améliorer. Encore ici, comme programmeur, on ne veut pas essayer de la deviner manuellement.  

Ajoutez deux attributs pour garder la trace de quels caractères ont été utilisé de l’alphabet et du texte chiffré : dans l’algorithme de substitution, un caractère est utilisé une seule fois.  

```python
# Caractères utilisés de l'alphabet
self.plain_chars_left = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

# Caractères utilisés dans le texte chiffré
self.secret_chars_left = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
```  

Ajoutez une méthode qui met la première meilleure correspondance dans une clé de mappage, puis retire cette clé des possibilités.  

```python
    def guess_key(self):
        """
        guess_key(self)
        Trouve la clé qui correspond le mieux au texte chiffré.
        
        Paramètres:
        self    : notre objet d'Attaque
        
        Return
        key (dict) : clés corepondantes (mappage)
        """
        # On veut trouver pour chacun des caractères chiffrés
        # lequel a le plus de chance de correspondre à une
        # entrée de notre alphabet.
        # Va contenir la clé de mappage
        key = {}
        for secret_char in self.secret_chars_left:
            # On veut passer toutes les correspondances
            # et la première disponible on veut la 
            # faire correspondre.
            # On a deux entrées dans mappings :
            # le caractère que l'on recherche,
            # la différence de probabilité.
            # On ne sert pas de la différence.
            for plain_char, diff in self.mappings[secret_char]:
                # Si ce caractère est toujours dans la liste
                # de caractères, on l'utilise et on l'enlève.
                if plain_char in self.plain_chars_left:
                    key[secret_char] = plain_char
                    self.plain_chars_left = self.plain_chars_left.replace(plain_char, '')
                    break
        return key

```  

Modifiez le code de vérification pour générer la clé et l’imprimer.

Testez votre code. Déboguez s’il y a des erreurs.

5. Maintenant, on va tester notre clé sur notre texte chiffrer.

Ajoutez une fonction de déchiffrement qui est en réalité la même chose que celle de chiffrement utilisé dans la partie 1, mais nous y passons la clé et le message chiffré.

La définition de votre fonction sera (attention, la fonction est en dehors de la classe) :

```python
	def decrypt(key, secret) :
	
	### Code à ajouter par vous.
```  

Ajoutez le code pour faire une première tentative de casser le message chiffré et exécutez votre code.

```python
# Déchiffre le message secret
message = decrypt(key, secret)
print(message)
```  

Avez-vous quelque chose de lisible ?

<details>
	<summary markdown="span">Réponse :</summary>
	Pas vraiment!
</details>  

6. On voit que notre système peut deviner des clés, mais malheureusement, il ne trouve pas la bonne clé.  

Dans la partie où l’on analysait le pourcentage d’utilisation des caractères dans le message chiffré, nous avons pu deviner quelques caractères. On pourrait alors passer les clés que l’on soupçonne à notre fonction.  

On va débuter par mettre la clé dans la classe pour pouvoir lui passer ce qu’on connait déjà. Ajoutez l’attribut <code>self.key = {}</code> à votre classe et retirer la définition de key de la méthode <code>guess_key(self)</code>. N’oublie pas de changer les descriptifs de clé dans la méthode pour <code>self.key</code> et d’enlever le <code>return</code> à la fin de la méthode.  

```python
### Code à modifier par vous.
```  

Ajoutez une méthode pour retourner la clé.

```python
    def get_key(self):
        """
        get_key(self)
        Retourne la clé qui correspond le mieux au texte chiffré.
        
        Paramètres:
        self    : notre objet d'Attaque
        
        Return
        key (dict) : clés corepondantes (mappage)
        """
        return self.key

```  

Changez votre appel de méthode :

```python
# Essai de trouver une clé
pirate.guess_key()
key = pirate.get_key()
```  

Testez votre code pour s’assurer qu’il retourne toujours la même chose.  

Maintenant, on veut ajouter des caractères que nous croyons bons à la clé de mappage.  

```python
    def set_key_mapping(self, secret_char, plain_char):
        """
        set_key_mapping(self, secret_char, plain_char)
        Ajoute des caratères connus à la clé.
        
        Paramètres:
        self    : notre objet d'Attaque
        secret_char string : le caractère secret
        plain_char string : le caractère de l'alaphabet
        
        Return
        key (dict) : clés corepondantes (mappage)
        """
        # Vérifions si le caractère existe dans nos caractères
        # Permet également de vérifier si on essaie d'ajouter
        # une correspondance existante.
        if secret_char not in self.secret_chars_left or plain_char not in self.plain_chars_left:
            print("Erreur de mappage de clés : ", secret_char, plain_char)
            # Sortie avec erreur -1
            sys.exit(-1)
        # Ajoute notre caractère et retire les caractères
        # des listes de caractères
        self.key[secret_char] = plain_char
        self.plain_chars_left = self.plain_chars_left.replace(plain_char, '')
        self.secret_chars_left = self.secret_chars_left.replace(secret_char, '')

```  

Pour utiliser <code>sys.exit()</code>, vous devez importer <code>sys</code>. Ajoutez l’importation au début de votre code :

```python
import sys
```  

Finalement, on modifie notre code pour tenir compte d’un caractère que nous croyons connaître : R devrait être E.

```python
# Créer un objet d'attaque
pirate = Attaque()

# Calcul la fréquence de caractères
pirate.calculate_freq(secret)
pirate.print_freq()
pirate.calculate_matches()

# Ajoute un caractère connu
pirate.set_key_mapping('R', 'E')

# Essai de trouver une clé
pirate.guess_key()
key = pirate.get_key()

# Un saut de ligne avant d'imprimer le reste
print()

# Imprime la clé
print(key)

# Déchiffre le message secret
message = decrypt(key, secret)
print(message)
```  

Exécutez votre code.

Avons-nous un meilleur succès ?

<details>
	<summary markdown="span">Réponse :</summary>
	Non, pas vraiment. Mais, c’est difficile à dire.
</details>  

7. Dans la section précédente, c’est difficile de dire si nous avançons, car on ne voit pas le texte chiffré avec notre déchiffrement (texte en clair). Nous allons imprimer les deux textes superposés.  

Changez la commande d’impression <code>print(message)</code> à la fin pour le code suivant :  

```python
# Imprime le message déchiffré P (plain texte)
# superposé au message chiffré C (chiffré)
message_lines = message.splitlines()
secret_lines = secret.splitlines()
for i in range(len(message_lines)):
	print('P:', message_lines[i])
	print('C:', secret_lines[i])
```  

Maintenant, nous allons essayer d’ajouter des caractères que l’on peut deviner. On peut encore utiliser les statistiques. Le deuxième plus utiliser est B.

Essayer de garder l’ordre alphabétique, c’est plus facile pour ne pas ajouter deux fois le même caractère (oui, nous avons un message d’erreur pour ce cas).

```python
# Ajoute un caractère connu
pirate.set_key_mapping('B', 'T')
pirate.set_key_mapping('R', 'E')
```  

Essayez votre code.  

Ce n’est pas encore concluant.  

On peut continuer à regarder les statistiques et voir que la prochaine lettre pourrait être M avec A.   

Exécutez votre code en ajoutant le mappage M avec A.  

Si vous regardez la première et la deuxième ligne de texte Plain, on devine `BECAUSE` et `FOCUS`. Donc, on peut essayer le mappage de V avec C.

```bash
P: BEWAUSE TRE CMAWTNWE OF TRE BASNW ZOJEZEITS OF YATA NS
C: LRVMNIR BPR SUMVBWVR JX BPR LMIWV YJERYRKBI JX QMBM WI
P: TRE FOWUS AIL ZASTEMG OF SEDF NS TRE ESSEIWE OF
C: BPR XJVNI MKD YMIBRUT JX IRHX WI BPR RIIRKVR JX
```  

Ajoutez le mappage et essayez votre code.  

Commencez-vous à pouvoir lire le texte ?

<details>
	<summary markdown="span">Réponse :</summary>
	Oui!
</details>  

Il ne vous reste qu’à continuer à deviner des mappages de caractères jusqu’au moment où le texte devient assez clair pour vous.  

## Compétences développées

| **FW19** – Distinguer les éléments de la cryptographie et de la cryptanalyse. | **FW19 # 1** – Reconnaître les étapes historiques de la cryptographie.<br>  **FW19 # 2** – Comparer les différentes méthodes actuelles de cryptographie.<br>  **FW19 # 3** – Expliquer les éléments de la cryptanalyse. |
| :--- | :--- |

**Note** : les compétences sont développées en partie.

## Références

<https://docs.python.org/3/library/random.html?highlight=random#module-random>  
<https://mathcenter.oxford.emory.edu/site/math125/englishLetterFreqs/>  
<https://www.geeksforgeeks.org/python-operators/>  
<https://docs.python.org/3/tutorial/classes.html>  
<https://www.udemy.com/course/learn-modern-security-and-cryptography-by-coding-in-python/learn/lecture/21402408#search>  

## Annexe 1

**Fréquences relatives des lettres**

| **Alphabétique** | | **Par fréquence d'utilisation** | |
| :--- | :--- | :--- | :--- |
| **Lettre** | **Fréquence** | **Lettre** | **Fréquence** |
| **A** | 0.08167 | **E** | 0.12702 |
| **B** | 0.01492 | **T** | 0.09056 |
| **C** | 0.02782 | **A** | 0.08167 |
| **D** | 0.04253 | **O** | 0.07507 |
| **E** | 0.12702 | **I** | 0.06966 |
| **F** | 0.02228 | **N** | 0.06749 |
| **G** | 0.02015 | **S** | 0.06327 |
| **H** | 0.06094 | **H** | 0.06094 |
| **I** | 0.06966 | **R** | 0.05987 |
| **J** | 0.00153 | **D** | 0.04253 |
| **K** | 0.00772 | **L** | 0.04025 |
| **L** | 0.04025 | **C** | 0.02782 |
| **M** | 0.02406 | **U** | 0.02758 |
| **N** | 0.06749 | **M** | 0.02406 |
| **O** | 0.07507 | **W** | 0.02360 |
| **P** | 0.01929 | **F** | 0.02228 |
| **Q** | 0.00095 | **G** | 0.02015 |
| **R** | 0.05987 | **Y** | 0.01974 |
| **S** | 0.06327 | **P** | 0.01929 |
| **T** | 0.09056 | **B** | 0.01492 |
| **U** | 0.02758 | **V** | 0.00978 |
| **V** | 0.00978 | **K** | 0.00772 |
| **W** | 0.02360 | **J** | 0.00153 |
| **X** | 0.00150 | **X** | 0.00150 |
| **Y** | 0.01974 | **Q** | 0.00095 |
| **Z** | 0.00074 | **Z** | 0.00074 |

Réf. : <https://mathcenter.oxford.emory.edu/site/math125/englishLetterFreqs/>