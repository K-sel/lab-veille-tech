---
title: "Claude Cowork exfiltre des fichiers : ce que ça signifie pour les indie hackers"
date: 2025-01-15
tags: ["Sécurité", "IA", "Indie Hacking"]
categories: ["Retours d'expérience"]
draft: false
---

Une vulnérabilité découverte dans Claude Cowork permet à des attaquants d'exfiltrer les fichiers des utilisateurs. Pour les indie hackers qui construisent leurs produits avec l'aide de l'IA, c'est un signal d'alarme.

## Ce que l'attaque permet

Des chercheurs en sécurité ont démontré comment exploiter une faille dans l'environnement d'exécution de code de Claude Cowork. L'attaque fonctionne par **prompt injection indirecte** : un fichier malveillant (un document Word, un "skill", ou n'importe quel fichier uploadé) contient des instructions cachées que Claude exécute sans demander l'approbation de l'utilisateur.

Le vecteur d'attaque est particulièrement vicieux. Les instructions malveillantes sont dissimulées dans un fichier .docx avec une police de 1 point, du texte blanc sur fond blanc, et un interligne de 0.1. Impossible à détecter visuellement.

Une fois le fichier traité par Claude, l'injection lui ordonne d'uploader les fichiers de l'utilisateur vers l'API Anthropic — mais sur le compte de l'attaquant. L'API Anthropic étant whitelistée dans la VM de Claude, la requête passe sans problème.

Résultat : des documents confidentiels (estimations de prêts, données financières, numéros de sécurité sociale partiels) se retrouvent sur le compte Anthropic d'un inconnu.

## Ce qu'Anthropic dit aux utilisateurs

Anthropic prévient que Cowork est une "research preview avec des risques uniques" et recommande de surveiller les "actions suspectes pouvant indiquer une prompt injection". Le problème, comme le souligne Simon Willison : on ne peut pas demander à des utilisateurs non-techniques de repérer des attaques par prompt injection.

La vulnérabilité avait été signalée avant même le lancement de Cowork. Elle a été reconnue mais pas corrigée.

Source de l'article : [PromptArmor - Claude Cowork Exfiltrates Files](https://www.promptarmor.com/resources/claude-cowork-exfiltrates-files)

---

## Pourquoi c'est pertinent pour les indie hackers

Cette vulnérabilité dépasse le cadre technique. Elle touche à deux réalités que les indie hackers feraient bien de garder en tête.

### On fait trop confiance aux IA

On se confie aux LLM comme à un collègue. On leur parle de nos problèmes, de notre business, de nos clients, de nos finances. On uploade des fichiers sensibles sans y réfléchir.

Si des attaques comme celle-ci sont possibles, elles pourraient aussi exfiltrer des informations personnelles ou compromettantes. Tout ce qui part dans une IA peut potentiellement ressortir — pas forcément via l'IA elle-même, mais via les failles de son écosystème.

Ce n'est pas de la paranoïa. C'est juste comprendre que ces outils, aussi pratiques soient-ils, ne sont pas des coffres-forts.

### Les indie hackers sont particulièrement exposés

La communauté indie hacker est motivée, créative, et capable de shipper vite. Mais soyons honnêtes : beaucoup ne viennent pas de parcours d'ingénierie et n'ont jamais été sensibilisés aux bases de la sécurité informatique.

Le scénario classique : quelqu'un de motivé mais néophyte prompte sans relâche une IA pour générer son SaaS en quelques semaines. L'objectif est de lancer vite, à moindre coût, et de commencer à vendre. Le code généré est copié-collé sans être vraiment compris.

Le problème, c'est que les opérations sensibles — gestion des `.env`, intégration de paiements, authentification, protection contre les injections SQL, gestion des fuites de données — sont entièrement déléguées à l'IA. Et l'IA n'est pas infaillible. Elle génère du code qui fonctionne, pas forcément du code sécurisé.

Si un hacker trouve une faille dans un SaaS en production qui génère des ventes et traite des données clients, les conséquences peuvent être sévères : fuite de données, responsabilité légale, perte de confiance, fin du business.

### Le minimum à retenir

- **Comprendre ce que l'IA génère.** Pas besoin d'être expert, mais il faut savoir ce que fait le code qu'on met en prod.
- **Ne jamais déléguer la sécurité à 100%.** L'IA est un assistant, pas un responsable sécurité.
- **Être conscient des risques d'exfiltration.** Ce qu'on uploade ou partage avec une IA peut potentiellement être exposé.
- **Se former sur les bases.** Authentification, gestion des secrets, validation des inputs — ce n'est pas optionnel quand on gère des données utilisateurs.

L'indie hacking, c'est construire vite et léger. Mais vite ne veut pas dire négligent. Et léger ne veut pas dire vulnérable.