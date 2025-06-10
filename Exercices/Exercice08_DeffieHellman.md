# Exercice 8 – Deffie-Hellman, la distribution de clés

## Informations

**Évaluation** : formative  
**Type de travail** : individuel  
**Durée** : 2 heures  
**Système d’exploitation** : Windows, Linux ou MAC  
**Environnement** : python  

## Objectifs

Cet exercice a pour objectifs :

* Implémenter un générateur de nombre premier aléatoire.
* Implémenter un générateur de groupe.
* Implémenter l’échange de clés Diffie-Hellman.

Dans cet exercice, nous allons faire une implémentation pour générer des nombres premiers aléatoires. Nous allons également implémenter un générateur de groupe pour pouvoir après implémenter l’échange de clés Diffie-Hellman.

### Partie 1 : Générateur de nombre premier

Pour notre exercice, on pourrait aller sur Internet et trouver des nombres premiers. Mais, à la place, nous allons en générer.

#### Étape 1 : nombre premier

1. Créer un nouveau document `DiffieHellman.py`.

On débute par une fonction qui vérifie si un nombre est premier.

```python
def est_premier(p):
	"""
	est_premier(p)
	Vérifier si p est un nombre premier
	
	Paramètres:
	p (int) : le nombre à vérifier
	
	Return:
	False ou True.
	"""
	# On ne vérifie pas 1 et 2.
	# On va jusqu'au nombre à vérifier
	# en excluant le nombre
	for i in range(2, p):
		# Si la division n'a pas de reste
		# donc il n'est pas premier
		if p % i == 0:
			return False
	return True
```  

Ajoutez un test avec un nombre premier et nombre qui n’est pas premier.

```python
# On vérifie avec un nombre non premier et un premier.
print("46 est non premier : ", est_premier(46))
print("23 est premier : ", est_premier(23))
```  

Exécutez votre code.

Le résultat est-il concluant ?

<details>
<summary markdown="span">Réponse :</summary>
Oui.

```bash
$ python3 DiffieHellman01.py
46 est non premier :  False
23 est premier :  True 

```  
</details>

