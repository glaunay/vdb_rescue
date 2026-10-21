# Visualisation de Données Biologiques à l'aide de Jupyter et matplotlib
Tous les exercices de cette série dédiée à [Jupyter](https://jupyter.org/) devront se dérouler dans un environnement virtuel créé lors de la première séance.
## Mise en place
Les étapes de mise en place doivent être suivies à la lettre pour vous permettre une pratique sereine et une autonomie maximale lors de toute cette série de TPs. Les configurations aternatives telles que *JupyterLab* ou *plugin-vscode-jupyter*, par exemple, sont à **proscrire** (adressez-vous à votre encadrant pour en savoir plus). 

La configuration de départ idéale est une distribution linux équipée d'un interprèteur python>=3.12. Si tel n'est pas votre cas, interpellez votre encadrant.

Commençons par forker et cloner, le présent repository et deplaçons nous à l'intérieur.

```sh
git clone https://gitlab-pedago.univ-lyon1.fr/vdb/jupyter_serie 
cd jupyter_serie
```

### Installation avec uv (recommandée)
[uv](https://docs.astral.sh/uv/) est un gestionnaire de projets Python qui crée l'environnement virtuel et y installe les librairies déclarées dans `pyproject.toml`, aux versions figées dans `uv.lock`. Il gère aussi l'interpréteur Python : la version requise (>=3.12, <3.14) est téléchargée si elle est absente de votre machine.

Installez uv (une seule fois) :

> [!TIP]
> **uv**
> ```sh
> curl -LsSf https://astral.sh/uv/install.sh | sh
> ```

Puis, à la racine du repository :

> [!TIP]
> **uv**
> ```sh
> uv sync
> ```

Cette commande crée le répertoire `.venv/` et y installe toutes les dépendances. Activez ensuite l'environnement avec `source .venv/bin/activate`, ou préfixez simplement vos commandes par `uv run` (par exemple `uv run jupyter notebook`).

Si vous utilisez uv, les sections *Création*, *Activation* et *Installation des librairies* ci-dessous (installation avec `venv` et `pip`) ne vous concernent pas.

## installation avec venv (moins recommandée)

### Création de l'environnment
Dans le répertoire de votre repository git, tapez

> [!NOTE]
> **venv**
> ```sh
> python -m venv venv_jupyter_serie
> ```

Cela créé un repertoire `venv_jupyter_serie` contenant les libraries python qui seront utilisées pour ces TPs.

### Activation de l'environnement
L'environnement devra toujours être activés dans les SHELL que vous utiliserez lors du TP. Pour cela, 'sourcez' le script `activate`. Une fois "activé", le nom de l'environnement est mentionné entre parenthèses dans le prompt du SHELL.

> [!NOTE]
> **venv**
> ```sh
> $> source ./venv_jupyter_serie/bin/activate
> $(venv_jupyter_serie)>
> ```

### Installation des librairies 
Un fichier, contenant toutes les librairies Python necessaires vous est fourni. il se nomme `requirements.txt`. Demandez à `pip` d'installer ces libraries depuis un SHELL avec l'environnement activé en tapant.

> [!NOTE]
> **venv**
> ```sh
> $(venv_jupyter_serie)> pip install -r requirements.txt
> ```

## Mise en place des fichiers 
* Le jeu de données auxiliaire est fourni avec le repository, dans le répertoire `data/`.

* L'organisation de votre répository est donc la suivante: 
```
jupyter_serie/
        |_venv_jupyter_serie/   (ou .venv/ si vous utilisez uv)
        |_data/
        |_exercices/
        |_pyproject.toml
        |_uv.lock
        |_requirements.txt

```

### Commencer à travailler avec les notebook Jupyter

Votre environnement est desormais capable de servir les notebooks contenus dans les repertoires exercices.
Vous pouvez dès maitenant démarrer le serveur Jupyter:

> [!NOTE]
> **venv**
> ```sh
> $(venv_jupyter_serie)> jupyter notebook
> ```

Ou depuis uv:

> [!TIP]
> **uv**
> ```sh
> uv run jupyter notebook
> ```


## Série de travaux pratiques

### Etude de cas: Protéomique quantitative

* Contexte biologique: Effet de la Tetracycline sur le contenu protéomique d'*E. coli*

* Données tabulées de Spectrométrie de Masse
   - 3 mutants *E.Coli* dans 3 conditions
   - ~2000 protéines quantifiées.


## Détails des Travaux Pratiques
Les TP suivants sont disponibles dans le répertoire `exercices` au **format JUPYTER** (extension ipynb):

### TP0
 - Introduction à matplotlib
### TP1

- Lecture des données et manipulation avec la librairie [PANDAS](https://pandas.pydata.org/).
- Manipulation des bibliothèques SciPy et Numpy
- Production de figures matplotlib

### TP2

- Scatter-plot simple et avancé
- Mise en forme riche d'objet dans le notebook

### TP3

- Un premier volcano plot interactif
- Rappel sur les palettes de couleurs
- Implémentation d'un volcano plot complexe


### TP4
- widgets Jupyter
- layout Jupyter
- exemple basé sur volcano-plot
- projet libre

### TP5
- Visualisation de réseaux avec networkx
- Widget et mise à jour interactive d'un réseau d'interactions protéine-protéine
- 💖 💖 **Most Beautiful Network Contest** 💖 💖

## Version navigateur (JupyterLite) A IGNORER

Les TPs sont aussi publiés comme site [JupyterLite](https://jupyterlite.readthedocs.io/) : Python tourne dans le navigateur (noyau Pyodide), sans installation. Le site est construit et publié sur GitLab Pages par la CI (`.gitlab-ci.yml`) depuis la branche `wasm` ; son URL est indiquée dans *Deploy › Pages*. Toute la machinerie est dans le dossier `jupyter-lite/`.

- **Contenus** : `exercices/` et `data/` sont embarqués avec leur arborescence, les chemins relatifs (`../data/…`, `figs/…`) fonctionnent donc comme en local (`jupyter-lite/jupyter_lite_config.json`).
- **Dépendances** : `jupyter-lite/scripts/make_wheels.py` lit `uv.lock` (issu de `pyproject.toml`) et place dans `jupyter-lite/pypi/` les wheels des paquets absents de Pyodide (`obogo`, `ipympl`, `ipywidgets`…). Le site les sert lui-même : le repli sur PyPI est désactivé (`jupyter-lite/jupyter-lite.json`). Le script échoue si un paquet nécessite une compilation WebAssembly. Après une modification de `pyproject.toml`, lancer `uv lock` et committer `uv.lock`.
- **Installation dans un notebook** : numpy, pandas, matplotlib et networkx sont préchargés au démarrage du noyau ; les autres paquets de Pyodide (scipy…) se chargent à l'`import`. Les paquets embarqués sont installés automatiquement, sans cellule visible, s'ils sont déclarés dans les métadonnées du notebook :
  ```json
  "metadata": { "jupyterlite": { "packages": ["obogo"] } }
  ```
  À chaque démarrage ou redémarrage du noyau, `jupyter-lite/custom/auto-install.js` les installe et affiche un bandeau « Mise en place des dépendances » en bas à gauche. TP2, TP3 et TP4 déclarent leurs paquets ; ces métadonnées sont ignorées en local. Dans un notebook sans métadonnées, on peut toujours installer à la main : `import piplite; await piplite.install(["obogo"])`.

Construction locale :

```bash
cd jupyter-lite
make install   # crée .venv-lite avec la chaîne de build (requirements-lite.txt)
make serve     # construit dist/ et le sert sur http://localhost:8000
```

`jupyter-lite/custom/` et `jupyter-lite/scripts/customize_site.py` (logo, favicon, bandeau sur l'enregistrement des fichiers, correctif du Launcher, installation automatique) proviennent de [python-educ](https://gitlab-pedago.univ-lyon1.fr/glaunay/python-educ).
