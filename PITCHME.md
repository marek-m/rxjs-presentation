---?image=assets/image/first.jpg
### <span class="white">Observable - pipe</span>
---

### Plan prezentacji
- RxJS <= 5.4 |
- Dlaczego używać pipe |
- Migracja z v5 do v6 |
- Zastosowanie RxJS |
- Imperatywne vs Deklaratywne |
---
## RxJS <= 5.4
- Dot chaining |
- Patchowanie operatorów |
- Tworzenie operatorów |
- Importy |
---
```
{
  loading: true,
  entities: [
    articles: [
      {
        id: "123",
        author: {...},
        comments: [ ... ]
      }
    ],
  ],
  uiState: {
    selectedRouting: {
        path: '/.../.../'
    },
    selectedTab: 1,
    visibleViews: [{name: ...}]
  },
  entities2: [...],
  otherEntities: [...]
}
```
---
## Migracja RxJS v5.x => v6
- Import paths
- Zmiana nazw operatorów |
- Operator pipe |
- rxjs-compat
---
## Import paths
```

```
## Zmiana nazw operatorów

## Operator pipe

## rxjs-compat









<p><span class="slide-title">Stan znormalizowany</span></p>
![company-normalized](assets/image/company-employees-normalized.png)
---
<p><span class="slide-title">@Normalizr</span></p>

```
//INPUT
{
  "id": "123",
  "author": {
    "id": "1",
    "name": "Paul"
  },
  "title": "My awesome blog post",
  "comments": [
    {
      "id": "324",
      "commenter": {
        "id": "2",
        "name": "Nicole"
      }
    }
  ]
}
// SCHEMAS
import { normalize, schema } from 'normalizr';

// Define a users schema
const user = new schema.Entity('users');

// Define your comments schema
const comment = new schema.Entity('comments', {
  commenter: user
});

// Define your article
const article = new schema.Entity('articles', {
  author: user,
  comments: [comment]
});

const normalizedData = normalize(originalData, article);

//OUTPUT
{
  result: "123",
  entities: {
    "articles": {
      "123": {
        id: "123",
        author: "1",
        title: "My awesome blog post",
        comments: [ "324" ]
      }
    },
    "users": {
      "1": { "id": "1", "name": "Paul" },
      "2": { "id": "2", "name": "Nicole" }
    },
    "comments": {
      "324": { id: "324", "commenter": "2" }
    }
  }
}
```
@[1-19](Przed normalizacją.)
@[20-38](Definiowanie schematów.)
@[39-58](Wynik normalizacji.)
---
<p><span class="slide-title">Operacje na tablicy</span></p>
![operations-for-array](assets/image/operations-array.png)
---

---
## Podsumowując
- 1 |
- 2 |
- 3 |
---
## [Redux structuring reducers @fa[external-link gp-download]](https://redux.js.org/recipes/structuring-reducers)
---
## Pytania
![kermit-questions](assets/image/questions2.png)
