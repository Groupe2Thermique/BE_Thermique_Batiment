# BE - Thermique du Bâtiment

#### Membres du groupe : Javier Aguilera - Pierre Boullé - Louis Colin - Aurélien LeBayon

### Introduction
L'objectif de ce bureau d'étude est d'étudier le comportement thermique d'un batiment au travers la mise en place de différents modèles de simulation. On cherche alors à observer l'évolution des températures et la valeur des fluxs thermiques afin d'en deduire l'efficacite de l'isolation thermique. On décide dans un premier temps de mettre en place un modèle statique simple avant de coder une résolution dynamique de notre probleme. L'ensemble de ces codes sont disponibles directement sur Github et peuvent être utilisé en ligne en utilisant le lien Binder suivant :

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Groupe2Thermique/BE_Thermique_Batiment/HEAD)

On va s'attacher dans ce qui va suivre à décrire le bâtiment choisi comme cas d'étude ainsi que ces caracteristiques.On présentera alors quelques resultats, en prenant garde à decrire les parametres qui peuvent influer dessus.

###### Note : on ne décrira pas précisemment les modèles utilisés dans ce document puisque ce travail a été fait dans les notebooks Jupyter.

### Description du bâtiment

![batiment](https://user-images.githubusercontent.com/128041310/225959537-b43232bd-7b2c-4ccb-aa09-0ec758bbc431.png)

L'ensemble de ce rapport se base sur l'étude d'un batiment rectangulaire de longueur totale 6,24 m et de largeur 3 m. Il est composé de deux pièces adjacentes de surface au sol identique, séparées par un mur en béton de 20 cm d'épaisseur. La pièce numeroté 2 est entièrement entourée de murs solides tandis que la pièce numeroté 1 possède une surface vitrée sur une de ses faces. L'épaisseur du verre est de 4 mm pour le cas témoin, les murs sont constitués de 20 cm de béton et 8 cm d'isolant alors que le plafond et le sol sont seulement réalisés en ciment. Les deux pièces sont controllées en température avec la mise en place d'un système HVAC de type proportionnel. 

On résume alors les caractéristiques des matériaux utilisés dans le tableau suivant :

<p align="center">
<img width="500" alt="Screenshot 2023-03-17 at 17 27 42" src="https://user-images.githubusercontent.com/128041310/225963202-7ae68203-a2ec-45cf-89cd-b73a6bed5e1e.png">
</p>

avec :
- ε_wLW = 0.85    l'emissivité des grandes longeurs d'ondes pour les murs 
- ε_gLW = 0.90    l'emissivité des grandes longeurs d'ondes pour le verre 
- α_wSW = 0.25    l'absorbance des petites longeurs d'ondes pour les surfaces blanches et lisses
- α_gSW = 0.38    l'absorbance des petites longeurs d'ondes pour le verre
- τ_gSW = 0.30    la transmittance des petites longeurs d'ondes pour le verre

### Principales hypothèses et résultats
Bien que l'on ne reprendra pas toutes les hypothèses des différents modèles dans cette partie puisque cela a été fait dans les notebooks Jupyter, on rapelle que les principaux résultats ont pu être obtenu en considérant que :
- le bâtiment se situe à Lyon
- l'on étudie notre site en se basant sur l'année de référence 2000 sur la période juillet-aout 
- le transfert de chaleur se fait par conduction, rayonnement, advection et convection
- l'on a chaque instant un équilibre thermodynamique permettant la conservation de l'énergie thermique
- le critère de convergence et de stabilité du schéma d'Euler est respecté avec le choix d'un pas de temps adapté

On peut alors mettre en place des circuits électriques équivalent aux phénomènes thermiques. On obtient après implementation du modèle le résultat suivant pour ce qui est du modèle dynamique :

<p align="center">
<img width="500" alt="Unknown" src="https://user-images.githubusercontent.com/128041310/225969063-85a2d0c6-08ea-431a-af0e-2695b1c9a1fb.png">
</p>

On remarque que les choix réalisés en terme d'isolation ne garantissent pas un confort idéal du bâtiment. En effet on peut observer sur les graphiques que l'on a des températures qui dépassent les 30 degrés Celsius à l'intérieur des pièces 1 et 2 pour plus de 5 jours consécutifs. Afin, de lutter contre cet inconfort on décide de réaliser la même simulation mais en changeant l'isolant afin d'observer son impact sur les résultats. On fait le choix de mettre en place de la laine de verre de caractéristiques suivantes : 
- conductivité : 0,035 W/(m.K)
- densité : 20 kg/m³



![image](https://user-images.githubusercontent.com/128041310/225596597-5e743a3f-2a84-4fb2-8a20-229cdc021aa3.png)
