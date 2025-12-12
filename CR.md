# TP – Antenne patch microbande pour smartphone

**Sujet :** Conception, modélisation et intégration d’une antenne patch à 2,1 GHz dans un smartphone (CST Studio Suite)

**Logiciel :** CST Studio Suite (version 2018 Classroom License)

**Étudiant :** *LI YANBO*

**Date :** *12/12/2025*

---

## 1. Introduction

L’objectif de ce TP est de concevoir et d’analyser une antenne patch microbande résonant à  **2,1 GHz** , destinée à être intégrée dans un smartphone.

Le travail suit la démarche suivante :

1. **Dimensionnement analytique** de l’antenne patch (largeur, longueur, substrat, ligne d’alimentation).
2. **Modélisation 3D sous CST** (substrat, patch, ligne microstrip, port, conditions aux limites).
3. **Validation de la fréquence de résonance et de l’impédance d’entrée** à l’aide des solveurs fréquentiel et temporel (et du filtre AR).
4. **Visualisation des champs électriques** pour illustrer l’effet de bord des antennes patch.
5. **Amélioration de l’adaptation** par ajout de fentes (paramètres  *yo* ,  *xo* ).
6. **Intégration dans un smartphone simplifié** (boîtier plastique + écran en verre) et étude de l’impact sur les performances.

Le substrat utilisé est un FR4 (

(\varepsilon_r = 4{,}4), (h = 0{,}8,\text{mm})), conformément au poly de TP. Toute la modélisation est réalisée en unités **mm / GHz / ns** dans CST.

---

## 2. Étape 1 – Dimensionnement analytique de l’antenne patch

### 2.1 Données de base

* **Fréquence de travail visée :**

  (f_r = 2{,}1,\text{GHz} = 2{,}1 \times 10^9,\text{Hz})
* **Vitesse de la lumière :**

  (c = 3 \times 10^8,\text{m/s} = 3 \times 10^{11},\text{mm/s})
* **Substrat :** FR4 / époxy

  – Permittivité relative : (\varepsilon_r = 4{,}4)

  – Épaisseur : (h = 0{,}8,\text{mm})

Ces données sont imposées par l’énoncé du TP.

---

### 2.2 Largeur du patch (W)

On utilise la formule classique :

[

W = \frac{c}{2 f_r} \sqrt{\frac{2}{\varepsilon_r + 1}}

]

1. Demi-longueur d’onde dans l’air :

   (\dfrac{c}{2 f_r} = \dfrac{3\times10^{11}}{2\times 2{,}1\times10^{9}} \approx 71{,}43,\text{mm})
2. Facteur diélectrique :

   (\sqrt{\dfrac{2}{\varepsilon_r+1}} = \sqrt{\dfrac{2}{5{,}4}} \approx 0{,}6086)
3. Largeur du patch :

   (W \approx 71{,}43 \times 0{,}6086 \approx \mathbf{43{,}47,\text{mm}})

---

### 2.3 Permittivité effective (\varepsilon_{\text{eff}})

La permittivité effective tient compte du fait qu’une partie du champ est dans l’air, l’autre dans le substrat :

[

\varepsilon_{\text{eff}} = \frac{\varepsilon_r + 1}{2} + \frac{\varepsilon_r - 1}{2}\left(1 + \frac{12h}{W}\right)^{-1/2}

]

En remplaçant les valeurs :

* (\dfrac{\varepsilon_r + 1}{2} = 2{,}7)
* (\dfrac{\varepsilon_r - 1}{2} = 1{,}7)
* (\dfrac{12h}{W} = \dfrac{12\times 0{,}8}{43{,}47} \approx 0{,}2208)
* (\left(1 + \dfrac{12h}{W}\right)^{-1/2} \approx 0{,}905)

On obtient :

[

\varepsilon_{\text{eff}} \approx 2{,}7 + 1{,}7 \times 0{,}905 \approx \mathbf{4{,}24}

]

---

### 2.4 Allongement électrique (\Delta L)

L’effet de bord allonge électriquement le patch. L’allongement de chaque côté est donné par :

[

\Delta L = 0{,}412 h \cdot \frac{(\varepsilon_{\text{eff}} + 0{,}3)\big(\frac{W}{h} + 0{,}264\big)}{(\varepsilon_{\text{eff}} - 0{,}258)\big(\frac{W}{h} + 0{,}8\big)}

]

Avec (\frac{W}{h} \approx 54{,}34), on obtient numériquement :

* (\Delta L \approx \mathbf{0{,}37,\text{mm}})

---

### 2.5 Longueur physique du patch (L)

La longueur électrique effective est :

[

L_{\text{eff}} = \frac{c}{2 f_r \sqrt{\varepsilon_{\text{eff}}}} \approx \frac{71{,}43}{\sqrt{4{,}24}} \approx 34{,}69,\text{mm}

]

La longueur physique en tenant compte des deux allongements est :

[

L = L_{\text{eff}} - 2\Delta L \approx 34{,}69 - 2\times 0{,}37 \approx \mathbf{33{,}95,\text{mm}}.

]

---

### 2.6 Largeur de la ligne d’alimentation (50 Ω)

La ligne d’alimentation est une microligne 50 Ω réalisée sur le même substrat FR4 ((\varepsilon_r = 4{,}4), (h = 0{,}8,\text{mm})). La largeur (A) est déterminée via le macro **« Impedance Calculation → Thin Microstrip »** de CST :

* Paramètres saisis : (h = 0{,}8,\text{mm}, \varepsilon_r = 4{,}4, f = 2{,}1,\text{GHz}).
* En ajustant la largeur jusqu’à obtenir (Z_0 \approx 50,\Omega), on trouve :

