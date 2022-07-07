## Modélisation d’un problème d’affectation
Pour cette modélisation, nous allons modéliser ce problème d’affectation sous forme d'un problème de flot valeurs maximale à coût minimum.

## Transformation des données
Pour ce faire nous devons premièrement transformer le poids des préférences. Pour ce fait nous devons changer les valeurs de pij :

- Préférence élevée : pij = 5
- Préférence moyenne : pij = 10
- Préférence très faible : pij = 15

Ces valeurs sont arbitraires, nous pouvons les changer, mais nous devons garder que plus le nombre est petit plus la préférence est élevée.

Pour le véto nous allons simplement supprimer l'arc, ce qui a pour effet de rendre impossible à l'étudiant de suivre ce cours.

## Modélisation du problème

- On construit un ensemble de sommets E = {E1, ..., Ei} de i étudiants, ce qui correspond à l'ensemble des étudiants.
- On construit un ensemble de sommets C = {C1, ..., Cj} de j cours à choix, ce qui correspond à l'ensemble des cours. Il faut prendre note que dans l'ensemble de ces cours il y a des cours qui sont fondamentaux. 

Un étudiant ne peut suivre que 5 cours dont 2 fondamentaux. Pour modéliser ce sous problème, nous allons rajouter quelques sommets supplémentaires :

- On construit un sommet Cfi et un sommet Csi pour chaque sommet Ei. Cfi est un sommet qui correspond à l'ensemble des cours fondamentaux que l'étudiant Ei veut suivre. Csi est un sommet qui correspond à l'ensemble des cours standards que l'étudiant Ei veut suivre.

- On va ajouter un arc entre les sommets Ei et Cfi, d'une capacité 5.
- On va ajouter un arc entre les sommets Ei et Csi, d'une capacité 3.

Ces arcs vont nous permettre de définir les contraintes de suivi des cours fondamentaux et standards. En baissant la capacité des arcs Ei -> Csi à 3, nous permet de forcer l'étudiant Ei de suivre au minimum 2 cours fondamentaux.

<figure>
  <img src="https://user-images.githubusercontent.com/6887819/177872783-747a7b68-f386-4a8a-8a1e-fd2a9e146a62.png" alt="Figure 1"/>
  <figcaption>Figure 1 : Graphe qui représente le choix des cours standards et fondamentaux.</figcaption>
</figure>

- On va ajouter un arc entre les sommets Cfi et les sommets Cj qui doivent être un cours fondamental et dont l'étudiant Ei veut suivre. La capacité de cet arc est de 1 et le poids est pij. Pour être sûr que tous les élèves ont au moins 5 cours, on relie avec un poids de préférence très faible tous les cours fondamentaux non choisis. Si l’étudiant a mis un veto sur un cours on retire l’arc.

- On va ajouter un arc entre les sommets Csi et les sommets Cj qui doivent être un cours standard et dont l'étudiant Ei veut suivre. La capacité de cet arc est de 1 et le poids est pij Pour être sûr que tous les élèves ont au moins 5 cours, on relie avec un poids de préférence très faible tous les cours standards non choisis. Si l’étudiant a mis un veto sur un cours on retire l’arc.

- On va ajouter un sommet source S et un sommet puits T.

- On va ajouter des arcs entre le sommet S et les sommets Ei, d'une capacité 5. Ce qui permet de forcer l'étudiant Ei de suivre au maximum 5 cours.

Il nous manque encore une étape dans notre modélisation pour borner le nombre d'étudiants qui peuvent suivre un cours.

- On va ajouter un sommet C+j et un sommet C-j pour chaque cours. C+j est un sommet qui va contenir le nombre maximum d'étudiants qui peuvent suivre le cours Cj. C-j est un sommet qui va contenir le nombre minimum d'étudiants qui peuvent suivre le cours Cj.

- On va ajouter un arc entre les sommets Cj et C+j, d'une capacité Bj-bj. (Nombre maximum d'étudiants qui peuvent suivre le cours - nombre minimum d'étudiants qui peuvent suivre le cours) Avec un poids de 1.

- On va ajouter un arc entre les sommets Cj et C-j, d'une capacité bj. 
	Avec un poids de 0.

Il nous manque plus qu'à ajouter les arcs entre les sommets C+j et C-j au puits T.

Le but de notre modélisation est de résoudre le problème de flot maximal à coût minimum. Nous devons trouver une solution où la valeur du flot maximum est égale au nombre d'étudiants multiplié par 5 (le nombre de cours que doit suivre chaque étudiant). ¬

## Exemple de graphe simplifié 
Avec deux étudiants et 6 cours dont 3 fondamentaux. Avec quelques arcs de choix de préférence de cours.

<figure>
  <img src="https://user-images.githubusercontent.com/6887819/177872831-3f062b01-1339-4016-9a1d-3c5eb7eccc63.png" alt="Figure 2"/>
  <figcaption>Figure 2: Exemple du graphe de la modélisation</figcaption>
</figure>

## Autre idée de modélisation en deux phases
La partie de la modélisation qui gère le maximum et le minimum du remplissage des cours et la partie la plus difficile à modéliser. 
Je ne suis pas convaincu de la procédure que j'ai utilisée pour modéliser cette partie. 

Une autre approche serait de résoudre le problème en 2 phases :
 
- La première phase serait de trouver une solution optimale pour le minimum du remplissage des cours. 

- La deuxième phase se ferait quand on a trouvé une solution optimale pour le minimum du remplissage des cours. Et que le flot maximum est égale à la somme du nombre d'étudiants minimum pour chaque cours. Puis de changer la valeur des capacités des arcs à la valeurs maximal de chaque cours. Et finir par trouver une solution dont le flot maximum est égale aux nombres d'étudiants multiplié par le nombre de cours que doivent suivre les étudiants.
