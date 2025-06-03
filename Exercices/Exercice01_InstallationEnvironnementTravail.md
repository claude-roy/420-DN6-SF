# Exercice 1 – Installation de l’environnement de travail

## Informations

**Évaluation** : formative  
**Type de travail** : individuel  
**Durée** : 1 heure  
**Système d’exploitation** : Windows, Linux ou MAC  
**Environnement** : python  

## Objectifs

Cet exercice a pour objectifs :

- De préparer l’environnement de travail.  

Dans cet exercice, vous allez préparer un environnement de travail pour les exercices de la première partie du cours.  

### Partie 1 : Installer et vérifier Python

#### Étape 1 : Installation de Python

1. Téléchargez et installez Python.

Allez sur <https://www.python.org/downloads> et téléchargez la version la plus récente de Python 3.

Lors de l'installation, assurez-vous de cocher la case "Ajouter Python 3.X au chemin".

Une fois l'installation terminée, cliquez sur "Désactiver la limite de longueur de chemin" si cette option est disponible pour votre installation.

#### Étape 2 : Testez votre installation Python

1. Pour le système d'exploitation Windows, ouvrez une invite de commande et entrez la commande python. L'interprète interactif devrait s'ouvrir. Tapez <code>quit()</code> pour quitter l'interpréteur interactif Python.

```bash
C:\> python
Python 3.6.3 (v3.6.3:2c5fed8, Oct 3 2017, 17:26:49) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()
C:\>
``` 

2. Pour le système d'exploitation Mac ou Linux, ouvrez une invite de commande et entrez la commande python3. Tapez <code>quit()</code> pour quitter l'interpréteur interactif Python.

```bash
$ python3
Python 3.5.2 (default, Oct 3 2017, 17:48:00)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()
$
```

### Partie 2 : Installation d’un IDE

Un environnement de développement intégré (IDE - integrated development environment) fait référence à une application logicielle qui offre aux programmeurs informatiques des capacités étendues de développement de logiciels. Les IDE se composent le plus souvent d'un éditeur de code source, d'outils d'automatisation de construction et d'un débogueur.

#### Étape 1 : Vérification d’IDLE

1. IDLE (Integrated Development and Learning Environment) est l’éditeur par défaut qui accompagne Python.

Vérifiez son installation. Pour le système d'exploitation Windows, ouvrez IDLE à partir du menu Windows : Démarrer > Python 3.x > IDLE (Python 3.x xx-bit).

Pour le système d'exploitation Mac ou Linux, ouvrez une invite de commande et entrez la commande idle3.

Le shell IDLE devrait s'ouvrir.

#### Étape 2 : Installation d’un IDE plus avancé

1. Pour bien travailler avec Python, je vous recommande l’installation d’un IDE ayant des fonctionnalités plus avancées. De nombreux IDE s’offrent à vous, en voici une liste :

Visual Studio Code : <https://code.visualstudio.com/>

VSCodium : <https://vscodium.com/>

PyCharm : <https://www.jetbrains.com/pycharm/>

Sublime Text : <https://www.sublimetext.com/>

PyDev : <https://www.pydev.org/>

### Partie 3 : Suivre son code sur github

GitHub est une plateforme extrêmement utile en cybersécurité pour plusieurs raisons, allant de la gestion de projets et du partage de code à la recherche et au développement d’outils spécifiques à la sécurité informatique. Voici quelques cas d’utilisation importants :

1. Partage et collaboration sur des outils de cybersécurité

- Développement d’outils : De nombreux outils de cybersécurité populaires (comme Metasploit, Wireshark, ou nmap) ont leur code source hébergé sur GitHub. Cela permet à la communauté de contribuer à leur développement.

- Open source : Les projets open source permettent aux professionnels de la cybersécurité de collaborer pour améliorer la qualité des outils existants.

2. Accès à une vaste bibliothèque d’outils

- GitHub regorge de projets utiles en cybersécurité, allant des scanners de vulnérabilités aux outils de forensic, en passant par des scripts d’automatisation.

- Les chercheurs en sécurité publient souvent leurs preuves de concept (PoC) pour des vulnérabilités récemment découvertes, ce qui aide à comprendre les menaces et à préparer des contre-mesures.

3. Gestion de projets et de scripts personnalisés