[

A \approx \mathbf{1{,}54,\text{mm}} \quad (Z_0 \approx 50{,}16,\Omega)

]

---

### 2.7 Longueur de la ligne d’alimentation

Le poly de TP impose que la longueur de la ligne d’alimentation soit égale à une demi-longueur d’onde dans l’air afin d’éviter l’excitation d’ondes évanescentes au niveau du port :

> *« la longueur de la ligne d’alimentation devra être égale à la moitié de la longueur d’onde de travail »*

La longueur d’onde dans l’air à 2,1 GHz vaut :

[

\lambda_0 = \frac{c}{f_r} = \frac{3\times 10^{11}}{2{,}1\times10^9} \approx 142{,}86,\text{mm}

]

On en déduit la longueur de la ligne :

[

L_{feed} = \frac{\lambda_0}{2} \approx \mathbf{71{,}43,\text{mm}}.

]

Cette valeur sera utilisée directement comme paramètre (L_{feed}) dans CST pour dessiner la ligne microstrip.

---

### 2.8 Paramètres récapitulatifs (Étape 1)

Les dimensions calculées et choisies pour la modélisation sont :

* **Substrat (FR4) :** (\varepsilon_r = 4{,}4), (h = 0{,}8,\text{mm})
* **Patch :** (W = 43{,}47,\text{mm}), (L = 33{,}95,\text{mm})
* **Ligne d’alimentation (50 (\Omega)) :** largeur (A = 1{,}54,\text{mm}), longueur (L_{feed} = 71{,}43,\text{mm})
* **Dimensions globales du substrat (choisies) :** (W_{sub} = 80,\text{mm}) (plan de masse suffisant), (L_{sub} = 120,\text{mm}) (permet d’intégrer la ligne de longueur (\lambda_0/2)).

---

### 2.9 Figures à insérer (Étape 1)

> *(Placeholders pour tes propres captures d’écran / photos de calculs)*

- **Figure 1 :** Capture d’écran du calcul de l’impédance de la ligne microstrip dans CST (macro *Impedance Calculation*, Thin Microstrip, \(Z_0 \approx 50\,\Omega\)).

---

## 3. Étape 2 – Paramètres CST et liste de variables

Dans cette partie, on met en place le projet CST et la liste de paramètres qui seront utilisés dans toute la modélisation.

### 3.1 Création du projet CST

1. **Template utilisé :**Dans la fenêtre de démarrage de CST, on choisit :`New Project → Antenna → Planar`.Ce template configure automatiquement un environnement adapté aux antennes microbandes.
2. **Unités :**Dans la fenêtre *New Antenna – Planar*, on sélectionne :

   - **Unités de longueur :** `mm`
   - **Unités de fréquence :** `GHz`
   - **Unités de temps :** `ns`
3. **Bande de fréquence :**On définit la plage de simulation :

   - **fmin = 1 GHz**
   - **fmax = 3 GHz**
   - Fréquence de référence : **2,1 GHz** (fréquence de résonance visée).

Après validation (*Finish*), CST crée un projet 3D vide avec ces paramètres globaux.

### 3.2 Définition de la liste de paramètres (Parameter List)

Afin de pouvoir modifier facilement les dimensions de l’antenne, on crée une liste de paramètres correspondant aux valeurs calculées à l’étape 1.

Les paramètres suivants sont ajoutés dans la **Parameter List** :

- `epsilon = 4.4`→ Permittivité relative du substrat FR4.
- `h = 0.8`→ Épaisseur du substrat en mm.
- `W = 43.47`→ Largeur du patch en mm.
- `L = 33.95`→ Longueur du patch en mm.
- `A = 1.5415`→ Largeur de la ligne d’alimentation 50 Ω (valeur issue du macro CST).
- `L_feed = 71.43`→ Longueur de la ligne d’alimentation (\(\lambda_0/2\) à 2,1 GHz).
- `W_sub = 80`→ Largeur du substrat (plan de masse suffisant autour du patch).
- `L_sub = 120`
  → Longueur du substrat (permet de loger la ligne d’alimentation de longueur \(\lambda_0/2\)).

Ces paramètres seront ensuite utilisés pour définir les coordonnées des briques (substrat) et des rectangles/briques (patch et ligne d’alimentation) dans les étapes suivantes.

### 3.3 Figures à insérer (Étape 2)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 2 :** Capture d’écran de la *Parameter List* dans CST avec les paramètres `epsilon`, `h`, `W`, `L`, `A`, `L_feed`, `W_sub`, `L_sub`.

---

## 4. Étape 3 – Modélisation géométrique de l’antenne

Dans cette étape, on construit la géométrie 3D de l’antenne dans CST à partir des paramètres définis précédemment : substrat FR4, patch, ligne d’alimentation microstrip.

### 4.1 Substrat (FR4)

Le substrat est modélisé par un parallélépipède rectangle (Brick) de dimensions (W_{sub} \times L_{sub} \times h) :

* **Nom :** `Substrate`
* **Système de coordonnées :** on centre le substrat en X et on le fait démarrer en Y = 0.

Coordonnées (en mm) :

* (X_{min} = -\dfrac{W_{sub}}{2}), (X_{max} = +\dfrac{W_{sub}}{2})
* (Y_{min} = 0), (Y_{max} = L_{sub})
* (Z_{min} = 0), (Z_{max} = h)

Matériau :

* **Material :** `FR4` (ou `Epoxy`)

  – (\varepsilon_r = \text{epsilon} = 4{,}4)

  – (h = 0{,}8,\text{mm}).

