# Life in plastic, it's fantastic !

## <ins>Prérequis</ins>
- Avoir un groupe
- Avoir un compte [Github](https://github.com)
- [Git / Git Bash](https://gitforwindows.org/)
- [Autodesk Fusion](https://www.autodesk.fr/products/fusion-360/overview?term=1-YEAR&tab=subscription)
---

## <ins>Objectif</ins>
On touche au but ! Vous avez désormais votre circuit sur un circuit imprimé "au propre", et qui est capable de remonter des données.

On cherche pour cette dernière étape à réaliser un boîtier pour accueillir le PCB en impression 3D. Vous aurez un rappel rapide des règles de DFM (Design For Manufacturing) spécifiques à l'impression 3D, et vous serez guidé.e.s pour la réalisation.

## <ins>Préparation</ins>
1. `Forkez` le projet du TP sur votre compte github avec le nom **"PDP_ETUDIANTS_3D_TD<`NUMERO_DE_TD`>\_GP<`NUMERO_DE_GROUPE`>"**. Par exemple, le groupe 7 du TD 2 devra créer le repo **"PDP_ETUDIANTS_3D_TD02_GP07"**
2. <ins>`ATTENTION : un projet mal nommé sera sanctionné par un 0 sur le suivi du TP si l'outil de correction ne peut pas le trouver.`</ins>
3. Clonez **votre fork** sur votre machine avec `git clone <url>`

## <ins>Evaluation</ins>
L'évaluation de votre travail sera réalisée sur les critères suivants :
- Règles de DFM pour impression FDM respectées
- Boîtier rendu fonctionnel

## <ins>Quelques ressources</ins>
- [Protolabs: How to design parts for FDM 3D-printing](https://www.hubs.com/knowledge-base/how-design-parts-fdm-3d-printing/)
- [Wikifactory: Ultimate Guide: How to design for 3D Printing](https://wikifactory.com/+wikifactory/stories/ultimate-guide-how-to-design-for-3d-printing)
---

## <ins>Etape 1 - Lezgoo</ins>
Une seule étape dans ce TP 😎

## Mise en place
1. Créer une `branch` etape_1 à partir de master avec `git checkout -b etape_1 master`. Cette commande crée la nouvelle branche et vous met dessus directement. Si vous la créez avec `git branch`, pensez à switcher dessus avec `git checkout etape_1`. La récupération des repos et des branches est automatisée. **Si la branche n'existe pas sur votre repo github ou qu'elle est mal nommée, elle ne sera pas corrigée.**
2. Il faudra déposer le fichier de votre projet sur votre git pour le rendre.

## Rappels de DFM
L'impression 3D FDM (Fused Deposition Modeling) utilise généralement des filaments de thermoplastique chauffés et extrudés par une buse d'impression pour créer des pièces de façon additive. Cette technique de fabrication connaît des contraintes mécaniques qu'il faut prendre en compte lorsqu'on conçoit des pièces.

### Extrusion et tolérances
Le plastique est extrudé à travers une buse d'un diamètre fixe et connu à l'avance. Ici, on conçoit nos pièces pour une buse de **0.4mm**. Le détail le plus fin que l'on pourra reproduire est donc de cet ordre de grandeur, et ne sera jamais inférieur ! Comme l'imprimante compresse le filament à sa sortie, il devient ovale et la **largeur d'extrusion** réelle est légèrement supérieure au diamètre de la buse. Pour compenser ce phénomène, on utilise des valeurs de **tolérance**. La tolérance est à ajuster en fonction de l'imprimante utilisée. La Prusa MK3S qui sera utilisée pour imprimer vos pièces fonctionne bien avec une tolérance de **+0.1mm** pour un emboitement "qui passe juste". Concrètement, ça signifie que si l'on veut s'assurer que le filement ne gênera pas à un endroit, on doit **retirer 0.1mm par côté (si faces opposées, pour un cylindre, on ajoute la tolérance au rayon, donc le double pour le diamètre)** au design, ce qui correspond à la largeur supplémentaire du filament écrasé pendant son dépôt. Par exemple, pour laisser passer une vis M3 sans frottement, on pourra utiliser un diamètre de 3.2mm (3mm + 2x0.1mm)

<p align="center">
  <img src="https://images.ctfassets.net/q2hzfkp3j57e/3mllYjoh6A2hPcVIxhrJ4z/ced210cde1bb2b2ea232d09f87c6fe04/Illustration_Knowledge_Base_How_to_design_parts__FDM___Vertical_hole_axis.webp?fm=jpg&w=1200&h=1200&q=82" />
</p>

*Source: *[Protolabs](https://www.hubs.com/knowledge-base/how-design-parts-fdm-3d-printing/)*

<p align="center">
  <img src="https://wikifactory.com/files/RmlsZTo0MzE2MzY=" />
</p>

*Source: [Wikifactory](https://wikifactory.com/+wikifactory/stories/ultimate-guide-how-to-design-for-3d-printing)*


### Hauteur de couche
Comme visible sur l'illustration ci-dessus, l'imprimante utilise une certaine distance entre chaque couche déposée que l'on appelle la **hauteur de couche**. La hauteur de couche est la plus petite unité d'épaisseur disponible à la verticale. Plus elle est élevée, plus le temps d'impression est réduit mais plus on perd en détail (moins de couches). On la prend en compte dans le design pour éviter les mauvaises surpises (eg un "plancher" prévu plus fin que la hauteur de couche aura pour épaisseur... la hauteur de couche). Ici, on utilisera une hauteur de couche de **0.3mm**. Idéalement, tous les élements de géométrie horizontaux sont alignés à des multiples de la hauteur de couche

### Murs fins
En reprenant le paragraphe sur la largeur d'extrusion, on voit bien qu'elle est fixe. Les murs fins (murs d'une épaisseur proche de la largeur d'extrusion) demandent une attention particulière. En effet, un mur prévu trop fin pourra soit disparaître dans le **slicer**, soit se retrouver très fragile. Le filament déposé est circulaire et un mur composé d'un seul **périmètre** serait très fragile. Il faut faire attention à ce que **l'épaisseur des murs fins soit égale à au moins 2-3x la largeur d'extrusion**, et toujours multiple de celle-ci pour éviter les mauvaises surprises.

<p align="center">
  <img src="https://wikifactory.com/files/RmlsZTo0MzE2MzA=" />
</p>

*Source: [Wikifactory](https://wikifactory.com/+wikifactory/stories/ultimate-guide-how-to-design-for-3d-printing)*

### Supports, ponts et angle d'impression
En impression DFM, l'imprimante **doit toujours déposer du filament sur quelque chose**, il n'est pas possible d'imprimer "en plein air". Par ailleurs, on retrouve cette problématique dans l'impression résine, mais pas dans l'impression à frittage de poudre comme des couches de matériau sont déposées successivement et servent de support en remplissant l'espace.

Il existe deux exceptions à cette règle :

- **Les angles d'impression** : Les imprimantes FDM peuvent faire dépasser un filament déposé de la couche inférieure pour créer des angles. En général on essaie de ne pas dépasser un angle de **45-60°**. Cette règle s'applique aux trous circulaires horizontaux, on les évite généralement comme l'angle devient très important en haut de ceux-ci, ce qui peut conduire à une mauvaise finition.
<p align="center">
  <img src="https://images.ctfassets.net/q2hzfkp3j57e/5Q00QLIUwUUHaG4cX8Tm3m/60e917448e67bf08eb0de09a3fbba131/Knowledge_Base_-_How_to_design_FDM_-_Overhangs.webp?fm=jpg&w=1200&h=1200&q=82" />
</p>

*Source: [Protolabs](https://www.hubs.com/knowledge-base/how-design-parts-fdm-3d-printing/)*

- **Les ponts** : Il est possible de créer des ponts horizontaux entre deux points et en ligne droite. Le filament se solidifie en l'air et crée un pont. Le succès de cette technique dépend de la longueur du pont (jusqu'à 4-6cm ?) et du matériau (eg, l'ABS marche très bien mais le PETG est beaucoup trop liquide et ne tolère quasiment aucune distance de pont). Attention ! Les ponts ne marchent bien qu'en ligne droite et avec un support à chaque extrémité.

Dans le cas où on ne peut ni utiliser un angle faible ou un pont, le slicer va générer des **supports** : des structures additionnelles qui viennent supporter les parties du modèle qui ne reposent sur rien. On essaie de les éviter dès que possible pour plusieurs raisons :
- Gâchis de matériau brut, impact environnemental pas cool.
- Les supports rajoutent un temps d'impression considérable.
- Les supports peuvent être *très* difficiles à retirer si la pièce est mal conçue.

#### Pro-tip #1 trous circulaires horizontaux
Ajouter un "chapeau pointu" aux trous circulaires horizontaux (à 45°) pour faciliter l'impression en évitant les angles importants en haut du cercle.

<p align="center">
  <img src="https://wikifactory.com/files/RmlsZTo0MzE2Mzc=" />
</p>

*Source: [Wikifactory](https://wikifactory.com/+wikifactory/stories/ultimate-guide-how-to-design-for-3d-printing)*

#### Pro-tip #2 Trous circulaires verticaux non-supportés
Utiliser des ponts "séquentiels" pour imprimer un trou vertical sans aucun support.

<p align="center">
  <img src="https://i.imgur.com/yfA2UYc.png" />
</p>

*Source: [Maker's Muse sur YouTube](https://www.youtube.com/watch?v=KBuWcT8XkhA)*

### DFM et design paramétrique
On vient de le voir, il y a plusieurs règles à respecter pour obtenir de bonnes impressions à partir d'un modèle 3D. **Un logiciel paramétrique comme Autodesk Fusion se prête très bien à l'exercice. On pourra définir des dimensions dans des paramètres (hauteur de couche, largeur d'extrusion, tolérance...) et les utiliser dans le design pour implémenter les contraintes. Idéalement, toutes les dimensions de votre design devraient être des paramètres** et utiliser des paramètres pour les dimensions critiques (murs fins, épaisseur = 3xlargeur d'extrusion par exemple).

### Résumé des paramètres de DFM à utiliser
| Paramètre | Valeur |
|---|---|
| Largeur d'extrusion | 0.4mm (utiliser pour les murs fins et les dimensions non-critiques) |
| Murs fins | Multiples de la largeur d'extrusion (au moins 2-3x) |
| Tolérance | 0.1mm (radial pour les éléments circulaires) |
| Hauteur de couche | 0.3mm |
| Angle d'impression maximum | 60° |
| Supports | Aucun |

## Réalisation
## Mise en place
Créez un dossier dans Fusion pour votre projet. Dans ce dossier, **créez un modèle pour la boîte, un autre pour le couvercle et enfin un dernier pour l'assemblage des deux.**

Téléchargez et importez le [modèle Fusion du PCB](https://drive.google.com/file/d/1Rqq1AttxM8qUIJOBX5kQ8Ay4OQxnz_pK/view?usp=sharing)

## Règles de conception
Après ce rappel sur la DFM, on peut à présent attaquer le design de notre boîte ! **Prenez le temps de lire les contraintes et utilisez les paramètres ci-dessus pour réfléchir**. Je vous invite très fortement à utiliser le tableau ou **une feuille de papier pour poser vos idées, écrire les dimensions que vous connaissez et en déduire celles qui vous manquent**.

**Les lignes qui commencent par "📏" contiennent des dimensions connues** que vous devez utiliser pour trouver celles qui vous manquent. Faites des schémas de profil et de haut de la boite et remplissez avec ce que vous savez !

**Les lignes qui commencent par 🔲 sont des critères à vérifier !**

- Dimensions extérieures **maximum** de la boîte, **couvercle inclus** :
  - 📏 Largeur : 44mm
  - 📏 Profondeur : 79mm
  - 📏 Hauteur : 35mm

- Dimensions du PCB :
  - 📏 Empreinte de 40x75mm
  - 📏 Hauteur totale de ~19mm (PCB compris)
  - 📏 Le PCB a une épaisseur de 1.6mm
  - 📏 Trous de montage de 3.2mm, à 3mm des bords
  - [ ] 📏 <ins>**Laisser 2mm d'air sous le PCB sauf autour des trous de montage pour laisser la place aux points de soudure**</ins>

- Composants d'assemblage :
  - [ ] 📏 <ins>**Deux vis à tête ronde M3x12mm**</ins> (les insérer depuis Mc MasterCarr dans votre assemblage)

<p align="center">
  <img src="https://imgur.com/0MI8bfP.png" />
</p>

  - [ ] 📏 <ins>Deux écrous hexagonaux M3</ins> (les insérer depuis Mc MasterCarr dans votre assemblage)

<p align="center">
  <img src="https://i.imgur.com/Bx44BIo.png" />
</p>

- <ins>**Features et contraintes obligatoires** :</ins>
  - Utiliser les paramètres de DFM ci-dessus.
  - [ ] Deux pièces en 3D (boite et couvercle)
  - [ ] Aucun support nécessaire
  - [ ] Numéro du TD et du groupe embossés quelque part sur le modèle (**hauteur minimum du texte 8mm**)
  - [ ] Un trou en face de la LED pour la voir
  - [ ] Un trou pour le port USB (prévoyez large)
  - [ ] Une série de petits trous rectangulaires ou circulaires (attention au DFM) de 2mm pour laisser l'air rentrer sans que les grains ne puissent rentrer (bon ok, on a des trous plus grands à côté mais 🤫)

## Rendre votre travail

Exportez votre modèle d'assemblage en .f3d, déposez le dans votre dossier Git, ajoutez le et commitez pour le livrer.

---