- Les équipes de cybersécurité utilisent GitHub pour gérer leurs propres projets, tels que des scripts d’automatisation ou des outils internes pour surveiller, analyser ou protéger les systèmes.

- L’utilisation de Git permet de suivre les modifications, de collaborer efficacement et de maintenir des versions de code fiables.

4. Partage de rapports et d’analyses

- Les professionnels peuvent partager des scripts ou du code qui illustrent une attaque ou démontrent une faille, ce qui est très utile pour les tests d’intrusion ou l’enseignement.

- Les entreprises publient parfois des rapports techniques sur des vulnérabilités et des analyses sur GitHub.

5. Veille technologique et apprentissage

- Les professionnels en cybersécurité utilisent GitHub pour suivre les dépôts liés aux dernières techniques d’attaque et de défense.

- Il sert également comme plateforme d’apprentissage : l’étude du code source d’outils connus ou la participation à des projets open source permet d’acquérir de nouvelles compétences.

6. Automatisation et intégration CI/CD

- GitHub peut être intégré dans des workflows de CI/CD pour automatiser les tests de sécurité (analyse de code statique, scan de dépendances, etc.).

- Cela garantit que les applications sont plus sécurisées avant d’être déployées.

7. Recherche et détection de menaces

- Les chercheurs en cybersécurité utilisent GitHub pour surveiller des dépôts publics où du code malveillant ou des informations sensibles (comme des clés API) peuvent avoir été accidentellement publiés.

- Des outils comme “truffleHog” ou “GitLeaks” permettent de scanner les dépôts GitHub pour détecter de telles fuites.

8. Formation et exercices

- De nombreux dépôts sur GitHub contiennent des environnements ou des scénarios d’entraînement pour apprendre la cybersécurité, tels que des machines virtuelles vulnérables ou des laboratoires de simulation.

En résumé, GitHub est une plateforme incontournable pour les professionnels de la cybersécurité en raison de sa capacité à centraliser, collaborer, et partager du code, des outils, et des connaissances tout en facilitant la gestion des projets et la veille technologique.

#### Étape 1 : Compte github

1. Si ce n'est pas déjà fait, vous devez vous créez un compte sur la plateforme Github : <https://github.com/>.

On doit ajouter la clé publique SSH de notre poste dans notre compte GitHub : <https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account>.

2. Nous allons maintenant préparer et initialiser le répertoire de notre code pour l'utilisation de git.

Créez un dépôt « Cryptographie » (vous pouvez utiliser un autre nom) sur Github.com, en ne mettant PAS de Readme. Il vaut mieux l’ajouter après, une fois que les fichiers ont été téléchargés, pour éviter tout conflit.

Dans votre poste, créer un dossier (répertoire) de travail « Dev » (vous pouvez utiliser un autre nom) et à l’intérieur de ce dossier créer un nouveau dossier « Cryptographie ».

Utiliser les commandes suivantes pour initialiser votre dossier et le lier à votre dépôt github :

```git
echo "# Cryptographie" >>README.md
git config --global user.email "your@exemple.com" #si pas déjà définit.
git config --global user.name "Votre nom" #si pas déjà définit.
git init
git add \*
git commit -m "Intialisation de mon dépôt"
git branch -M main
```

3. Si ce n'est pas fait, créez le projet sur Github.com avec votre navigateur

```git
git remote add origin git@github.com:[votrecompte git hub]/Cryptographie.git
git push -u origin main
git status
```

4. Rafraîchir la page GitHub de vote navigateur pour voir le projet mis à jour.

## Compétences développées

| **FW19** – Distinguer les éléments de la cryptographie et de la cryptanalyse. | **FW19 # 1** – Reconnaître les étapes historiques de la cryptographie.<br>  **FW19 # 2** – Comparer les différentes méthodes actuelles de cryptographie.<br>  **FW19 # 3** – Expliquer les éléments de la cryptanalyse. |
| :--- | :--- |

**Note** : les compétences sont développées en partie.

## Références

<https://www.python.org/downloads>  
<https://code.visualstudio.com/>  
<https://vscodium.com/>  
<https://www.jetbrains.com/pycharm/>  
<https://www.sublimetext.com/>  
<https://www.pydev.org/>  
<https://www.simplilearn.com/tutorials/python-tutorial/python-ide>  