### 4.2 Patch (PEC)

Le patch est une surface conductrice située à la surface supérieure du substrat (plan (Z = h)). Il est centré en X et positionné à l’extrémité de la ligne d’alimentation en Y.

* **Nom :** `Patch`
* **Type d’objet :** Brick très mince ou Rectangle dans le plan (Z = h)
* **Matériau :** `PEC` (Perfect Electric Conductor)

Coordonnées :

* (X_{min} = -\dfrac{W}{2}), (X_{max} = +\dfrac{W}{2})
* (Y_{min} = L_{feed}), (Y_{max} = L_{feed} + L)
* (Z_{min} = h), (Z_{max} = h )

Ainsi, le patch est relié à l’extrémité de la ligne d’alimentation à (Y = L_{feed}).

### 4.3 Ligne d’alimentation microstrip (Feed)

La ligne d’alimentation est une microligne de largeur (A), de longueur (L_{feed}), centrée en X et partant du bord inférieur du substrat ((Y = 0)).

* **Nom :** `FeedLine`
* **Type d’objet :** Brick ou Rectangle dans le plan (Z = h)
* **Matériau :** `PEC`

Coordonnées :

* (X_{min} = -\dfrac{A}{2}), (X_{max} = +\dfrac{A}{2})
* (Y_{min} = 0), (Y_{max} = L_{feed})
* (Z_{min} = h), (Z_{max} = h )

La ligne est ainsi directement connectée au patch à (Y = L_{feed}). La longueur (L_{feed} \approx 71{,}43,\text{mm}) correspond à (\lambda_0/2) à 2,1 GHz, comme demandé dans l’énoncé.

### 4.4 Remarque sur le plan de masse

Dans ce TP, le plan de masse infini n’est pas modélisé par un objet PEC séparé, mais par la **condition aux limites Perfect E** appliquée sur le plan (Z_{min} = 0) (Étape 4).

Le substrat et la ligne microstrip sont donc construits au-dessus de ce plan conducteur parfait.

### 4.5 Figures à insérer (Étape 3)

> *(Placeholders pour les captures d’écran CST)*

* **Figure 3 :** Vue 3D du patch et de la ligne d’alimentation sur le substrat (antenne complète, sans port ni boîtier).

## 5. Étape 4 – Conditions aux limites (Boundaries)

L’objectif de cette étape est de modéliser correctement l’environnement électromagnétique autour de l’antenne. On souhaite que l’antenne rayonne dans un espace ouvert, tout en imposant un plan de masse parfait sous le substrat.

### 5.1 Choix des conditions aux limites

Dans CST, on ouvre la fenêtre :

> `Simulation → Boundaries…`

Les conditions aux limites sont configurées comme suit :

- **Xmin :** `Open (add space)`
- **Xmax :** `Open (add space)`
- **Ymin :** `Open (add space)`
- **Ymax :** `Open (add space)`
- **Zmax :** `Open (add space)`
- **Zmin :** `Electric (Et = 0)`  (Perfect E)

Les conditions « Open (add space) » modélisent un espace ouvert avec une marge d’air supplémentaire autour de l’antenne, afin de limiter les réflexions sur les bords de la boîte de calcul.

Le plan **Zmin = 0** est imposé comme conducteur parfait (Perfect E) et joue le rôle de **plan de masse infini** de la microligne et du patch. Le substrat FR4 et la ligne microstrip sont donc construits au-dessus de ce plan de masse idéal : `Zmin = Electric (Et=0)` et les autres faces en `Open (add space)`.

---

## 6. Étape 5 – Port d’alimentation et calcul du mode (Calculate modes only)

L’objectif de cette étape est de définir le port d’excitation de la ligne microstrip et de vérifier que la ligne présente bien une impédance caractéristique proche de **50 Ω**.

### 6.1 Définition du port d’alimentation

Le port est placé à l’extrémité de la ligne d’alimentation, sur la face **Y = 0**, perpendiculaire à la direction de propagation de la ligne.

1. On se place dans une vue où l’on voit clairement la face **Y = 0** du substrat et de la ligne `FeedLine`.
2. On sélectionne la face correspondante (plan normal à Y) : soit en utilisant `Pick Face`, soit en utilisant les trois points P1, P2, P3 comme indiqué dans le poly de TP.
3. On crée ensuite un **Waveguide Port** :

   > `Simulation → Waveguide Port`
   >

   Le port est défini dans le plan **Y = 0** (normal à Y) et doit :

   - recouvrir entièrement la section de la ligne microstrip (`FeedLine`) ;
   - inclure l’épaisseur complète du substrat (de Z = 0 à Z = h) ;
   - laisser **quelques millimètres d’air** au-dessus de la ligne (par exemple jusqu’à Z = h + 5 mm).

   En coordonnées, on a typiquement :

   - \(X_{\min} \approx -5\ \text{mm}\), \(X_{\max} \approx +5\ \text{mm}\) (le port est plus large que la ligne de largeur \(A \approx 1{,}54\ \text{mm}\)) ;
   - \(Z_{\min} = 0\), \(Z_{\max} = h + 5 = 0{,}8 + 5 = 5{,}8\ \text{mm}\) ;
   - le port est placé à \(Y = 0\).

   Ainsi, le port englobe la ligne et le substrat, avec une marge d’air suffisante pour que le mode microstrip soit correctement excité.

### 6.2 Calcul du mode de la ligne (Time Domain – Calculate modes only)

Pour vérifier l’impédance de la ligne, on utilise le solveur temporel en mode « calculate modes only » :

1. On ouvre le solveur temporel :

   > `Simulation → Setup Solver → Time Domain Solver`
   >
