# BE - Thermique du Bâtiment

#### Membres du groupe : Javier Aguilera - Pierre Boullé - Louis Colin - Aurélien LeBayon

### Introduction
L'objectif de ce bureau d'étude est d'étudier le comportement thermique d'un batiment au travers la mise en place de différents modèles de simulation. On cherche alors à observer l'évolution des températures et la valeur des fluxs thermiques afin d'en deduire l'efficacite de l'isolation thermique. On décide dans un premier temps de mettre en place un modèle statique simple avant de coder une résolution dynamique de notre probleme. L'ensemble de ces codes sont disponibles directement sur Github et peuvent être utilisé en ligne en utilisant le lien Binder suivant :

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Groupe2Thermique/BE_Thermique_Batiment/HEAD)

On va s'attacher dans ce qui va suivre à décrire le bâtiment choisi comme cas d'étude ainsi que ces caracteristiques. On présentera alors quelques resultats, en prenant garde à decrire les parametres qui peuvent influer dessus.

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

On peut alors mettre en place des circuits électriques équivalent aux phénomènes thermiques. Ainsi :
- pour le modèle statique :

<p align="center">
<img width="500" alt="Untitled" src="https://user-images.githubusercontent.com/128041310/226108514-a54e9682-0b49-49d9-ac6e-92215b831990.png">
</p>

###### Note : Figure décrite dans le notebook Jupyter intitulé "Modèle_Statique"

L'implémentation sous Python se fait en trois parties, représentées en gris, jaune et rouge sur le schéma. Cette segmentation permet de définir les matrices d'incidence et de conductance propres à notre bâtiment et nécessaires à la résolution de l'équation présentée dans le Jupyter "Modèle_Statique". 
Avec une température de consigne intérieure à 20°C et une témpérature extérieure de 5°C, notre modèle nous permet de calculer une température d'environ 14°C aux noeuds muraux. Nous sommes également en mesure de calculer les flux de chaleur dans chaque pièce, ce qui permet d'adapter les coefficients K1 et K2 régissant les températures de consigne. 

- pour le modèle dynamique :

<p align="center">
<img width="500" alt="Unknown" src="https://user-images.githubusercontent.com/128041310/226108558-7853629d-3b82-46da-9194-9d5f9ad14679.jpeg">
</p>

###### Note : Figure décrite dans le notebook Jupyter intitulé "Modèle_Dynamique"

On obtient après implementation du modèle le résultat suivant pour ce qui est du modèle dynamique :

<p align="center">
<img width="500" alt="Unknown" src="https://user-images.githubusercontent.com/128041310/225969063-85a2d0c6-08ea-431a-af0e-2695b1c9a1fb.png">
</p>

avec : 
- T_indoor 1 : la temperature interieure de la salle 1
- T_indoor 2 : la temperature interieure de la salle 2
- q_HVAC : le flux de temperature necessaire pour maintenir la temperature de consigne

On remarque que les choix réalisés en terme d'isolation ne garantissent pas un confort idéal du bâtiment. En effet on peut observer sur les graphiques que l'on a des températures qui dépassent les 30 degrés Celsius à l'intérieur des pièces 1 et 2 pour plus de 5 jours consécutifs. Afin, de lutter contre cet inconfort on décide de réaliser la même simulation mais en changeant l'isolant afin d'observer son impact sur les résultats. 

### Comparaison des résultats suivant les isolants
Une première série de test a été menée sur un mois de janvier pour comparer différents isolants (laine de verre et paille). Voici les différents résultats :
- Isolant en laine de verre (conductivité de 0,035 W/(m.K) et densité de 20 kg/m^3):
Pour la première salle, nous trouvons une consommation de chauffage de 1035 kWh et de 283 kWh pour la seconde salle. La température est maintenue au niveau de la consigne tout au long du mois (20°C dans ce cas-là). On remarque que la consommation de chauffage dépend des conditions extérieures : s'il y a plus de soleil, moins de chauffage est consommé, de même si la température extérieure est plus élevée.

