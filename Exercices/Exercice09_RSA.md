# Exercice 9 – RSA

## Informations

**Évaluation** : formative  
**Type de travail** : individuel  
**Durée** : 2 heures  
**Système d’exploitation** : Windows, Linux ou MAC  
**Environnement** : python  

## Objectifs

Cet exercice a pour objectifs :

* Implémenter l’algorithme RSA.
* Essayer de casser l’algorithme RSA.

Dans cet exercice, nous allons implémenter l’algorithme RSA pour chiffrer et déchiffrer un message. Finalement, nous allons essayer de casser notre implémentation.

### Partie 1 : Algorithme RSA

Nous allons débuter par implémenter l’algorithme RSA.

#### Étape 1 : générateur de clés RSA

Nous allons débuter par se faire un petit générateur de clés RSA.

1. Créer un nouveau document `RSA.py`.

Nous allons avoir besoin de nombres premiers. Importer les librairies math et random, puis copier les fonctions `est_premier(p)` et `trouve_premier(size)` développer avec l’implémentation de Diffie-Hellman.

Pour nous aider, allons consulter la section Key generation de la page RSA de Wikipédia : [https://en.wikipedia.org/wiki/RSA\_(cryptosystem)#Key\_generation](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29#Key_generation). Nous pouvons voir que nous avons 5 étapes à faire.

Étape 1 : Choisir 2 nombres premiers, p et q, distincts. P et q doivent être gardés secrets.

Ajoutez à votre code le code pour qu’Alice se génère 2 nombres premiers : utiliser une variable `size` pour appeler la fonction `trouve_premier(size)` et affecter 300 à cette variable.

```python
# Génération de clé par Alice (secret)
# Étape 1 : générer 2 nombres premiers distincts
###Votre code ici###
print("Nombres premiers p, q", p, q)
```  

Exécutez votre code.

Avez-vous deux nombres premiers distincts ?

<details>  
<summary markdown="span">Réponse :</summary>
Oui.  
</details>  

2. Étape 2 : Calculez n = pq.

Ajoutez le code nécessaire.

```python
# Étape 2 : calculer n = p*q
###Votre code ici###
print("Le modulo n :", n)
```  

Exécutez votre code.

Avez-vous une valeur pour n ?

<details>  
<summary markdown="span">Réponse :</summary>
Oui.  
</details>  

3. Étape 3, pour cette étape, nous devons calculer λ(n). Le calcul est un peu complexe, mais on nous donne quelques informations pour nous aider.

Puisque n = pq, λ(n)=lcm(λ(p),λ(q)), et puisque p et q sont premier, λ(p)=p-1, et λ(q)=q-1. Donc, λ(n)=lcm(p-1,q-1).

Lcm (Least common multiple, le plus petit commun multiple) se calcul par lcm(a,b)=|ab|/gcd⁡(a,b). (gcd est Greatest common divisor ou plus grand diviseur commun).

OK, on implémente ça. Ajoutez le code suivant à votre code.

```python
def lcm(a ,b):
	"""
	lcm(a, b)
	Trouve le plus petit multiple commun entre a et b.
	
	Paramètres:
	a (int)
	b (int)
	
	Return
	(int)
	"""
	
	# On veut un entier, donc on utilise la division d'entier
	return a*b//math.gcd(a, b)

…

# Étape 3 : calculer lambda(n) (lcm(n) = λ(n) = lcm(λ(p), λ(q)),
# λ(p) = p − 1, λ(q) = q − 1,
# lcm(a, b) = |ab|/gcd(a, b))
lambda_n = lcm(p-1, q-1)
print("Lambda_n :", lambda_n)
```  

Exécutez votre code et vérifier que vous avez bien un λ(n).

4. Étape 4 : choisir un entier *e* tel que *1 < e < λ(n)* et *gcd(e, λ(n)) = 1*.

Nous allons ajouter une fonction pour calculer `e`. Ajoutez le code suivant.

```python
def trouve_e(lambda_n):
	"""
	trouve_e(lambda_n)
	Trouver un entier e tel que 1 < e < λ(n) et
	gcd(e, λ(n)) = 1.
	
	Paramètres:
	lambda_n (int) : l'extrémité supérieure.
	
	Return:
	(int)
	(logical) False : si ne trouve pas
	"""
	
	for e in range(2, lambda_n):
		if math.gcd(e, lambda_n) == 1:
			return e
	return False

…

# Étape 4 : choisir un entier e tel que 1 < e < λ(n)
# et gcd(e, λ(n)) = 1.
e = trouve_e(lambda_n)
print("Clé publique (exposant) e :", e)
```  

Exécutez votre code et vérifiez que vous avez bien un *e*.

5. Étape 5 : on trouve *d*. Pour trouver *d*, résoudre pour *d* l'équation *d⋅e ≡ 1 (mod λ(n))*.

Nous allons ajouter une fonction pour calculer d. Ajoutez le code suivant.

```python
def trouve_d(e, lambda_n):
	"""
	trouve_d(e, lambda_n)
	Trouve un entier qui résout l'équation
	d⋅e ≡ 1 (mod λ(n))
	
	Paramètres:
	e (int) : e clé publique
	lambda_n : l'extrémité supérieure
	
	Return:
	(int)
	(logical) False : si ne trouve pas
	"""
	
	# Nous allons par essai.
	# Comme notre échantillon sera petit, ça fonctionne.
	for d in range(2, lambda_n):
		if d*e % lambda_n == 1:
			return d
	return False

…

# Étape 5 : pour trouver d, résoudre pour d l'équation d⋅e ≡ 1 (mod λ(n)).
d = trouve_d(e, lambda_n)
print("Clé secret (exposant) d :", d)
```  

Exécutez votre code et vérifiez que vous avez bien *d*.

6. Maintenant, nous avons les clés secrètes et publiques.

Imprimez vos clés publiques (e, n) et votre clé secrète (d).

```python
###Ajouter votre code###
```

#### Étape 2 : mettre en œuvre l’algorithme RSA

On termine l’implémentation de RSA.

1. Bob veut envoyer un message à Alice. Il doit avoir la clé publique (plutôt les clés publiques) d’Alice et un message à envoyer.

Nous avons vu dans la théorie que le chiffrement se fait par <code>c=m<sup>e</sup> mod n</code> et le déchiffrement par <code>m=c<sup>d</sup>  mod n</code>.

Ajoutez le code suivant.

```python
# Bob veut envoyer un message à Alice.
# Le message est simple
m = 117

# Il chiffre le message
c = m**e % n
print("Le message chiffré de Bob:", c)

# Alice déchiffre le message
m = c**d % n
print("Le message pour Alice :", m)
```  

Exécutez votre code.

Avons-nous réussi à implémenter RSA ?

<details>  
<summary markdown="span">Réponse :</summary>
Oui.  
</details>  

### Partie 2 : Essayer de casser l’algorithme RSA

Nous pouvons maintenant chiffrer et déchiffrer des données. Quand est-il maintenant du secret de nos données ? Eve, peut-elle intercepter nos informations et les déchiffrer ?

Si l’on retourne à la page Wikipédia de RSA et que l’on regarde la section sur « Security and practical considerations », on parle des problèmes de factorisation des entiers : [https://en.wikipedia.org/wiki/RSA\_(cryptosystem)#Integer\_factorization\_and\_RSA\_problem](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29#Integer_factorization_and_RSA_problem). Principalement, on dit que si l’on peut casser la factorisation des entiers, on peut casser RSA.

Un autre problème, la fréquence ou répétitions des caractères. Un problème que nous avons déjà vu.

#### Étape 1 : casser la factorisation des entiers

1. Si l’on se place du côté d’Eve, que voit-elle ?

Eve peut voir la clé publique (*e* et *n*) et le message chiffré.

Ajouter le code suivant.

```python
# Du côté d'Eve
print("Eve peut voir :")
print(" La clé publique (e, n) :", e, n)
print(" Le message chiffré de Bob :", c)
```  

Exécutez votre code.

Eve, voit-elle la clé publique et le message chiffré de Bob ?

<details>  
<summary markdown="span">Réponse :</summary>
Oui.  
</details>  

2. Avec ces informations, peut-elle récupérer le message original ?

Comme on travaille avec de petits problèmes, on peut les factoriser (factoriser une expression revient à la transformer en un produit de deux ou plusieurs facteurs). On va essayer de trouver *p* et *q* (qui sont censés être secrets).

Ajoutez le code suivant.

```python
def facteurs(n):
	"""
	facteurs(n)
	Trouve les facteurs de n.
	
	Paramètres:
	n (int) : le nombre que l'on veut trouver les facteurs.
	
	Return:
	p (int) : premier facteur
	(int) : deuxième facteur
	"""
	
	for p in range(2, n):
		if n % p == 0:
			return p, n//p

…

# Nous allons factoriser n
p, q = facteurs(n)
print("Facteurs d'Eve (p, q) :", p, q)
```  

Avons-nous trouvé *p* et *q* ?

<details>  
<summary markdown="span">Réponse :</summary>
Oui.  
</details>  

3. Maintenant qu’Eve a trouvé *p* et *q*, que peut-elle faire ?

On se rappelle que l’étape 1 était de créer deux nombres premiers, nous venons de trouver les deux nombres premiers.  

L’étape 2 était de calculer *n*. Mais, nous avons *n*, car il est public.  

L’étape 3 était de calculer *λ(n)*. Pour calculer *λ(n)*, on appelle la fonction `lcm(p-1, q-1)`.  

Eve peut calculer *λ(n)*. La méthode n’est pas secrète.

Ajoutez à votre code.

```python
# Eve calcul lambda
lambda_n = lcm(p-1, q-1)
print("Lambda_n de Eve :", lambda_n)
```  

Exécutez votre code. Vous devriez avoir un λ(n).

4. Il reste les étapes 4 et 5.

L’étape 4 est de calculer *e*, mais *e* est public.

Il reste l’étape 5 qui est de trouver *d*. Encore ici, la méthode n’est pas secrète.

Ajoutez à votre code.

```python
# Eve calcul d
d = trouve_d(e, lambda_n)
print("Clé secrète (exposant) d'Eve d :", d)
```  

Exécutez votre code. Vous devriez avoir *d*.

5. Il ne reste qu’à déchiffrer le message.

Ajoutez le code d’Alice pour déchiffrer le message à Eve.

```python
###Ajouter le code d'Alice qui déchiffre le message dans la partie d'Eve.###
```

Avez-vous réussi à déchiffrer le message ?

<details>  
<summary markdown="span">Réponse :</summary>
Oui.  
</details>  

Donc, si l’on peut factoriser, on peut casser RSA.

#### Étape 2 : fréquence

1. On suppose que Bob veut envoyer un vrai message à Alice.

Comme le texte est long, il n’entre pas dans un chiffrement. Nous avons vu dans la théorie que nous devons diviser notre message en plusieurs blocs.

Ajoutez le code suivant.

```python
# Bob envoie un vrai message à Alice
# Mais, Bob n'est pas prudent.
print("+++++++++++++++++")
print("Bob l'imprudent!")
message = "Alice est plus forte que Bob."

# On divise le message en bloc et
# chiffre chacun des blocs.
for m_c in message:
	c = ord(m_c)**e % n
	# On affiche le message envoyé
	print(c, " ", end='')

print("\n+++++++++++++++++")
```  

Exécutez votre code.

Que remarquez-vous (vérifier les caractères e du message) ?

<details>  
<summary markdown="span">Réponse :</summary>
Les caractères e ont tous le même code.  
</details>  

Effectivement, les caractères identiques sont chiffrés avec des codes identiques. Nous pourrions ici aussi utiliser notre analyse de fréquence pour trouver le texte.

Il nous manque un côté aléatoire à notre message. Il nous faut ajouter un tampon (padding) aléatoire à nos caractères. Ce problème est soulevé dans la page Wikipédia de l’algorithme RSA.

La page Wikipédia nous parle du choix de nos nombres premiers (<https://en.wikipedia.org/wiki/RSA_cryptosystem#Importance_of_strong_random_number_generation>) et du tampon (<https://en.wikipedia.org/wiki/RSA_cryptosystem#Padding>).

On vous parle également de la mauvaise génération de clé : <https://en.wikipedia.org/wiki/RSA_cryptosystem#Faulty_key_generation>.

Le côté aléatoire d’un nombre est hyper important en cryptographie et on le souligne également ici : <https://en.wikipedia.org/wiki/RSA_cryptosystem#Importance_of_strong_random_number_generation>.

Je vous recommande de consulter la page sur le défi de factorisation RSA : <https://en.wikipedia.org/wiki/RSA_Factoring_Challenge>. Le défi était de casser sur la difficulté pratique de factoriser les grands nombres entiers et de casser les clés RSA utilisées en cryptographie. Le plus petit nombre à casser était un nombre à 100 chiffres décimaux appelé RSA-100. Le défi s’est terminé en 2007, mais on peut voir que RSA-250 a été cassé en 2020.

La norme, aujourd’hui, est d’utilisé RSA-2048.

Compétences développées

| **FW19** – Distinguer les éléments de la cryptographie et de la cryptanalyse. | **FW19 # 1** – Reconnaître les étapes historiques de la cryptographie.<br>**FW19 # 2** – Comparer les différentes méthodes actuelles de cryptographie.<br>**FW19 # 3** – Expliquer les éléments de la cryptanalyse. |
| :--- | :--- |

**Note** : les compétences sont développées en partie.

Références

<https://en.wikipedia.org/wiki/RSA_(cryptosystem)>  
<https://en.wikipedia.org/wiki/RSA_cryptosystem#Integer_factorization_and_the_RSA_problem>  
<https://en.wikipedia.org/wiki/RSA_cryptosystem#Importance_of_strong_random_number_generation>  
<https://en.wikipedia.org/wiki/RSA_cryptosystem#Padding>  
<https://en.wikipedia.org/wiki/RSA_cryptosystem#Faulty_key_generation>  
<https://en.wikipedia.org/wiki/RSA_cryptosystem#Importance_of_strong_random_number_generation>  
<https://en.wikipedia.org/wiki/RSA_Factoring_Challenge>  
<https://www.geeksforgeeks.org/python-operators/>  
<https://crypto.stackexchange.com/>  