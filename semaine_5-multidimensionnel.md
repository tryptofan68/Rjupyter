
# Matrice de corrélation

La matrice de données comporte beaucoup de données manquantes : quelle option faut-il utiliser dans la commande cor pour calculer la matrice de corrélation ?
--> use="pairwise.complete.obs"

Dans la commande "cor" permettant de calculer la matrice de corrélation entre plusieurs variables, quel est l’effet de l’option use="complete.obs" ?
--> Les observations comportant au moins une donnée manquante parmi toutes les variables sont retirées du calcul de la corrélation entre deux variables

Dans la commande "cor" permettant de calculer la matrice de corrélation entre plusieurs variables, quelle est l’utilité de l’option use="pairwise.complete.obs" ?
--> Seules les observations comportant au moins une donnée manquante parmi les deux variables considérées dans le calcul de la corrélation sont retirées du calcul de la corrélation


```R
data = read.csv2('../data//smp1.csv')
str(data)
```

'data.frame':	799 obs. of  9 variables:
 $ age      : int  31 49 50 47 23 34 24 52 42 45 ...
 $ prof     : Factor w/ 8 levels "agriculteur",..: 3 NA 7 6 8 6 3 2 6 6 ...
 $ dep.cons : int  0 0 0 0 1 0 1 0 1 0 ...
 $ scz.cons : int  0 0 0 0 0 0 0 0 0 0 ...
 $ grav.cons: int  1 2 2 1 2 1 5 1 5 5 ...
 $ n.enfant : int  2 7 2 0 1 3 5 2 1 2 ...
 $ rs       : int  2 2 2 2 2 1 3 2 3 2 ...
 $ ed       : int  1 2 3 2 2 2 3 2 3 2 ...
 $ dr       : int  1 1 2 2 2 1 2 2 1 2 ...

```R
var = c('age', 'n.enfant', 'scz.cons', 'dep.cons', 'grav.cons', 'rs', 'ed', 'dr')
round(cor(data[, var], use="complete.obs"), digits=3)
```


<table>
<thead><tr><th></th><th scope=col>age</th><th scope=col>n.enfant</th><th scope=col>scz.cons</th><th scope=col>dep.cons</th><th scope=col>grav.cons</th><th scope=col>rs</th><th scope=col>ed</th><th scope=col>dr</th></tr></thead>
<tbody>
	<tr><th scope=row>age</th><td> 1.000</td><td> 0.441</td><td>-0.044</td><td>-0.110</td><td>-0.139</td><td>-0.223</td><td>-0.038</td><td> 0.003</td></tr>
	<tr><th scope=row>n.enfant</th><td> 0.441</td><td> 1.000</td><td> 0.003</td><td> 0.002</td><td>-0.055</td><td>-0.126</td><td> 0.011</td><td> 0.015</td></tr>
	<tr><th scope=row>scz.cons</th><td>-0.044</td><td> 0.003</td><td> 1.000</td><td> 0.064</td><td> 0.290</td><td> 0.021</td><td> 0.077</td><td>-0.009</td></tr>
	<tr><th scope=row>dep.cons</th><td>-0.110</td><td> 0.002</td><td> 0.064</td><td> 1.000</td><td> 0.439</td><td> 0.107</td><td> 0.259</td><td> 0.093</td></tr>
	<tr><th scope=row>grav.cons</th><td>-0.139</td><td>-0.055</td><td> 0.290</td><td> 0.439</td><td> 1.000</td><td> 0.151</td><td> 0.234</td><td> 0.001</td></tr>
	<tr><th scope=row>rs</th><td>-0.223</td><td>-0.126</td><td> 0.021</td><td> 0.107</td><td> 0.151</td><td> 1.000</td><td> 0.093</td><td> 0.088</td></tr>
	<tr><th scope=row>ed</th><td>-0.038</td><td> 0.011</td><td> 0.077</td><td> 0.259</td><td> 0.234</td><td> 0.093</td><td> 1.000</td><td> 0.115</td></tr>
	<tr><th scope=row>dr</th><td> 0.003</td><td> 0.015</td><td>-0.009</td><td> 0.093</td><td> 0.001</td><td> 0.088</td><td> 0.115</td><td> 1.000</td></tr>
</tbody>
</table>
```R
install.packages("corrplot", repos='http://cran.us.r-project.org')
library(corrplot)
```

Installing package into 'C:/R/library'
(as 'lib' is unspecified)

package 'corrplot' successfully unpacked and MD5 sums checked

The downloaded binary packages are in
​	C:\Users\Sébastien\AppData\Local\Temp\RtmpysbPLg\downloaded_packages

corrplot 0.84 loaded

```R
corrplot(cor(data[, var], use="complete.obs"), method="circle")
```


![png](output_7_0.png)


# PCA

A partir d'une matrice de correlation, il est possible de calculer
$$ d = \sqrt{2(1-r)} $$
les distances allant de 0 à 2.
1. Si r est voisin de 1 alors d est voisin de 0 = correlation importante <-> proximité des points projetés SEULEMENT si proche du bord de la sphère principale.
2. Si d est voisin de 2 alors r est voision de -1 = points dimaétralement opposés et proche du bord.
3. Si d est voision de $\sqrt{2}$ alors r est voisin de 0 = pas de corrélation pour des points séparés par un angle droit.

Attention, les points projetés vers le centre ne sont pas interprétables...


```R
str(var)
```

 chr [1:8] "age" "n.enfant" "scz.cons" "dep.cons" "grav.cons" "rs" "ed" ...

```R
install.packages("psy", repos='http://cran.us.r-project.org')
library(psy)
```

Installing package into 'C:/R/library'
(as 'lib' is unspecified)

package 'psy' successfully unpacked and MD5 sums checked

The downloaded binary packages are in
​	C:\Users\Sébastien\AppData\Local\Temp\RtmpysbPLg\downloaded_packages

```R
mdspca(data[, var])
# F1 + F2 = 40% variance expliquée
# ATTENTION no stat inference possible...
```


![png](output_12_0.png)

```R
sphpca(data[,var], v=60) # rotation
```


![png](output_13_0.png)


Une ACP focalisée est utile si l’on dispose d’une variable à expliquer et des plusieurs variables explicatives Une ACP focalisée est utile si l’on dispose d’une variable à expliquer et des plusieurs variables explicatives.


```R
expliquer = 'grav.cons'
explicatives = c('age', 'n.enfant', 'dep.cons', 'scz.cons', 'rs', 'ed', 'dr')
fpca(data=data, y=expliquer, x=explicatives, partial='No')
```


![png](output_15_0.png)


# Classification

hclust(dist(t(scale(smp.l[,var]))),method="ward.D") produit un message The "ward" method has been renamed to "ward.D"; note new "ward.D2". Il faut donc utiliser la méthode ward.D ou ward.D2.


```R
cha = hclust(dist(t(scale(data[, var]))), method="ward")
```

The "ward" method has been renamed to "ward.D"; note new "ward.D2"

```R
cha = hclust(dist(t(scale(data[, var]))), method="ward.D")
```


```R
plot(cha, xlab='', ylab='', main='Classification hiérarchique')
```


![png](output_20_0.png)

