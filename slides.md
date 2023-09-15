---
theme: the-unnamed #seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## GraphQL
  Presentation
drawings:
  persist: false
transition: slide-left
title: GraphQL

themeConfig:
  default-headingBg: var(--slidev-theme-center-headingBg)
  default-headingColor: var(--slidev-theme-center-headingColor)
---

# GraphQL

Presentation

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: image-right
image: ./images/gql-unplug-rest.png
transition: fade-out
---

# What is GraphQL?

GraphQL est un langage de requête et un environnement d'exécution côté serveur pour les interfaces de programmation d'application (API) qui s'attache à fournir aux clients uniquement les données qu'ils ont demandées, et rien de plus.

Utilisé à la place de REST, GraphQL permet aux développeurs de créer des requêtes qui extraient les données de plusieurs sources à l'aide d'un seul appel d'API.

---
layout: default
---

# Table of contents

<Toc columns="3"></Toc>

---
layout: image
image: ./images/gql-vs-rest.jpg
transition: slide-up
---

---
transition: slide-up
---

# Schémas

Les développeurs d'API utilisent GraphQL pour créer un **schéma** qui liste toutes les données que les clients peuvent demander par l'intermédiaire de ce service.

Un **schéma** GraphQL est constitué de types d'objets, qui définissent le genre d'objet qu'il est possible de demander et les champs qu'il contient.

Lorsque les **requêtes** arrivent, GraphQL les compare au **schéma**, puis exécute celles qui ont été validées.

---
transition: slide-up
layout: two-columns
---

# Système de Types

```graphql
{
  hero {
    name
    friends {
      name
      homeWorld {
        name
        climate
      }
      species {
        name
        lifespan
        origin {
          name
        }
      }
    }
  }
}
```

Les API GraphQL sont organisées en termes de types et de champs, et non de points de terminaison.

::right::

```ts
type Query {
  hero: Character
}

type Character {
  name: String
  friends: [Character]
  homeWorld: Planet
  species: Species
}

type Planet {
  name: String
  climate: String
}

type Species {
  name: String
  lifespan: Int
  origin: Planet
}
```

GraphQL utilise des types pour garantir que les applications ne demandent que ce qui est possible et fournissent des erreurs claires et utiles.

<style>
.slidev-layout h1::before {
  background: var(--slidev-theme-default-headingBg);
}

/*  Default */
.slidev-layout {
  h1 {
    color: var(--slidev-theme-default-headingColor);

    &::before {
      background: var(--slidev-theme-default-headingBg);
    }
  }
}
</style>

---
transition: slide-up
---

# Les opérations GraphQL

Si on les transpose dans le modèle CRUD (create, read, update and delete), une requête correspond à l'action **read**. Toutes les autres actions (create, update et delete) sont gérées par les **mutations**.

---
transition: slide-up
layout: two-cols
---

# Opération de requête

Exemple de requête GraphQL
```graphql
{
  me {
    name
  }
}
```

En Javascript:
```js
await fetch(GRAPHQL_URL, {
  body: JSON.stringify({
    query: `
      query Me{
        me{name}
      }
    `,
  }),
});
```

::right::

Une API GraphQL renverra le résultat suivant au format JSON :
```json
{
  "me": {
    "name": "Dorothy"
  }
}
```

---
transition: slide-up
layout: two-cols
---

# Opération de requête avec argument

Exemple de requête GraphQL avec argument
```graphql
{
  human(id: "1000") {
    name
    location
  }
}
```

::right::

Une API GraphQL renverra le résultat suivant au format JSON :
```json
{
  "data": {
    "human": {
      "name": "Dorothy,
      "location": "Kansas"
    }
  }
}
```

---
transition: slide-up
---

# Communication avec GraphQL (CURL)

On peut utiliser curl ou n’importe quelle autre bibliothèque HTTP.

Dans REST, les verbes HTTP déterminent l’opération effectuée. Dans GraphQL vous fournirez un corps encodé JSON, que vous effectuiez une requête ou une mutation; le verbe HTTP est donc POST.