2. On définit de nouveau la bande de fréquence (cohérente avec le reste du TP) :

   - **fmin = 1 GHz**
   - **fmax = 3 GHz**
3. On sélectionne l’option **« Calculate modes only »** (suivant la version de CST, soit via une case à cocher, soit en lançant un calcul de modes dans la boîte du solveur temporel).

Le solveur temporel calcule alors les modes propres associés au *Waveguide Port* défini à Y = 0.

### 6.3 Résultat : impédance de la ligne ≈ 50 Ω

Après le calcul, l’analyse du mode du port se fait via :

> `2D/3D Results → E-Field → Port Modes (Port 1, mode 1)`
> *ou* `Port Modes / Port1 / e1` suivant la version de CST.

Sur la vue de mode (champ E sur la section du port), une boîte d’information affiche les paramètres du mode fondamental :

- **Mode type :** quasi-TEM (mode fondamental de la ligne microstrip)
- **Line impedance :** \(Z_0 \approx 50{,}16\ \Omega\) (à f ≈ 2 GHz)

Cette valeur est en excellent accord avec la conception analytique de la ligne (section 2), où l’on a dimensionné la largeur \(A \approx 1{,}54\,\text{mm}\) pour obtenir 50 Ω sur FR4 (εr = 4,4, h = 0,8 mm).

On peut donc conclure que :

> La ligne microstrip d’alimentation présente une impédance caractéristique très proche de **50 Ω**, ce qui valide le choix de la largeur \(A\) et du modèle de port d’alimentation.

### 6.4 Figures à insérer (Étape 5)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 4 :** Vue du *Waveguide Port* à Y = 0, montrant la section de la ligne et du substrat.
- **Figure 5 :** Vue du mode du port (Port 1, mode 1) avec la boîte d’information affichant
  *Line impedance ≈ 50 Ω* (et éventuellement le type de mode « quasi-TEM »).

## 7. Étape 6 – Solveur fréquentiel et premier S11

L’objectif de cette étape est de calculer la réponse en réflexion de l’antenne patch à l’aide du **solveur fréquentiel** de CST et de vérifier la position et la profondeur du premier minimum de \(|S_{11}|\).

### 7.1 Configuration du solveur fréquentiel

On ouvre le solveur fréquentiel via :

> `Simulation → Setup Solver → Frequency Domain Solver`

Les paramètres suivants sont utilisés :

- **Type de solveur :** Frequency Domain Solver
- **Bande de fréquence :**– \(f_{\min} = 1\,\text{GHz}\)– \(f_{\max} = 3\,\text{GHz}\)
- **Port excité :** Port 1 (waveguide port sur la ligne d’alimentation)

On lance ensuite la simulation (`Start Simulation`). À la fin du calcul, CST génère automatiquement les paramètres S dans :

> `1D Results → S-Parameters → S1,1 (dB)`

### 7.2 Résultat : premier S11 du patch “simple” (sans fentes, sans boîtier)

On trace la magnitude de \(S_{11}\) en dB sur l’intervalle 1–3 GHz. La courbe obtenue présente :

- un **minimum** de \(|S_{11}|\) situé aux alentours de\(\,f \approx 2{,}07\text{–}2{,}10\,\text{GHz}\) (proche de la fréquence visée \(2{,}1\,\text{GHz}\)) ;
- une **profondeur de ce minimum** d’environ
  \(|S_{11}|_{\min} \approx -2{,}5\,\text{dB}\) (ordre de grandeur \(-2\) à \(-3\) dB).

On en déduit que :

- La **fréquence de résonance** du patch est correcte : la cavité résonne bien dans la zone de 2,1 GHz.
- En revanche, **l’adaptation est mauvaise** : un minimum à seulement \(-2{,}5\,\text{dB}\) est très loin du critère classique de \(-10\,\text{dB}\) (correspondant à un ROS acceptable).

Autrement dit, le patch “simple” alimenté directement au bord par une ligne 50 \(\Omega\) résonne à la bonne fréquence, mais son **impédance d’entrée est très différente de 50 \(\Omega\)** (on estime une impédance d’entrée de l’ordre de quelques centaines d’ohms), ce qui entraîne un fort désaccord d’adaptation.

Ce constat justifie les étapes ultérieures du TP (Étape 9), où l’on modifiera la géométrie du patch (ajout de fentes \(y_o, x_o\)) afin de rapprocher l’impédance d’entrée de 50 \(\Omega\) et d’obtenir un minimum de \(|S_{11}|\) plus proche de \(-10\,\text{dB}\).

### 7.3 Figures à insérer (Étape 6)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 6 :** Courbe \(|S_{11}|\,\text{(dB)}\) obtenue avec le solveur fréquentiel pour le patch simple (sans fentes, sans boîtier), montrant un minimum d’environ \(-2{,}5\,\text{dB}\) vers \(2{,}1\,\text{GHz}\).

## 8. Étape 7 – Solveur temporel, filtre AR et comparaison avec le solveur fréquentiel

Après avoir obtenu un premier résultat en réflexion avec le **solveur fréquentiel** (Étape 6), cette étape consiste à :

1. Refaire la simulation avec le **solveur temporel**.
2. Appliquer un **filtre AR (Auto-Regressive)** sur les signaux de port.
3. Comparer les courbes \(|S_{11}|\) issues des deux solveurs.

L’objectif est de montrer que, une fois le filtrage AR appliqué, les résultats temporels et fréquentiels coïncident très bien, tout en mettant en évidence les artefacts numériques du solveur temporel brut.

### 8.1 Simulation avec le solveur temporel (Time Domain)