<p align="center">
<img width="400" alt="janvier_laine_de_verre_sans_ext" src="https://user-images.githubusercontent.com/78414656/226101540-b33fd2f2-e42e-49c6-9421-e6c7c35bb3d7.png">
<img width="425" alt="janvier_laine_de_verre" src="https://user-images.githubusercontent.com/78414656/226101532-a87959ce-e5ec-43a0-98f4-dc33513520fd.png">
</p>

- Isolant paille (conductivité de 0,045 W/(m.K) et densité de 100 kg/m^3):
Pour la première salle, nous trouvons une consommation de chauffage de 1100 kWh et de 376 kWh pour la seconde salle. L'isolation à la paille pour une épaisseur d'isolant égale est donc moins efficace que la laine de verre. Cela s'explique du fait de la différence de conductivité. En réalité, l'isolation paille peut avoir de meilleure performance que l'isolation à la laine de verre, notamment car les bottes de pailles utilisées ont une grande épaisseur (37cm d'épaisseur pour les bottes de paille en général). La résistance thermique est ainsi augmentée.

<p align="center">
<img width="425" alt="janvier_paille_sans_ext" src="https://user-images.githubusercontent.com/78414656/226101710-04da7618-e96d-4e16-a995-231f3fdc562a.png">
<img width="400" alt="janvier_paille" src="https://user-images.githubusercontent.com/78414656/226101721-e2f9dc0c-8e1f-4dd8-a4ee-3cd426aa5cde.png">
</p>

Dans les deux cas, la première pièce a besoin de plus de chauffage pour atteindre la température de consigne. Cela peut s'expliquer par le fait que la paroi en verre isole moins bien que les autres parois en béton + isolant. Mais la pièce 1 est celle qui va le plus bénéficier de l'ensoleillement.

Nous avons ensuite mené une autre simulation au mois de juillet pour comparer les performances des isolants en période estivale.

- Isolant laine de verre :
Dans la première pièce, nous trouvons une température moyenne de 24,1°C et de 23,7°C dans la seconde. La première pièce est plus chaude que la seconde du fait de la paroi en verre qui va chauffer la pièce grâce au rayonnement solaire.

<p align="center">
<img width="425" alt="juillet_laine_de_verre" src="https://user-images.githubusercontent.com/78414656/226102170-56ee15d9-c7b6-45e2-bc5b-4db44956d4a6.png">
<img width="425" alt="juillet_laine_de_verre_sans_ext" src="https://user-images.githubusercontent.com/78414656/226102171-9752e18e-f3ff-41ce-9fc1-4d7f18fd19bf.png">
</p>

- Isolant paille :
Dans la première pièce, nous trouvons une température moyenne de 24,0°C et de 23,6°C dans la seconde. Les températures sont légèrements plus faibles dans ce cas. Cela peut-être dû à la différence de densité entre les deux matériaux.

<p align="center">
<img width="425" alt="juillet_paille" src="https://user-images.githubusercontent.com/78414656/226102226-a4cf5095-7853-486c-bcf2-69189aab4744.png">
<img width="425" alt="juillet_paille_sans_ext" src="https://user-images.githubusercontent.com/78414656/226102244-d3c1332b-0a14-4b88-a97b-4bf1bc7188fc.png">
</p>

Les deux isolants présentent quand même des résultats très similaires dans ce modèle, notamment pour la simulation au mois de juillet. Pour améliorer le modèle, nous pourrions prendre en compte l'orientation des parois pour modifier les flux solaires entrants puisque ce sont eux qui semblent avoir le plus d'influence sur l'ensemble de nos résultats.