On peut faire une petite optimisation. Nous ne sommes pas obligés de vérifier toutes les divisions jusqu’au nombre p. Si c’est un nombre composé[[1]](#footnote-1) l’un de ses facteurs sera moins que la racine carrée du nombre, `p`.

Nous avons besoin de `math` pour calculer la racine carrée. Importez `math` et modifié votre fonction pour limiter votre boucle à `math.isqrt(p)+1`.

```python
def est_premier(p):
	"""
	est_premier(p)
	Vérifier si p est un nombre premier
	
	Paramètres:
	p (int) : le nombre à vérifier
	
	Return:
	False ou True
	"""
	# On ne vérifie pas 1 et 2.
	# On va jusqu'à la racine carrée
	# du nombre plus 1: la valeur
	# de droite de range n'est pas
	# incluse
	for i in range(2, math.isqrt(p)+1):
		# Si la division n'a pas de reste
		# donc il n'est pas premier
		if p % i == 0:
			return False
	return True
```  

Exécutez votre code et dépannez s’il y a lieu.

2. Maintenant, nous allons générer des nombres premiers aléatoires.

La manière la plus simple, générer des nombres aléatoires et vérifier s’ils sont premiers.

Importer `random` et ajouter la fonction suivante à votre code :

```python
def trouve_premier(size):
	"""
	trouve_premier(p)
	Trouve un nombre premier de manière aléatoire
	On recherche le nombre du début de size jusqu'au
	double. Nous aurions pu mettre les intervalles
	dans l'appel de fonction.
	
	Paramètres:
	size (int) : le nombre à vérifier
	
	Return:
	(int) : un nombre premier aléatoire
	"""
	while True:
		p = random.randrange(size, 2*size)
		if est_premier(p):
			return p
```  

Modifiez votre code pour appeler `est_premier(p)` avec 1000 et imprimer la réponse.

```python
# On génère un nombre premier aléatoire.
print("Nombre premier : ", trouve_premier(1000))
```  

Exécutez votre code.

Avez-vous un nombre premier ?

<details>
<summary markdown="span">Réponse :</summary>

Oui.  
</details>


Vous pouvez vérifier votre nombre ici : <https://calculis.net/premier#resultat>.

### Partie 2 : Générateurs

Maintenant, nous avons besoin d’un générateur. Où trouver un générateur ?

Allons voir la page Wikipédia de Diffie-Hellman : <https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange>. Sous « Cryptographic explanation » on retrouve : « The simplest and the original implementation of the protocol uses the multiplicative group of integers modulo p, where p is prime, and g is a primitive root modulo p. » ou « L'implémentation la plus simple et la plus originale du protocole utilise le **groupe multiplicatif d'entiers** modulo p, où **p est premier**, et **g est une racine primitive modulo p** ». C’est ce que nous voulons faire.

Par contre, comment fait-on une racine primitive modulo p ? Encore ici, on se réfère à Wikipédia : <https://en.wikipedia.org/wiki/Primitive_root_modulo_n>. Dans la section « Finding primitive roots », on nous indique qu’aucune formule générale simple pour calculer les racines primitives modulo n n'est connue. Comme nous allons utiliser de petits nombres premiers et que nous n’allons pas utiliser notre algorithme pour échanger des messages ultrasecrets, nous allons faire une implémentation naïve.

#### Étape 1 : implémenter un générateur

1. Donc, pour Deffie-Hellman, on a besoin d’un générateur mathématique. Dans les références, vous avez plusieurs descriptions d’un générateur mathématique, mais en voici une plus facile à comprendre :

« Un générateur d'un groupe fini est une valeur 𝑔 telle que tous les éléments du groupe peuvent être représentés par 𝑔<sup>𝑘</sup> pour un entier 𝑘. Une autre manière pour le regarder est que si nous considérons la séquence 𝑔, 𝑔\*𝑔, 𝑔\*𝑔\*𝑔,..., dire que 𝑔 est un générateur signifie que toutes les valeurs du groupe apparaîtront quelque part dans la séquence. »[[2]](#footnote-2)

Nous avons besoin d’une fonction qui nous génère un générateur. Nous allons le faire en deux fonctions : une qui génère un générateur et l’autre qui vérifie si nous avons un générateur.

Voici le code à ajouter :

```python
def is_generator(g, p):
	"""
	is_generator(g, p)
	Vérifie si g est un générateur.
	Essaie toutes les valeurs de 1 à p-1 (on ne teste
	pas p). Tous les éléments du groupe peuvent être
	représentés par g élevé à k pour un entier k.
	Donc, si le g élevé à k divisé par p retourne un reste
	de 1, ce n'est pas un générateur.
	
	Paramètres:
	p (int) : un nombre premier
	
	Return:
	(int) : un nombre générateur
	"""
	# On vérifie si élevé à une puissance on ne revient pas
	# à 1. Si ça devient 1, alors il recommence. Donc,
	# ce n'est pas un générateur.
	# On vérifie de 1 à p-1 (on ne teste pas p)
	for i in range(1, p - 1):
		if(g**i) % p == 1:
			return False
	return True
	
def get_generator(p):
	"""
	get_generator(p)
	Retourne un générateur.
	Essaie tous les nombres de 2 à p et
	appelle la fonction is_generator pour
	vérifier si c'est un generator
	
	Paramètres:
	p (int) : un nombre premier
	
	Return:
	(int) : un nombre générateur
	"""
	# On débute à 2, car 0 ou 1 élève
	# à une puissance ne donne pas grand-chose.
	for g in range(2, p):
		if is_generator(g, p):
			return g
```  

Modifier la fin de votre code pour créer un nombre premier aléatoire et l’assigner à une variable `p`, de générer un générateur `g` à partir de `p` et d’afficher le nombre premier avec son générateur.

Exécutez votre code.

Vous devriez avoir un résultat comme suit (j’ai utilisé 10000 pour générer mon nombre premier) :

```bash
$ python3 DiffieHellman04.py

Nombre premier : 14207 Générateur : 5
```  

### Partie 3 : Deffie-Hellman

Maintenant que nous avons un générateur de nombre premier, ainsi que générateur mathématique, on peut implémenter, de manière rudimentaire Deffie-Hellman.

Alice a besoin d’un nombre aléatoire (a), d’un nombre premier et d’un générateur. Elle doit ensuite créer un nombre à envoyer à Bob (j) avec l’équation g<sup>a</sup> mod p.

Bob a besoin d’un nombre aléatoire (b), d’un nombre premier et d’un générateur. Il doit ensuite créer un nombre à envoyer à Alice (k) avec l’équation g<sup>b</sup> mod p.

Finalement, ils vont créer un nombre commun avec k<sup>a</sup> mod p = j<sup>b</sup> mod p = g<sup>ab</sup> mod p.

#### Étape 1 : on fait un Deffie-Hellman

1. Nous avons déjà tout ce qu’il nous faut dans notre code.

On débute par créer les informations publiques : un nombre premier et un générateur.

```python
# Informations publiques
# On génère un nombre premier aléatoire.
p = trouve_premier(10000)

# On génère un générateur
g = get_generator(p)
print("Nombre premier : ", p, "Générateur : ",g)
```  

On ajoute Alice 1.

```python
# Alice 1
# Elle doit générer un nombre aléatoire.
# Normalement, on utiliserait un très
# grand nombre.
a = random.randrange(0, p)

# Elle calcule le nombre à envoyer à Bob
j = (g**a) % p

# Alice envoie son nombre à Bob
# Elle utilise un canal non sécurisé
print(" Alice j : ", j)
```  

On ajoute Bob 1.

```python
# Bob 1
# Il doit générer un nombre aléatoire.
# Normalement, on utiliserait un très
# grand nombre.
b = random.randrange(0, p)

# Il calcule le nombre à envoyer à Alice
k = (g**b) % p

# Bob envoie son nombre à Alice
# Il utilise un canal non sécurisé
print(" Bob k : ", k)
```  

On veut vérifier si Alice et Bob peuvent générer un nombre identique

```python
# Alice 2
g_ab = (k**a) % p
print("Alice g_ab : ", g_ab)

# Bob 2
g_ab = (j**b) % p
print("Bob g_ab : ", g_ab)
```  

Exécutez votre code.

Est-ce que Alice et Bob ont généré le même nombre ?

<details>
<summary markdown="span">Réponse :</summary>
Oui.  

```bash
$ python3 DiffieHellman05.py
Nombre premier :  19919 Générateur :  7
 Alice j :  10189
 Bob k :  8701
Alice g_ab :  7752
Bob g_ab :  7752
```
</details>


Eve ne peut pas avec le nombre premier, le générateur et les deux nombres publics d’Alice et Bob retrouver la clé commune entre Alice et Bob.

En réalité, avec notre implémentation, si Eve a accès à beaucoup d’échantillons, elle pourrait trouver l’information. Il faudrait utiliser un très grand nombre premier pour la rendre plus sécuritaire.

Maintenant, Alice et Bob ont un nombre aléatoire qu’ils peuvent utiliser pour échanger de l’information avec un chiffrement symétrique.

L’implémentation que nous avons faite suit l’idée originale de l’algorithme. Si vous retournez à la page Wikipédia de Diffie-Hellman, à la section « Generalization to finite cyclic groups », vous pouvez voir que maintenant on utilise un algorithme basé sur les courbes elliptiques (elliptic-curve) pour une meilleure sécurité.

La section « Security »[[3]](#footnote-3) de la page Wikipédia nous indique aussi les faiblesses de l’algorithme et les types d’attaques. Un point important pour sa sécurité est l’utilisation d’un très grand nombre premier. À la fin de cette section, il est indiqué :

« Pour éviter ces vulnérabilités, les auteurs de Logjam recommandent l'utilisation de la cryptographie à courbe elliptique, pour laquelle aucune attaque similaire n'est connue. À défaut, ils recommandent que l'ordre, *p*, du groupe Diffie-Hellman soit d'au moins 2048 bits. Ils estiment que le précalcul requis pour un nombre premier de 2048 bits est 109 fois plus difficile que pour des nombres premiers de 1024 bits. »[[4]](#footnote-4)

Compétences développées

| **FW19** – Distinguer les éléments de la cryptographie et de la cryptanalyse. | **FW19 # 1** – Reconnaître les étapes historiques de la cryptographie.<br>**FW19 # 2** – Comparer les différentes méthodes actuelles de cryptographie.<br>**FW19 # 3** – Expliquer les éléments de la cryptanalyse. |
| :--- | :--- |

**Note** : les compétences sont développées en partie.

Références

<https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange>  
<https://en.wikipedia.org/wiki/Primitive_root_modulo_n>  
<https://crypto.stanford.edu/pbc/notes/numbertheory/gen.html>  
<https://www.geeksforgeeks.org/python-operators/>  
<https://crypto.stackexchange.com/>  
<https://crypto.stackexchange.com/questions/16196/what-is-a-generator>  
<https://crypto.stackexchange.com/questions/828/for-diffie-hellman-must-g-be-a-generator>  

##

1. <https://www.alloprof.qc.ca/fr/eleves/bv/mathematiques/les-nombres-composes-primaire-m1606> [↑](#footnote-ref-1)
2. <https://crypto.stackexchange.com/questions/16196/what-is-a-generator> [↑](#footnote-ref-2)
3. <https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange#Security> [↑](#footnote-ref-3)
4. <https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange#Practical_attacks_on_Internet_traffic> [↑](#footnote-ref-4)