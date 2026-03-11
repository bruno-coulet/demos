# Projet Demos

Ce projet est une introduction à l'utilisation de R et RStudio.

#### Stack technique
- R 4.x
- Tidyverse
- Python 3.10 (via Reticulate)

#### Installation

### GitHub
Créer le repo sur github, et copier le lien
```bash
git clone <lien du repo>

git add <fichiers à ajouter>
git commit -m "text de description du commit"
git push origin main
```
### Intaller tidyverse`via la console de Rstudio
```R
install.packages("tidyverse")
```
Vérifier l'installation :
```R
library(tidyverse)
```


### Créer l'env via la console de Rstudio
1. Installer l'outil d'environnement
```R
install.packages("renv")
```

2. Initialiser l'environnement dans ce projet spécifique
```R
renv::init()
```
Vérifie l'état de l'environnement
```R
renv::status()
```

R demander à redémarrer la session, dire oui.



##### PLAN DE TRAVAIL
1. Chargement des librairies et des données
2. Nettoyage et typage des variables
3. Analyse Univariée (distribution de chaque variable)
4. Analyse Bivariée (relations entre variables)
5. Étude des données compositionnelles
6. Réduction de dimension (ACP/PCA)
 -----------------------


```R
# Chargement
df <- read_csv("raw_data/data_abs.csv") 

# Aperçu rapide
glimpse(df)
```

#### Analyse exploratoire Univariée
```R
ggplot(df, aes(x = ma_variable_num)) +
  geom_histogram(fill = "steelblue", bins = 30) +
  theme_minimal() +
  labs(title = "Distribution de ma_variable", y = "Fréquence")
```

#### Analyse exploratoire Bivariée
```R
ggplot(df, aes(x = categorie, y = valeur)) +
  geom_boxplot(aes(fill = categorie)) +
  theme_minimal()
```


#### Réduction de dimension
```R
# Sélectionner uniquement le numérique pour l'ACP
df_num <- df %>% select(where(is.numeric)) %>% drop_na()

# Exécuter l'ACP
res_pca <- prcomp(df_num, scale. = TRUE)

# Visualiser l'éboulis des valeurs propres (pour choisir le nombre de variables)
screeplot(res_pca, type = "lines")
```

