---
title: "TP3 - INF111-80-81 Programmation orientée-objet(hors programme) (E2016)"
subtitle: "TP à réaliser en équipe de deux et à rendre le 15 aout à 23h55"
author: 
- name: Mathieu Nayrolles
  location: Montréal, QC, Canada
---


# Bataille Navale

La bataille navale, appelée aussi touché-coulé, est un jeu de société dans lequel deux joueurs doivent placer des navires sur une grille tenue secrète et tenter de toucher les navires adverses.
Le gagnant est celui qui parvient à torpiller complètement les navires de l’adversaire avant que tous les siens ne le soient.
Le principe du jeu de bataille navale semble trouver son origine dans le jeu français L’Attaque lors de la Première Guerre mondiale, on a aussi trouvé des liens de parenté avec le jeu de E. I. Horseman en 1890 (Baslinda) et on dit que des officiers russes y auraient joué antérieurement à la première guerre. 
La première version commerciale du jeu fut publiée en 1931 par la StarexNovelty Co. sous le nom de Salvo. 
Ce jeu est devenu populaire lors de son apparition en 1943 dans les publications américaines de divertissement de laMilton Bradley Company qui l’exploita sous la forme papier jusqu’en 1967, où elle sortit un jeu de plateau, puis en réalisa une version électronique en 1977.


## Liste des navires

Chaque joueur possède les mêmes navires, dont le nombre et le type dépendent des règles du jeu choisies.

Une disposition peut ainsi comporter :


- 1 porte-avions (5 cases)
- 1 croiseur (4 cases)
- 1 contre-torpilleurs (3 cases)
- 1 sous-marin (3 cases)
- 1 torpilleur (2 cases)


## Grille de jeu

Qu'elle soit en version électronique ou non, la grille de jeu est toujours la même, numérotée de 1 à 10 horizontalement et d’A à J verticalement.

\input{tex/tab1}

Conventionnellement, les joueurs placent des pions blancs sur la grille lorsque les coordonnées n'ont pas touché de bateau adverse, et rouge lorsqu'une touche a été faite.

# Règles

La bataille navale oppose deux joueurs qui s'affrontent.
Chacun a une flotte composée de 5 bateaux, qui sont, en général, les suivants : 1 porte-avion (5 cases), 1 croiseur (4 cases),
1 contre torpilleur (3 cases), 1 sous-marin (3 cases), 1 torpilleur (2 cases).
Au début du jeu, chaque joueur place ses bateaux sur sa grille. Celle-ci est toujours numérotée d’A à J verticalement et de 1 à
10 horizontalement. Un à un, les joueurs vont "tirer" sur une case de l'adversaire. Par exemple B.3 ou encore H.8. Le but est donc de couler les bateaux adverses. En général, les jeux de société prévoient des pions blancs pour les tirs dans l'eau (donc qui ne touchent aucun bateau adverse) et des pions rouges pour les "touché". 
Au fur et à mesure, il faut mettre les pions sur sa propre grille afin de se souvenir de nos tirs passés.

# Énoncé du TP

Votre objectif pour ce travail pratique est de créer un joueur artificiel capable de jouer contre vous à la bataille navale de manière graphique.

## Algorithmes

Le joueur artificiel va avoir deux modes de difficulté : Facile et Normale.

### Difficulté facile

Dans le mode facile, le joueur artificiel placera ses navires au hasard sur la carte et effectuera des tirs sur la grille de son adversaire (vous) au hasard jusqu'à ce que l'un des deux joueurs perde. 
Un joueur perd la partie lorsque tous ses navires sont coulés.

### Difficulté Normal

Dans le mode de difficulté normal, les navires sont placés au hasard et les tirs sont effectués au hasard jusqu'à ce qu'un navire soit touché. Une fois qu'un navire est touché, il est possible de parcourir les cases au NORD, à l'EST, au SUD et à l'OUEST de la case courante afin de tirer sur le reste du navire \footnote{Algorithme Nick Berry.}. 