On ouvre le solveur temporel via :

> `Simulation → Setup Solver → Time Domain Solver`

Les paramètres utilisés sont cohérents avec le reste du TP :

- **Bande de fréquence cible :**– \(f_{\min} = 1\,\text{GHz}\)– \(f_{\max} = 3\,\text{GHz}\)
- **Port excité :** Port 1 (waveguide port sur la ligne microstrip).

On lance ensuite la simulation temporelle complète :

> `Simulation → Start Simulation`

À l’issue de cette simulation, CST fournit une première estimation des paramètres S dans :

> `1D Results → S-Parameters → S1,1 (dB)` (trace d’origine temporelle, souvent notée « temp »).

**Observation :**

- Le minimum de \(|S_{11}|\) se situe toujours aux alentours de \(2{,}1\,\text{GHz}\), comme pour le solveur fréquentiel.
- En revanche, la courbe présente des **ondulations importantes** et du bruit sur l’ensemble de la bande (notamment en dessous de 1,8 GHz et au-dessus de 2,3 GHz).

Ces ondulations sont dues aux effets de fenêtre temporelle et aux limitations de la transformée de Fourier sur un signal tronqué dans le temps. Elles ne correspondent pas à des phénomènes physiques réels, mais à des **artefacts numériques**.

### 8.2 Application du filtre AR sur les signaux de port

Pour améliorer la qualité spectrale des résultats temporels, on applique le **filtre AR** fourni par CST sur les signaux de port :

1. Menu :

   > `Post-Processing (ou Results) → Time Signal Calculations → AR Filter for Port Signals`
   >
2. Dans la fenêtre du filtre AR :

   - On sélectionne le **Port 1**.
   - On indique la bande de fréquence d’intérêt :– \(f_{\min} = 1\,\text{GHz}\)– \(f_{\max} = 3\,\text{GHz}\)
   - On garde les paramètres par défaut du filtre (ordre AR, etc.).
3. On clique sur **Start** pour lancer le filtrage AR.

CST recalcule alors les paramètres S à partir des signaux filtrés, et stocke les nouveaux résultats dans un dossier séparé, typiquement :

> `1D Results → S-Parameters (AR) → S1,1 (dB)`

### 8.3 Comparaison des courbes S11 : temporel vs fréquentiel

On superpose maintenant trois courbes sur un même graphe :

- \(|S_{11}|_{\text{freq}}\) : résultat du **solveur fréquentiel** (Étape 6).
- \(|S_{11}|_{\text{temp}}\) : résultat brut du **solveur temporel** (sans filtre).
- \(|S_{11}|_{\text{AR}}\) : résultat du solveur temporel **après filtre AR**.

**Comparaison :**

- La courbe \(|S_{11}|_{\text{temp}}\) (sans filtre) présente des ondulations et du bruit important en dehors de la zone de résonance.
- La courbe \(|S_{11}|_{\text{AR}}\) est **nettement plus lisse** :
  – le minimum de \(|S_{11}|\) reste centré autour de \(2{,}1\,\text{GHz}\) ;– la forme globale de la courbe suit très bien celle obtenue avec le solveur fréquentiel.
- En pratique, \(|S_{11}|_{\text{AR}}\) et \(|S_{11}|_{\text{freq}}\) sont **quasi confondus** sur toute la bande 1–3 GHz.

On peut donc conclure que :

> Le solveur temporel, combiné au **filtre AR**, fournit des résultats en réflexion \(|S_{11}|\) en excellent accord avec le solveur fréquentiel.
> Le filtre AR permet de **réduire les erreurs numériques** (ondulations, bruit) inhérentes à l’analyse spectrale d’un signal temporel tronqué.

### 8.4 Figures à insérer (Étape 7)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 7 :** Comparaison des paramètres \(|S_{11}|\) en dB obtenus par les différentes méthodes. On observe la courbe temporelle brute avec ondulations (**rouge, \(S_{11,\text{temp}}\)**), la courbe après application du filtre AR qui est lissée (**bleue, \(S_{11,\text{AR}}\)**), et la courbe de référence du solveur fréquentiel (**verte, \(S_{11,\text{freq}}\)**). La superposition quasi-parfaite des courbes bleue et verte valide les résultats du solveur temporel filtré.

## 9. Étape 8 – Visualisation des champs E et effet de bord

L’objectif de cette étape est de visualiser la répartition du champ électrique sur le patch à la fréquence de résonance, afin de mettre en évidence l’**effet de bord** caractéristique des antennes patch microbandes.

### 9.1 Mise en place du monitor de champ E

On commence par ajouter un monitor de champ électrique dans CST :

> `Simulation → Field Monitors → E-Field`

Les paramètres du monitor sont les suivants :

- **Type :** E-Field (fréquentiel)
- **Fréquence :** \(f = 2{,}1\,\text{GHz}\) (fréquence de résonance visée)
- **Région :**
  – domaine complet (Use subvolume ou bounding box par défaut)
  – avec une **visualisation en plan Z** (plan parallèle au patch)

Dans la version utilisée, le choix du plan 2D se fait lors de l’affichage des résultats : le monitor stocke le champ 3D, puis on affiche une **coupe dans le plan \(Z = h\)** (plan du patch) à la fréquence 2,1 GHz.

Après la définition du monitor, on relance une simulation avec le **solveur fréquentiel** (bande 1–3 GHz). À la fin du calcul, CST met à disposition le champ électrique :

> `2D/3D Results → E-Field → e-field (f = 2.1 GHz)`

### 9.2 Visualisation du champ E sur le plan du patch (Z = h)

