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
---
## Imperatywne vs Deklaratywne
@snap[west half]
GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.
@snapend

@snap[east half]
- 1 |
- 2 |
- 3 |
@snapend
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
