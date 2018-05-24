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
(OBRAZEK) ZAŁADOWANE DANE I PRZYKŁADOWY STAN (NESTED OBJECTS, NOT CATEGORIZED)
---
## Uporządkowanie stanu
- Grupowanie danych tego samego rodzaju |
- Ustanowienie relacji pomiędzy nimi |
- Usunięcie duplikacji |
- Normalizacja |
---
## Postać znormalizowana
- 1 postać normalna | 
- 2 posać normalna | 
- 3 postać normalna |
- 4, 5 (tylko w rozważaniach teoretycznych) |
---
## 1 postać normalna
(OPIS + PRZYKŁAD)
---
## 2 postać normalna
(OPIS + PRZYKŁAD)
---
## 3 postać normalna
(OPIS + PRZYKŁAD)
---
### Praktyczne zastosowanie ma postać 2
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
Udostępnia metody do zarządzania pojedynczą koleckcją (tablcą) danych określonego typu.
---
## W PRAKTYCE
- addOne, addMany, updateOne, deleteOne, deleteAll itd... |
- Automatyczni generuje podstawowe selektory |
- Generuje strukture indeksowaną z podanej tablicy |
- Możemy od razu pobrać 'initialState' |
---
## Jak to działa
---
## 1. Model danych
```typescript
interface Book {
  id: string;
  title: string;
}
```
---
## 2. Adapter
Pierwszym krokiem jest utworznie adaptera dla modelu danych.

```typescript
import { createEntityAdapter } from '@ngrx/entity';
const bookAdapter = createEntityAdapter<Book>();
```
---
## 3. Interface extends EntityState
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