L’exception est une requête d’introspection, qui est un simple GET au point de terminaison.

Pour interroger GraphQL dans une commande curl, effectuez une requête POST avec une charge utile JSON. La charge utile doit contenir une chaîne nommée query :

```sh
curl -H "Authorization: bearer TOKEN" -X POST -d " \
 { \
   \"query\": \"query { viewer { login }}\" \
 } \
" https://api.github.com/graphql
```

---
transition: slide-up
layout: image-right
image: ./images/graphql-explorer-starwars.png
---

# GraphQL Explorer

Si vous utilisez GitHub, vous pouvez rapidement tester GraphQL grâce à GraphQL Explorer pour GitHub.

https://developer.github.com/v4/explorer/

<br/>
<br/>
https://studio.apollographql.com/public/star-wars-swapi/variant/current/explorer


---
transition: slide-up
---

# Point de terminaison GraphQL
L’API REST a de nombreux points de terminaison; l’API GraphQL, elle, n’en a qu’un seul :

```
https://api.github.com/graphql
```

Le point de terminaison reste constant, quelle que soit l’opération que vous effectuez.

---
transition: slide-up
---

# Les avantages

- Un schéma GraphQL définit une source unique de vérité dans une application GraphQL. Il permet à une entreprise de fédérer l'ensemble de son API.
- Les appels GraphQL sont gérés par un seul aller-retour. Les clients obtiennent précisément ce qu'ils ont demandé, rien de plus.
- Les types de données sont définis rigoureusement, ce qui limite les problèmes de communication entre le client et le serveur.
- GraphQL est introspectif. Un client peut demander une liste des types de données disponibles.
- GraphQL permet de faire évoluer l'API d'une application sans dégrader les requêtes existantes.
- Il existe de nombreuses extensions GraphQL Open Source qui proposent des fonctions indisponibles dans les API REST.
- GraphQL n'impose aucune architecture d'application spécifique. Il peut être mis en œuvre sur une API REST existante et fonctionner avec les outils de gestion des API existants.

---
transition: slide-up
---

# Les inconvénients

- Les développeurs qui travaillent habituellement avec des API REST devront apprendre à utiliser GraphQL.
- GraphQL déplace l'essentiel de la charge de travail d'une requête de données du côté serveur, ce qui complique la tâche des développeurs serveur.
- Selon la mise en œuvre, les stratégies de gestion des API nécessaires pour GraphQL peuvent différer de celles des API REST, notamment en matière de limites de débit et de tarifs.
- La mise en cache est plus complexe qu'avec les API REST.
- Les équipes chargées de la maintenance des API doivent écrire un schéma GraphQL supplémentaire compatible avec les opérations de maintenance.

---
transition: slide-up
---

# GraphQL et l'Open Source
GraphQL a été développé par Facebook, qui a commencé à l'utiliser pour les applications mobiles en 2012. La spécification de GraphQL est devenue Open Source en 2015. Elle est désormais supervisée par la GraphQL Foundation.

Divers projets Open Source utilisent GraphQL :

- **Apollo** : plateforme GraphQL qui comprend une bibliothèque client front-end (Apollo Client) et une structure de serveur back-end (Apollo Server).
- **Offix** : client hors ligne qui permet d'exécuter les mutations et requêtes GraphQL, même lorsqu'il est impossible de communiquer avec une application.
- **Graphback** : client en ligne de commande qui s'utilise pour générer des serveurs Node.js compatibles avec GraphQL.
- **OpenAPI-to-GraphQL** : interface en ligne de commande et bibliothèque pour traduire les API décrites par des spécifications OpenAPI ou Swagger dans le langage GraphQL.

---
transition: slide-up
layout: two-cols
---

# Qui utilise GraphQL ?

- Facebook (depuis 2012)
- Github
- Pinterest
- Shopify
- Coursera
- Airbnb

::right::

- Club Med
- Dailymotion
- Atlassian
- Allociné
- NBC
- PayPal
- Rakuten

---
transition: slide-up
layout: center
---

The end