Pour analyser le champ sur le patch, on utilise une vue en coupe :

- On affiche le résultat `e-field (f = 2.1 GHz)` en 3D.
- On active ensuite une **Sectional View** avec :
  – **Normal :** Z
  – **Position du plan :** \(Z = h \approx 0{,}8\,\text{mm}\)

On obtient alors une **vue 2D** du module (et éventuellement de la direction) du champ électrique sur le plan du patch.

Les observations principales sont :

- Le champ est **maximal au niveau des bords rayonnants** du patch (les deux côtés opposés le long de la largeur \(W\)).
- Le champ au **centre du patch** est nettement plus faible.
- On observe également un champ notable au niveau de la jonction entre la ligne d’alimentation et le patch (point d’alimentation), mais les maxima les plus marqués restent localisés aux **deux bords courts du patch**.

### 9.3 Interprétation : effet de bord et modèle de cavité

Cette distribution de champ est conforme au modèle de cavité d’une antenne patch :

- Le patch se comporte comme une **cavité résonante** dans laquelle le champ est quasi sinusoïdal selon la longueur.
- Les deux bords opposés se comportent comme des **ouvertures rayonnantes** (fentes), où les charges s’accumulent et où le champ électrique est maximal.
- La radiation principale de l’antenne provient donc de ces **effets de bord**, et non pas du centre du patch.

On peut ainsi conclure que :

> La visualisation du champ E sur le plan \(Z = h\) montre clairement que les champs sont concentrés sur les bords rayonnants du patch, ce qui illustre l’**effet de bord** et confirme le modèle théorique de l’antenne patch microbande.

### 9.4 Figures à insérer (Étape 8)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 8 :** Vue 2D/3D du champ électrique à \(f = 2{,}1\,\text{GHz}\) montrant la distribution du champ sur le patch (plan \(Z = h\)), avec des maxima au niveau des bords rayonnants.

## 10. Étape 9 – Amélioration de l’adaptation par fentes (yo, xo)

Les résultats de l’Étape 6 ont montré que le patch simple (sans modification) résonne bien autour de 2,1 GHz, mais avec une adaptation médiocre : \(|S_{11,\min}|\) n’atteint qu’environ \(-2{,}5\,\text{dB}\). L’objectif de cette étape est d’améliorer l’adaptation en modifiant la géométrie du patch à l’aide de **deux fentes symétriques** caractérisées par les paramètres \(y_o\) et \(x_o\).

### 10.1 Principe des fentes (slots) yo, xo

On ajoute deux fentes rectangulaires symétriques de part et d’autre de la ligne d’alimentation :

- la **longueur** de chaque fente, notée \(y_o\), s’étend **le long de Y** à partir du bord inférieur du patch (côté alimentation) vers l’intérieur du patch ;
- la **largeur** de chaque fente, notée \(x_o\), s’étend **le long de X**, de sorte que la ligne d’alimentation (de largeur \(A\)) se trouve au centre, entre les deux fentes.

Ces fentes réduisent localement la largeur effective du patch au niveau du point d’alimentation : le courant est forcé de passer par une « langue » métallique plus étroite, ce qui modifie l’impédance d’entrée et permet de la rapprocher de **50 Ω**.

### 10.2 Paramétrisation et modélisation des fentes dans CST

On introduit deux nouveaux paramètres dans la *Parameter List* :

- `yo` : longueur des fentes le long de Y (en mm)
- `xo` : largeur de chaque fente le long de X (en mm)

Les fentes sont modélisées comme deux briques “air” (ou *Background*) que l’on retire du patch (opération booléenne *Subtract*) :

- Fente gauche `Slot_L` :

  - \(X_{\min} = -\dfrac{A}{2} - x_o\), \(X_{\max} = -\dfrac{A}{2}\)
  - \(Y_{\min} = L_{feed}\), \(Y_{\max} = L_{feed} + y_o\)
  - \(Z_{\min} = h\), \(Z_{\max} = h + t_{Cu}\)
- Fente droite `Slot_R` :

  - \(X_{\min} = +\dfrac{A}{2}\), \(X_{\max} = +\dfrac{A}{2} + x_o\)
  - \(Y_{\min} = L_{feed}\), \(Y_{\max} = L_{feed} + y_o\)
  - \(Z_{\min} = h\), \(Z_{\max} = h + t_{Cu}\)

Étapes dans CST :

1. Création des deux briques `Slot_L` et `Slot_R` avec matériau `Vacuum` ou `Background`.
2. Sélection de `Patch` comme **Target** puis de `Slot_L` et `Slot_R` comme **Tools**.
3. Application de `Modeling → Boolean → Subtract` (en supprimant les Tools après l’opération).

Le patch initial devient alors un patch « fendu » : deux fentes encadrent la ligne d’alimentation, ne laissant qu’une **langue centrale** de largeur \(A\) connectée au reste du patch.

### 10.3 Méthode d’optimisation des paramètres \(y_o\) et \(x_o\)

L’optimisation des fentes a été réalisée de manière itérative :

1. **Variation de \(y_o\) à \(x_o\) fixé**

   - On fixe d’abord \(x_o\) à une valeur raisonnable (ici \(x_o = 5\,\text{mm}\)).
   - On fait varier \(y_o\) (par exemple \(y_o = 6, 8, 10, 12\,\text{mm}\)),et pour chaque valeur, on relance le **solveur fréquentiel** (1–3 GHz) et on observe :
     - la position de la résonance principale ;
     - la profondeur du minimum de \(|S_{11}|\).
   - On retient la valeur de \(y_o\) qui donne le minimum le plus profond autour de \(2{,}1\,\text{GHz}\).
