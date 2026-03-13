# Projet Demos

Ce projet est une introduction à l'utilisation de R et RStudio.

#### Stack technique

-   R 4.x
-   Tidyverse
-   Python 3.10 (via Reticulate)

#### Installation

### GitHub

Créer le repo sur github, et copier le lien

``` bash
git clone <lien du repo>

git add <fichiers à ajouter>
git commit -m "text de description du commit"
git push origin main
```

### Intaller \`tidyverse\` via la console de Rstudio

``` r
install.packages("tidyverse")
```

Vérifier l'installation :

``` r
library(tidyverse)
```

### Créer l'env via la console de Rstudio

1.  Installer l'outil d'environnement

``` r
install.packages("renv")


# Initialiser l'environnement dans ce projet spécifique
renv::init()

# Vérifie l'état de l'environnement
renv::status()

#  (si besoin) installe tidyvers dans l'env
renv::install("tidyverse")

# mettre à jour le fichier lockfile
renv::snapshot()
```

R demander à redémarrer la session, dire oui.

##### PLAN DE TRAVAIL

1.  Chargement des librairies et des données
2.  Nettoyage et typage des variables
3.  Analyse Univariée (distribution de chaque variable)
4.  Analyse Bivariée (relations entre variables)
5.  Étude des données compositionnelles
6.  Réduction de dimension (ACP/PCA) -----------------------

``` r
# Chargement
df <- read_csv("raw_data/data_abs.csv") 

# Aperçu rapide
glimpse(df)


# Transformation des données en 2 colonnes : Variable/valeur
df_long <- df %>%
select(where(is.numeric)) %>% # On ne garde que les chiffres
pivot_longer(cols = everything(), names_to = "Variables", values_to = "Valeurs")

# création d'un qui affiche toutes les variables
 ggplot(df_long, aes(x = Valeurs)) +
     geom_histogram(fill = "steelblue", bins = 20) +
     facet_wrap(~ Variables, scales = "free") + # "free" permet à chaque axe X/Y de s'adapter
     theme_minimal() +
     labs(title = "Distribution de toutes mes variables numériques",
          y = "Fréquence",
          x = "Valeurs")
```

#### Commandes utiles

``` r
# Affichage sous forme de tableau
View(df)

# (Structure) C'est l'équivalent de df.info(). Elle te montre le type de chaque colonne (num, char, factor) et les premières valeurs.
str(df)

# L'équivalent de df.describe(). Pour les colonnes numériques, elle donne les quartiles, la moyenne et le nombre de valeurs manquantes (NA).
summary(df)

# (Nécessite le package tidyverse) Une version plus lisible et moderne de str().
glimpse(df)

# Ouvre une table interactive style Excel dans RStudio pour faire défiler les données manuellement.
View(df)
```

#### Analyse exploratoire Univariée

``` r
ggplot(df, aes(x = ma_variable_num)) +
  geom_histogram(fill = "steelblue", bins = 30) +
  theme_minimal() +
  labs(title = "Distribution de ma_variable", y = "Fréquence")
```

#### Analyse exploratoire Bivariée

``` r
ggplot(df, aes(x = categorie, y = valeur)) +
  geom_boxplot(aes(fill = categorie)) +
  theme_minimal()
```

#### Réduction de dimension

``` r
# Sélectionner uniquement le numérique pour l'ACP
df_num <- df %>% select(where(is.numeric)) %>% drop_na()

# Exécuter l'ACP
res_pca <- prcomp(df_num, scale. = TRUE)

# Visualiser l'éboulis des valeurs propres (pour choisir le nombre de variables)
screeplot(res_pca, type = "lines")
```