### Effet de la ventilation :
A présent, nous regardons quel est l'impact de la ventilation sur la température des deux pièces. Pour cela, nous faisons varier le paramètre ACH, c'est à dire le taux de renouvellement d'air (ACH=1 correspond au renouvellement d'une fois le volume de la pièce par heure). Voici les résultats obtenus :

<center>
  
| ACH | Température moyenne pièce 1 (°C) | Température moyenne pièce 2 (°C) |
|-----|----------------------------------|----------------------------------|
|  1  |               24.06              |               23.57              |
|  2  |               23.81              |               23.25              |
|  5  |               23.35              |               22.70              |
|  10 |               22.95              |               22.30              |
  
</center>

<p align="center">
<img width="425" alt="ventilation_room_1" src="https://user-images.githubusercontent.com/78414656/226109644-a6caa2a4-fe3f-4d4a-af8a-bbc5d03f089b.png">
<img width="425" alt="jventilation_room_2" src="https://user-images.githubusercontent.com/78414656/226109650-34e58277-1884-4518-97ca-7bb4d9c68b80.png">
</p>


Nous pouvons voir que plus le taux de renouvellement est élevé, plus la température des pièces diminuent. En effet, la température des pièces va tendre vers la température extérieure si le taux de renouvellement est plus élevé. Plus le taux de renouvellement est important, moins les parois ont un effet sur la température intérieure. Par exemple, si toutes les fenêtres d'une maison sont ouvertes, la température intérieure sera égale à la température extérieure.

### Ombrage de la paroi vitrée
Nous allons procéder à quelques simulations pour estimer l'impact de l'ombre sur la température des pièces (nuage qui passe devant le soleil, présence d'arbre). Pour cela, nous allons multiplier le flux $\phi_a$ qui arrive sur la vitre par un coefficient d'ombrage. Voici nos résultats :

| Coefficient d'ombre | Température moyenne pièce 1 (°C) | Température moyenne pièce 2 (°C) |
|---------------------|----------------------------------|----------------------------------|
|          0%         |               23.5               |               23.78              |
|         20%         |               23.22              |               23.58              |
|         50%         |               22.80              |               23.27              |
|         80%         |               22.37              |               22.97              |

<p align="center">
<img width="425" alt="ombres_pièce_1" src="https://user-images.githubusercontent.com/78414656/226111019-d312c34b-4cf1-4c1a-b8c4-b4bd1a26c6d8.png">
</p>

Naturellement, plus il y a d'ombrage sur la vitre, plus la température de la pièce 1 diminue. On remarque sur le graphique que cela est vrai uniquement en journée car aucun flux solaire n'arrive sur la vitre en journée. De plus, comme cela peut-être remarqué dans le tableau, la température de la pièce 2 est très peu impactée car seul le flux arrivant sur la vitre de la pièce 1 est concerné par l'ombrage.
L'ombrage des parois est en effet important dans la conception des logements, à la fois pour les conforts d'été et d'hiver. En effet, on veut minimiser l'apport solaire en été et le maximiser en hiver. La présence d'arbre feuillu devant une paroi vitrée peut remplir ce rôle : les feuilles bloquent le flux solaire en été et l'absence des feuilles en hiver permet en flux solaire de passer. Une casquette de toit peut également remplir ce rôle.

### Conclusion

Cette étude nous a permis de constater qu'il est possible d'obtenir des résultats robustes concernant la thermique des bâtiments en utilisant un modèle théorique relativement élaboré et pouvant être implémenté avec des langages de programmation accessibles. Au vu des multiples paramètres d'entrée et de leur facilité d'accès, nombreuses sont les disccusions pouvant être menées pour améliorer la thermique des bâtiments en utilisant ce modèle. 
Cependant, il est à noter que la structure que nous décrivons dans cette étude est très simpliste. Complexifier le bâtiment apporte des changements lourds, en particulier concernant les matrices d'incidence et de conductance, qu'il n'est pas envisageable d'effectuer manuellement, ce qui constitue une limite de ce modèle.
