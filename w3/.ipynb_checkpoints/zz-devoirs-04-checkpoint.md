---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.1
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# à faire pour le cours #4

+++

## finir le tp sur les collages

commencé en cours

+++

## couper en tranches

on part du jeu de données du titanic

```{code-cell} ipython3
# votre code
import pandas as pd

df = pd.read_csv('titanic.csv')
```

1. ajoutez une colonne - appelons-là `par-age` - qui contient une donnée de type catégorie, pour répartir les passagers en 4 tranches d'âge:

- 0 à 20 ans
- 20 à 40 ans
- 40 à 60 ans
- 60 ans et plus

````{admonition} indice
:class: tip dropdown

nous n'avons pas vu ça en classe, il vous faut aller farfouiller dans la documentation du coté de `pandas.cut`
```````

```{code-cell} ipython3
# votre code

df['par-age'] = pd.cut(df['Age'],[0,20,40,60,100])
```

2. produisez la table suivante qui donne le nombre de survivants par catégories définies selon 4 critères

```{image} zz-devoirs-04-img1.png
:width: 400px
:align: center
```

```{code-cell} ipython3
# votre code

pivot=df.pivot_table(
    index=['Sex','par-age'],
    columns=['Pclass','Embarked'],
    values='Survived',
    aggfunc="sum"
)

pivot
```

## 

+++

3. quels sont les types des colonnes ? pourquoi n'obtient-on pas des entiers ?

```{code-cell} ipython3
# votre code

pivot.dtypes
```

Avec ma méthode j'obtiens pourtant bien des entiers

+++

4. refaites le même travail mais:

   - on découpe cette fois en 4 quartiles; on leur donne les noms: enfants, jeunes, adultes seniors (voyez cette fois `pandas.qcut`)
   - on supprime la fragmentation par le port de départ
  
vous devez obtenir alors ceci

```{image} zz-devoirs-04-img2.png
:width: 250px
:align: center
```

```{code-cell} ipython3
# votre code

df['par-age'] = pd.qcut(df['Age'], [0, .25, .5, .75, 1.], ['enfants', 'jeunes', 'adultes', 'seniors'])

pivot = df.pivot_table(
    index=['Sex', 'par-age'],
    columns=['Pclass'],
    values=['Survived'],
    aggfunc='sum'
)
pivot
```
