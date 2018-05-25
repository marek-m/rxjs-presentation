---?image=assets/image/first.jpg
### <span class="white">Redux state management</span>
---

## Stan aplikacji
```javascript
    global.state = {};
```
---
### Plan prezentacji
- Stan aplikacji |
- Modelowanie/Porządkowanie |
- Normalizacja |
- Postać normalna |
- @ngrx/entity |
---
## Stan aplikacji

- Jest globalnym obiektem |
- Przepływ danych jasno określony (jednokierunkowy) |
- Reducery zapisują dane do stanu |
- Selektory pobierają dane ze stanu |
---
![redux-data-flow](assets/image/redux-data-flow.png)
--- 
## Typy danych
- Backend API |
- Dane aplikacji |
- Przykład: Informacje o wybranej zakładce, informacje o danych routingu, paginacji | 
- Zachowanie aplikacji: Stan załadowania danych - loading/loaded, Wybrany element |
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
## Uporządkowanie stanu
- Grupowanie danych tego samego rodzaju |
- Ustanowienie relacji pomiędzy nimi |
- Usunięcie duplikacji |
- Normalizacja |
---
## Postać znormalizowana
Jest to zespół restrykcji/reguł które porządkują model danych .
- 3 postacie normalne (1,2,3NF) gdzie 3 forma najbardziej restrykcyjna | 
- 1NF w każdej kolumnie przechowujemy tylko dane prymitywne (atomowe) |
- 2NF każda dana w kolumnie polega na kluczu głównym (co przekłada się na brak powtarzających się wierszy) |
- 3NF w kolumnach właściwie tylko same klucze... |
- Jeżeli dane są w 2NF to są i w 1NF, jeżeli w 3NF to w 2NF i 1NF... |
---
### Praktyczne zastosowanie ma postać 2 :)
- Spłaszczamy każdy obiekt, jeżeli posiada jakies zagnieżdzone elementy to wyrzucamy je do innego zbioru |
- Dane wiążemy za pomocą kluczy głównych |
---
![user-nested](assets/image/users-nested.png)
--- 
![users-normalized](assets/image/users-normalized.png)
---
## Dlaczego znormalizowane
- Pozbywamy się duplikatów! |
- Pozbywamy się zagnieżdżonych struktur! |
---
## Zduplikowane dane 
- Zwiększanie zużycia pamięci |
- Zduplikowane dane === problemy z modyfikacją |
- Musimy pamiętać o aktualizacji wszystkich miejsca duplikacji na raz... |
---
![user-nested](assets/image/users-nested.png)
---
## Zagnieżdzone dane
- Nadmiarowe odświeżanie widoków | 
- Niepotrzebna złożoność |
- Zniszczenie aktualnego stanu (immutability, predictability) |
- Jak ułożyć reducery?? |
---
![company-nested](assets/image/company-employees-nested.png)
---
![company-example](assets/image/company-example.png)
---
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
<p><span class="slide-title">Operacje na indeksowanym zbiorze</span></p>
![operations-for-map](assets/image/operations-for-map.png)
---
![ngrx-entity](assets/image/ngrx-entity.png)
---
## ADAPTER
Udostępnia metody do zarządzania pojedynczą koleckcją (tablicą) danych określonego typu.
---
## W PRAKTYCE
- addOne, addMany, updateOne, deleteOne, deleteAll |
- Automatyczni generuje podstawowe selektory |
- Generuje strukture indeksowaną z podanej tablicy |
- Możemy od razu pobrać 'initialState' |
---
## Jak to działa
---
## Model danych
```typescript
interface Book {
  id: string;
  title: string;
}
```
---
## 1. Adapter
Pierwszym krokiem jest utworznie adaptera dla modelu danych.

```typescript
import { createEntityAdapter } from '@ngrx/entity';
const bookAdapter = createEntityAdapter<Book>();
```
---
## 2. Interface extends EntityState
Musimy zdeklarować interfejs.
```typescript
import { EntityState } from '@ngrx/entity';
export interface BookState extends EntityState<Book> { }
```
Który pod spodem wygląda tak:
```typescript
interface EntityState<V> {
  ids: string[];
  entities: { [id: string]: V };
}
```
---
## 4. Reducer
---
```typescript
const initialState: BookState = bookAdapter.getInitialState();
export function reducer(
  state: BookState = initialState,
  action: BookActions,
): BookState {
  switch (action.type) {
    case BookActionTypes.ADD_ONE:
      return bookAdapter.addOne(action.book, state);
    case BookActionTypes.UPDATE_ONE:
      return bookAdapter.updateOne({
        id: action.id,
        changes: action.changes,
      }, state);
    case BookActionTypes.DELETE_ONE:
      return bookAdapter.deleteOne(action.id, state);
    case BookActionTypes.GET_ALL:
      return bookAdapter.addAll(action.books, state);
    default:
      return state;
  }
}
```
@[1](initialState pobieramy z adaptera)
@[7-8](Operacja dodania jednej encji.)
@[9-13](Modyfikacja encji o podanym id.)
@[14-15](Usunięcie jednego elementu.)
@[16-17](Zapis całej kolekcji.)
---
## 5. Selectors
Ostatnią rzeczą jaką musimy zrobić jest pobranie selektorów
```typescript
export const {
  selectIds: getBookIds,
  selectEntities: getBooks,
  selectAll: getAllBooks,
  selectTotal: getBooksTotal,
} = bookAdapter.getSelectors();
```
---
## Pytania
![kermit-questions](assets/image/questions2.png)