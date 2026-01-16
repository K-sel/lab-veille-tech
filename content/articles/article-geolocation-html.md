---
title: "La balise HTML <geolocation> arrive : ce que ça change pour les devs indie"
date: 2025-01-16
tags: ["HTML", "Géolocalisation", "Web API", "Chrome"]
categories: ["Outils"]
draft: false
---

Chrome 144 introduit un nouvel élément HTML : `<geolocation>`. Pour les indie hackers qui développent des apps web avec des fonctionnalités de localisation, c'est une simplification bienvenue — et une occasion de réfléchir aux limites du web face au natif.

## Ce que change la balise `<geolocation>`

Jusqu'à maintenant, demander la position d'un utilisateur passait par l'API JavaScript `navigator.geolocation.getCurrentPosition()`. Le problème : cette approche impérative déclenche des popups d'autorisation qui interrompent l'utilisateur, souvent au mauvais moment. Pire, si l'utilisateur refuse trois fois, Chrome bloque silencieusement les demandes pendant une semaine. Ton code échoue sans explication.

La nouvelle balise `<geolocation>` change la donne. L'autorisation est déclenchée uniquement quand l'utilisateur clique sur l'élément — pas au chargement de la page, pas via un script. C'est déclaratif, contextuel, et contrôlé par le navigateur.

```html
<geolocation
  onlocation="handleLocation(event)"
  accuracymode="precise">
</geolocation>
```

```javascript
function handleLocation(event) {
  if (event.target.position) {
    const { latitude, longitude } = event.target.position.coords;
    console.log("Position :", latitude, longitude);
  }
}
```

Plus besoin de gérer manuellement les callbacks, les états d'erreur, les cas de blocage. Le navigateur s'occupe de tout, y compris d'un flux de récupération si l'utilisateur avait précédemment bloqué l'accès.

## Les chiffres qui valident le concept

Avant de lancer la balise `<geolocation>`, Chrome a testé un élément générique `<permission>` pendant plusieurs versions. Les résultats sont parlants :

- **Zoom** : -46,9% d'erreurs de capture caméra/micro
- **Immobiliare.it** : +20% de flux de géolocalisation réussis
- **ZapImóveis** : 54,4% de récupération pour les utilisateurs précédemment bloqués

Le problème n'était pas la fonctionnalité. C'était le manque de contexte au moment de la demande.

## Le parallèle avec mon projet Track

J'ai récemment terminé [Track](https://www.jpinard.ch/projets/tracks), une PWA de tracking sportif développée dans le cadre d'un projet croisé entre deux cours à la HEIG-VD (ArchiOWeb et DevMobil). L'app enregistre les sorties running/vélo via les capteurs du téléphone : tracé GPS en temps réel, calcul de distance et dénivelé, photos géolocalisées, enrichissement météo automatique.

La géolocalisation était au cœur du projet. Avec l'API JavaScript actuelle, j'ai dû gérer pas mal de cas limites : utilisateur qui refuse, blocage silencieux, demande qui arrive trop tôt dans le flow. La balise `<geolocation>` aurait simplifié cette partie du code.

Mais — et c'est là que ça devient intéressant — même avec cette amélioration, le web reste limité face au natif.

## Les limites du web : l'exemple de l'altimètre barométrique

En développant Track, j'ai voulu intégrer l'altitude barométrique. Sur une app native, l'altimètre du téléphone donne une altitude précise et réactive, idéale pour calculer le dénivelé en temps réel.

Sur le web ? Impossible. L'API Geolocation donne une altitude GPS, mais elle est bruitée et peu fiable pour du tracking sportif précis. L'altimètre barométrique, lui, n'est tout simplement pas exposé aux applications web.

C'est le genre de friction qui pousse certains projets vers le natif ou vers des solutions hybrides comme Capacitor. Pour un side project ou un MVP, ça peut être un deal-breaker.

La balise `<geolocation>` améliore l'expérience utilisateur, mais elle ne résout pas ce problème fondamental : certains capteurs restent hors de portée du web.

## Pourquoi c'est pertinent pour les indie hackers

Cette évolution s'adresse surtout aux indie hackers avec un background dev — ceux qui comprennent comment fonctionne l'API Geolocation actuelle et qui peuvent apprécier la simplification.

Concrètement, si tu développes :
- Une app de livraison ou de services géolocalisés
- Un outil de tracking (sport, transport, logistique)
- Une fonctionnalité "trouver autour de moi"

...cette balise va te faire gagner du temps et améliorer ton taux de conversion sur les autorisations.

L'amélioration progressive est aussi bien pensée : tu peux utiliser la balise avec un fallback vers l'API JavaScript pour les navigateurs non compatibles.

```html
<geolocation onlocation="updateMap()">
  <!-- Fallback pour navigateurs non compatibles -->
  <button onclick="navigator.geolocation.getCurrentPosition(updateMap)">
    Utiliser ma position
  </button>
</geolocation>
```

## Quand cette info me sera utile

Pour mes futurs projets demandant la géolocalisation, je garderai cette balise en tête. Elle simplifie le code, améliore l'UX, et réduit les frictions liées aux permissions.

Mais je resterai aussi conscient des limites du web. Pour certains cas d'usage sportifs ou IoT, le natif reste incontournable. La question à se poser avant de commencer : est-ce que les capteurs dont j'ai besoin sont accessibles depuis le navigateur ?

Source : [Chrome for Developers](https://developer.chrome.com/blog/geolocation-html-element?hl=fr)