2. **Ajustement de \(x_o\) à \(y_o\) fixé**

   - On fixe ensuite \(y_o\) à la valeur précédente (optimale).
   - On fait varier \(x_o\) (par ex. \(x_o = 3, 4, 5, 6\,\text{mm}\)),en recalculant \(|S_{11}|\) à chaque fois.
   - On retient le couple \((y_o, x_o)\) qui donne un minimum de \(|S_{11}|\) le plus proche possible de \(-10\,\text{dB}\) à \(2{,}1\,\text{GHz}\).

### 10.4 Résultat optimisé et comparaison des S11

Après cette optimisation, on obtient le couple \((y_o, x_o)\) suivant :

- **Valeurs retenues :**
  \[
  y_o = 8\,\text{mm}, \quad
  x_o = 5\,\text{mm}
  \]

Les résultats de simulation montrent alors :

- Pour le **patch simple** (sans fentes) :\[
  |S_{11,\min}| \approx -2{,}5\,\text{dB} \quad \text{à} \quad f \approx 2{,}07\,\text{GHz}
  \]
- Pour le **patch avec fentes optimisées** :
  \[
  |S_{11,\min}| \approx -10{,}5\,\text{dB} \quad \text{à} \quad f \approx 2{,}11\,\text{GHz}
  \]

On observe donc :

- une **amélioration très nette de l’adaptation** (le minimum passe d’environ \(-2{,}5\,\text{dB}\) à environ \(-10{,}5\,\text{dB}\)) ;
- une **fréquence de résonance** qui reste proche de \(2{,}1\,\text{GHz}\), avec une légère translation de \(\sim 2{,}07\) vers \(\sim 2{,}11\,\text{GHz}\).

Cette légère translation fréquentielle s’explique par la **modification du chemin de courant** induite par les fentes, qui affecte légèrement la longueur électrique effective du patch.

On peut conclure que :

> Les fentes \((y_o, x_o)\) jouent le rôle d’un petit réseau d’adaptation intégré au patch :
> elles permettent de réduire l’impédance d’entrée au point d’alimentation et de la rapprocher de **50 Ω**, ce qui se traduit par un minimum de \(|S_{11}|\) proche de \(-10\,\text{dB}\) à 2,1 GHz.

### 10.5 Figures à insérer (Étape 9)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 9 :** Vue 3D du patch fendu (géométrie avec les deux fentes symétriques de longueur \(y_o = 8\,\text{mm}\) et de largeur \(x_o = 5\,\text{mm}\)).
- **Figure 10 :** Courbe \(|S_{11}|\) comparant :
  – le patch simple (sans fentes, minimum ≈ \(-2{,}5\,\text{dB}\) à 2,07 GHz) ;
  – le patch avec fentes optimisées (minimum ≈ \(-10{,}5\,\text{dB}\) à 2,11 GHz).

## 11. Étape 10 – Intégration dans un smartphone (boîtier plastique + écran en verre) et comparaison S11_addphone

L’objectif de cette étape est d’évaluer l’impact de l’environnement « smartphone » sur l’antenne optimisée à l’Étape 9 (\(y_o = 8\,\text{mm}\), \(x_o = 5\,\text{mm}\)). Le smartphone est modélisé de façon simplifiée par un **boîtier plastique** et un **écran en verre**, et l’on compare la courbe \(|S_{11}|\) obtenue **avec** et **sans** smartphone.

### 11.1 Modélisation du smartphone (géométrie + matériaux)

Le smartphone est représenté par :

- une coque **plastique** (bords arrondis), dimensions globales \(58{,}27 \times 123{,}81 \times 7{,}62\,\text{mm}^3\),
  avec un matériau `Plastic` (Type *Normal*), \(\varepsilon_r \approx 3\) ;
- un écran en **verre** placé au-dessus de l’antenne, matériau `Glass` (Type *Normal*), \(\varepsilon_r \approx 5{,}5\),
  avec une épaisseur typique de l’ordre du millimètre.

L’antenne (substrat FR4 + patch + ligne + fentes optimisées) est positionnée à l’intérieur de ce boîtier, conformément au schéma du TP.

### 11.2 Simulation fréquentielle

On relance une simulation avec le **solveur fréquentiel** sur la bande :

\[
f_{\min} = 1\,\text{GHz}, \qquad f_{\max} = 3\,\text{GHz}.
\]

On obtient la nouvelle courbe notée \(|S_{11}|_{\text{addphone}}\), superposée à la courbe de référence de l’antenne optimisée **sans smartphone** (Étape 9).

### 11.3 Comparaison et observations (glissement fréquentiel)

L’ajout du boîtier plastique et de l’écran en verre modifie fortement la courbe \(|S_{11}|\).

- **Glissement fréquentiel de la résonance principale (frequency shift).**La résonance principale, initialement située autour de \(2{,}11\,\text{GHz}\) (antenne optimisée en espace libre), **se décale fortement vers les basses fréquences** et apparaît désormais vers **\(\approx 1{,}43\,\text{GHz}\)** (voir *Mark 4* sur la figure).
- **Désadaptation à la fréquence de travail initiale (2,1 GHz).**À \(2{,}1\,\text{GHz}\), l’antenne devient **fortement désadaptée**, avec \(|S_{11}|\) qui remonte proche de 0 dB (réflexion quasi totale).
- **Résonances supplémentaires (modes parasites).**
  On observe également d’autres minima, par exemple vers **\(\approx 2{,}4\,\text{GHz}\)**, interprétés comme des **résonances parasites** liées au couplage avec la coque et aux modes de cavité du boîtier (effet « enclosure/cavity modes »).

