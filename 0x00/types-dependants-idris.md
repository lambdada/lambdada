# Les types dépendants en Idris

## Idris

* [Idris](http://www.idris-lang.org/) est un langage de programmation généraliste utilisant les types dépendants
* Voir [l'article Wikipedia](https://en.wikipedia.org/wiki/Dependent_type#Comparison_of_languages_with_dependent_types) pour une liste exhaustive (ou pas...) de langages implémentant un typage avec des types dépendants

Un exemple classique, celui des *vecteurs de taille n*, où le type `Vect` est défini à partir d'une taille (`Nat`) et d'un *type* d'éléments (`Type`):

```idris
data Vect : Nat -> Type -> Type where
    Nil  : Vect Z a
    (::) : a -> Vect k a -> Vect (S k) a
```

## Les types dépendants

* À quoi ça sert ? Définir des contraintes plus fortes que les types habituels qui seront vérifiées à la compilation. L'exemple classique est celui de la fonction `head` (ou `car` en LISP) qui retourne le premier élément d'un vecteur ou liste. En voici une définition en Haskell:

   ```haskell
   head :: [ a ] -> a
   head (a : _) = a
   head []      = undefined
   ```


* Cette fonction a un problème : elle n'est pas définie correctement pour *toutes* les listes. Si la liste est vide, une erreur se produit.
* Avec Idris, on peut définir une fonction `head` qui ne s'applique qu'aux listes non vides:

   ```idris
   head :: Vect (S k) a -> a
   head (a :: as) = a
   ```

  Le fait que `Vect` contienne la longueur du vecteur permet de décrire le type contenant au moins un élément: `Vect (S k) a`
* La théorie des types dépendants est ancienne, elle remonte aux travaux sur la théorie des types de Per Martin-Löf, entre autres, dans les années 60-70
* Le principe général c'est de permettre de mélanger *termes* et *types* dans les expressions de types
* Le boulot du compilateur devient beaucoup plus compliqué car il doit non seulement unifier des expressions de types, mais aussi faire des calculs arbitraires lors de cette vérification. Dans le pire des cas, l'inférence de types devient indécidable
