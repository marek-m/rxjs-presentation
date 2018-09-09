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
+++ Imperatywne vs Deklaratywne
---
## Przykład II
Chcemy pokazać zachowanie się operatorów wyższego rzędu poprzez wizualizację rzucenia piłki.
Po wybraniu operacji dynamicznie tworzymy nowy element HTML i wykonujemy animację spadającej piłki. Zachowanie rzucenia (wcisnięcia przycisku)
sterowane jest za pomocą operatorów switchMap, concatMap, mergeMap i exhaustMap.
---
## Przykład II
Chcemy pokazać zachowanie się operatorów wyższego rzędu poprzez wizualizację rzucenia piłki.
Po wybraniu operacji dynamicznie tworzymy <span class="orange">nowy element HTML</span> i wykonujemy <span class="orange">animację spadającej piłki</span>. Zachowanie rzucenia (wcisnięcia przycisku)
sterowane jest za pomocą operatorów <span class="orange">switchMap, concatMap, mergeMap i exhaustMap</span>.
---
@snap[west half]
![Image](./assets/image/simple-mapping.png)
@snapend
@snap[east half]
![Image](./assets/image/map-steps.png)
@snapend
---
@snap[west left-70]
![Image](./assets/image/animation-function.png)
@snapend
@snap[east right-30]
![Image](./assets/image/interval.gif)
@snapend
---
```typescript
of(1).pipe(
  map(() => this.createNewBall()),
  tap((component) => this.addToContainer(component)),
  switchMap((element: Vue) => {
    return interval(100).pipe(
      map(value => value * (containerHeight / steps),
      map((value: number) => ({element, value})),
      take(steps)
    )
  }),
).subscribe((params) => {
  params.element.$el.style.top = `${params.value}px`;
});
```
@[2-3](Tworzymy element HTML i dodajemy do kontenera)
@[4](Zaczynamy emitować wartość wewnętrzną)
@[5-9](Animacja wartości 0, 1, 2, 3)
@[8](Kończymy Observable po określonej ilości kroków)
@[11-13](Subskrybujemy się i podmieniamy style.top)
---
@snap[west left-30]
![Image](./assets/image/throw-one.gif)
@snapend
@snap[east right-60]
![Image](./assets/image/ready-anim.png)
@snapend
---
![Image](./assets/image/filter-out.png)
---
![Image](./assets/image/filter-out-highlight.png)
---
## Parametryzowany operator
![Image](./assets/image/create-and-animate.png)
---
## [Operatory RxJS](https://rxjs-operators.firebaseapp.com)
---
## Pytania
![kermit-questions](assets/image/questions2.png)