### 11.4 Interprétation physique

Le comportement observé est cohérent avec l’introduction de matériaux diélectriques proches de l’antenne :

1. **Augmentation de la permittivité effective (dielectric loading).**Le verre et le plastique augmentent la permittivité effective vue par l’antenne. Comme la fréquence de résonance est approximativement inversement proportionnelle à \(\sqrt{\varepsilon_{\text{eff}}}\), l’augmentation de \(\varepsilon_{\text{eff}}\) entraîne un **décalage de la résonance vers les basses fréquences**, ce qui explique que le minimum principal « migre » de ~2,11 GHz vers ~1,43 GHz.
2. **Modification des couplages et apparition de modes parasites.**La présence d’un volume diélectrique proche (coque + écran) modifie les champs proches, les chemins de courant et peut exciter des résonances supplémentaires (modes liés à l’environnement du smartphone), visibles ici vers ~2,4 GHz.
3. **Réseau d’adaptation non optimal in-situ.**
   Les fentes (\(y_o, x_o\)) ont été optimisées en espace libre. Dans le smartphone, l’impédance d’entrée change fortement : l’adaptation à 50 \(\Omega\) n’est donc plus assurée à 2,1 GHz.

On conclut que :

> L’intégration dans un smartphone **détériore significativement** les performances à 2,1 GHz : la résonance principale est **fortement décalée** vers ~1,43 GHz et l’antenne devient très désadaptée à la fréquence de travail visée. Une optimisation spécifique « in-situ » (antenne dans le boîtier) serait nécessaire pour retrouver \(|S_{11}| < -10\,\text{dB}\) à 2,1 GHz.

### 11.5 Figures à insérer (Étape 10)

> *(Placeholders pour les captures d’écran CST)*

- **Figure 11 :** Vue 3D de l’antenne intégrée dans le smartphone (coque plastique arrondie + écran en verre).
- **Figure 12 :** Courbes \(|S_{11}|\) superposées : antenne optimisée (sans smartphone) vs \(|S_{11}|_{\text{addphone}}\) (avec smartphone), avec repères (*Mark 4* à ~1,43 GHz, et minimum parasite vers ~2,4 GHz).

## Conclusion / Synthèse

Dans ce TP, une antenne patch microbande destinée à fonctionner autour de **2,1 GHz** a été dimensionnée analytiquement, modélisée sous CST, puis optimisée et intégrée dans un environnement de type smartphone.

Les principaux résultats sont les suivants :

- **Dimensionnement analytique (Étape 1).**Sur FR4 (\(\varepsilon_r = 4{,}4\), \(h = 0{,}8\,\text{mm}\)), on obtient :\(W = 43{,}47\,\text{mm}\), \(L = 33{,}95\,\text{mm}\).La ligne microstrip 50 \(\Omega\) est dimensionnée à \(A \approx 1{,}54\,\text{mm}\) et sa longueur est imposée par l’énoncé à \(L_{feed} = \lambda_0/2 \approx 71{,}43\,\text{mm}\).Pour permettre la modélisation complète, on choisit \(W_{sub} = 80\,\text{mm}\) et \(L_{sub} = 120\,\text{mm}\).
- **Validation du port et de la ligne (Étape 5).**Le calcul des modes au port confirme un mode fondamental **quasi-TEM** et une impédance caractéristique proche de **50 \(\Omega\)** (par ex. \(Z_0 \approx 50{,}16\,\Omega\)), ce qui valide la ligne d’alimentation.
- **Premier S11 (Étape 6) et cohérence des solveurs (Étape 7).**Le solveur fréquentiel montre un minimum de \(|S_{11}|\) proche de la zone 2,1 GHz mais avec une adaptation médiocre (ordre de \(-2\) à \(-3\) dB).Le solveur temporel présente des ondulations numériques ; après application du **filtre AR**, la courbe \(|S_{11}|\) devient lisse et coïncide quasi parfaitement avec le solveur fréquentiel, ce qui confirme la cohérence des méthodes.
- **Champs électriques et effet de bord (Étape 8).**La visualisation du champ \(E\) sur le plan \(Z=h\) met en évidence une concentration du champ au niveau des **bords rayonnants** du patch, illustrant clairement l’**effet de bord** et le modèle de cavité des antennes patch.
- **Optimisation par fentes (Étape 9).**L’ajout de deux fentes symétriques permet d’améliorer fortement l’adaptation. Le couple retenu est :\[
  y_o = 8\,\text{mm}, \quad x_o = 5\,\text{mm}.
  \]
  On obtient alors un minimum d’environ \(|S_{11,\min}| \approx -10{,}5\,\text{dB}\) autour de \(f \approx 2{,}11\,\text{GHz}\).La légère translation fréquentielle observée s’explique par la modification du chemin de courant induite par les fentes, qui change légèrement la longueur électrique effective du patch.
- **Intégration smartphone (Étape 10).**
  L’ajout d’un boîtier plastique et d’un écran en verre modifie fortement l’environnement électromagnétique (chargement diélectrique et couplages), entraînant un **glissement fréquentiel** de la résonance principale vers les basses fréquences et une **désadaptation** nette à 2,1 GHz, avec apparition possible de résonances parasites (modes de cavité).

En conclusion, l’antenne patch peut être efficacement adaptée à 2,1 GHz en espace libre grâce aux fentes (\(y_o, x_o\)), mais son intégration dans un smartphone détériore les performances et nécessite une **re-optimisation in-situ** (optimisation des fentes et/ou de la position dans le boîtier, prise en compte des matériaux et distances réelles).