Une implémentation simple de cette stratégie consiste à créer une pile dynamique des cibles potentielles. 
Initialement, l'algorithme commence en mode Chasse et tir au hasard. Lorsqu'un navire est touché, l'algorithme passe en mode Cible. 
Après un touché, les quatre cases (ou moins si l'on se trouve sur un bord / angle) qui entourent la case courante sont empilées dans la pile des cibles potentielles.

Les cases ne sont ajoutées à la pile des cibles potentielles que si elles n’ont pas encore été ciblées par un tir. 

Lorsque le mode Cible est activé, on dépile une cible potentielle de la pile et on tire dessus. 
Si le tir touche un navire, alors les cases adjacentes sont ajoutées à la pile. 
L'algorithme reste en mode Cible tant qu'il reste des cibles potentielles dans la pile et qu'il reste des navires adverses. 
Si la pile est vide et que la partie n'est pas finie, alors l'algorithme repasse en mode Chasse et tir au hasard. 

Les figures qui suivent illustrent une partie.


\begin{figure}[H]

\includegraphics[width=\textwidth]{media/1.png}


\caption{Initialement, l'algorithme commence en mode Chasse et tir au hasard. Au tour 3, il touche un navire et le mode Cible est activé. 
Les quatre cases (NORD, SUD, EST, OUEST) sont ajoutées à la pile des cibles potentielles. 
Au tour 4 l'algorithme tir au nord du dernier touché et touche à nouveau un navire. Les cases N, E et O de ce nouveau touché sont ajoutées à la pile. La case SUD n'est pas ajoutée car l'algorithme a déjà tiré dessus.}
\end{figure}

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/2.png}
\caption{Au tour 5, l'algorithme touche encore, on ajoutes donc les cases adjacentes qui n'ont pas encore été visitées. Les tours 6 et 7 ne touchent pas.}
\end{figure}

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/3.png}
\caption{Les tours 9 et 10 montrent comment l'algorithme test et élimine les cases des deux côtés du navire. Au tour 12, l'algorithme découvre un nouveau navire.}
\end{figure}

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/4.png}
\caption{L'exploration de ce groupe de navires continue.}
\end{figure}


\begin{figure}[H]
\includegraphics[width=\textwidth]{media/5.png}
\caption{Pour être certain, nous devons tirer sur toutes les cases adjacentes à une case qui a donné un touché.}
\end{figure}



\begin{figure}[H]
\includegraphics[width=\textwidth]{media/6.png}
\caption{Au tour 21, nous avons un nouveau touché.}
\end{figure}



\begin{figure}[H]
\includegraphics[width=\textwidth]{media/7.png}
\caption{Au tour 28, la dernière cible potentielle a été dépilée de la pile, l'algorithme repasse donc en mode Chasse.}
\end{figure}


\begin{figure}[H]
\includegraphics[width=\textwidth]{media/8.png}
\caption{Aucun touché ici.}
\end{figure}

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/9.png}
\caption{Au tour 36, nous avons un touché, l'algorithme repasse en mode Cible.}
\end{figure}

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/10.png}
\caption{Au tour 40, le croiseur est coulé.}
\end{figure}


\begin{figure}[H]
\includegraphics[width=\textwidth]{media/11.png}
\caption{Certaines cases ont déjà été visitées ce qui accélère la recherche.}
\end{figure}


\begin{figure}[H]
\includegraphics[width=\textwidth]{media/12.png}
\caption{Le raté du tour 45 fait repasser l'algorithme en mode Chasse.}
\end{figure}

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/13.png}
\caption{Nous avons un nouveau touché au tour 49}
\end{figure}

\begin{figure}[H]
\includegraphics{media/14.png}
\caption{Finalement, les navires sont coulés au tour 53.}
\end{figure}

### Contraintes d'implémentation

Au niveau de l'implémentation des différentes classes:

- Vous devez avoir une interface IJoueur qui contient les services suivants
	- tire(IJoueur cible)
	- recoitTire(Position position)
- Vous devez avoir une classe abstraite AJoueur qui implémente l'interface IJoueur et implémente l'algorithme "normal". En plus, cette classe abstraite contient tout les services nécessaire au positionnement des bateaux (placeNavire(Navire navire)). L'algorithme "normal" est factorisé au niveau de la classe abstraite car le joueur humain va aussi l'utiliser (voir prochaine section).
- La classe AJoueur est étendue par une classe JoueurArtificiel et une classe JoueurHumain. Chacune des classes spécialises AJoueur de manière différente.
- La classe Navire contient toutes les informations nécessaire pour modéliser un navire 
	- Liste dynamique de Position. Une position est une case de la carte (i.e. A-1). Une position, en plus de la position en lettre et en chiffre contient un boolean touché qui indique si cette position a déjà été visée par un tir.
	- boolean estCoulé() qui retourne vrai si toute les positions ont touché == true.
	- Le nom du navire (i.e. porte-avions, ...)

## L'interface à réaliser

La figure ci-dessous présente l’interface graphique à réaliser.



\begin{figure}[H]
\includegraphics[width=\textwidth]{media/base.png}
\caption{Interface de base}
\end{figure}


- Placer *: Place un *
- Annuler : Annule le dernier coup du joueur humain.
- Aide : Suggère une case sur laquelle tirer pour le joueur humain grâce au même algorithme que le joueur artificiel
- Recommencer : Recommence une nouvelle partie.
- 5 Derniers coups : Affiche les cinq derniers coups.
- Zone de texte en bas : Affiche les informations sur la partie. (Choisissez une case pour faire feu, Vous avez gagné, ect...).
- x, o et + :  Les x sont des tires touchés, les o sont des tires à l'eau et les + sont vos bateaux non touchés.

### Comportement de l'interface

Lorsque la partie commence, les deux grilles sont vides et le joueur humain doit placer ses navires. 
Pour placer un navire, le joueur humain clique sur l'un des boutons de placement. 
Lorsque l'un des boutons est choisi, les autres se grisent comme sur la figure \ref{fig:1}. 
Le joueur humain, doit cliquer sur la case de début du navire et ensuite sur la case de fin. 
Bien entendu, le nombre de cases entre le début et la fin du navire doit correspondre au nombre de cases du navire choisi. 
De plus, les navires ne peuvent se chevaucher et ils ne peuvent pas être en diagonale.

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/base1.png}
\caption{Placement d'un bateau.\label{fig:1}}
\end{figure}

Le joueur humain choisit place ensuite son second navire. Le porte-avion étant déjà placé, le bouton est grisé.

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/base2.png}
\caption{Placement d'un second bateau.\label{fig:2}}
\end{figure}

Lorsque les navires sont tous placés, la partie commence et je joueur humain fait feu en cliquant sur une case de la grille de son adversaire. Après le clique, la case se transforme en x ou o si c'est un touché ou un coulé. 
Pareillement, les la liste des cinq derniers coups est mise à jour. 

\begin{figure}[H]
\includegraphics[width=\textwidth]{media/base3.png}
\caption{Action de tir.\label{fig:3}}
\end{figure}

### Implémentation de l'interface

Voici les règles d'implémentation à respecter :

- Les boutons sont créé grâce à une classe personnalisée qui hérite de JButton.
- Les grilles peuvent être, au choix :
 - Des JTable
 - Une suite de JButton
 - une suite de JTextArea
- Les boutons de gauche sont dans un JPannel
- Chaque grille est dans un JPannel
- Chaque "5 derniers coups" est dans un JPannel et implémente le comportement observateur.
- Les coups des joueurs sont stockés dans des files dynamiques ce qui permet d'annuler un coup.
- Le bouton "Aide" qui suggère une case sur laquelle tirer au joueur humain fait appel au même algorithme (ensemble de sous programmes) que le joueur artificiel.
- Les comportements suivant s'affichent dans la zone texte en bas :
 - Statistiques (nombre de parties joués / gagnés. Nombre de coups moyen pour une victoire)
 - Case déjà ciblée
 - Placement correct des navires
 - Fin de partie
- Aucun point n'est attribué sur la "beauté" de l'interface graphique. 
Si votre interface respecte les normes ci-dessus et l'organisation générale de l'interface décrite ici, vous ne serez pas pénalisé.

# Corrections

Les normes de programmation établies pour les TP1 et TP2 sont toujours en vigueur. 
L’exécution compte pour 60% et la qualité du code pour 40%. 
