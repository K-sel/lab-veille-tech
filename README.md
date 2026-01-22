# LabVeilleTech

Blog de veille technologique sur l'univers des **indie hackers** — entrepreneurs qui construisent des produits rentables sans lever de fonds.

Projet réalisé dans le cadre du cours de **Veille Technologique** (HEIG-VD, Ingénierie des Médias, 3ème année).

**Site en ligne** : [k-sel.github.io/lab-veille-tech](https://k-sel.github.io/lab-veille-tech/)

## Objectifs du cours

Construire un système de veille personnalisé et documenter l'apprentissage à travers 5 articles :

- **3 articles de veille** avec justification de leur pertinence pour le domaine choisi
- **1 article sur le système de veille** : sources, outils, routine
- **1 article sur les LLM et l'apprentissage** : comment utiliser les IA pour apprendre efficacement

La page "À propos" présente l'Ikigai, la taxonomie du blog et le choix du générateur de site statique.

## Stack technique

- **Hugo** + thème PaperMod
- **GitHub Pages** + GitHub Actions (déploiement automatique)
- Contenu en **Markdown**

## Commandes

```bash
# Serveur de dev (avec drafts)
hugo server -D

# Build production
hugo --minify

# Nouveau post
hugo new articles/mon-article.md
```

## Structure

```
content/
├── articles/    # Articles de veille
└── about.md     # Page À propos
```

## Auteur

**Jonathan Pinard**
Étudiant en Ingénierie des Médias (HEIG-VD)

- GitHub : [@k-sel](https://github.com/k-sel)
- LinkedIn : [jonathan-music](https://linkedin.com/in/jonathan-music)
