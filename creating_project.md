# Tworzymy pierwszy projekt (tag: init)

1. Instalujemy angular-cli: ```npm install -g @angular/cli```

1. Nowy projekt tworzymy poleceniem: ```ng new [nazwa projektu]```

1. Repozytorium projektu na potrzeby szkolenia: ```https://github.com/HadHod/Angular4-sample-beer-application```

1. Projekt uruchamiamy poleceniem ```ng serve``` (domyslny port :4200)

### Projekt ma postawione tagi na poszczególnych etapach, lista tagów:
Na konkretny wybrany tag przechodzimy poleceniem ```git checkout [nazwa tagu]``` (np. ```git checkout v4```)

* init (inicjalny projekt, który się stworzy po wpisaniu komendy ```ng new [nazwa projektu]```)
* v1 (Material Designe)
* v2 (Modele)
* v3 (Service)
* v4 (Wyświetlanie listy)
* v5 (Nowa podstrona z detalami)

### Ściąga do [Angular-cli](https://github.com/angular/angular-cli/wiki)

| Scaffold | Usage |
| ------------- |:-------------:|
| Component | ng g component my-new-component |
| Directive | ng g directive my-new-directive |
| Pipe | ng g pipe my-new-pipe |
| Service | ng g service my-new-service |
| Class | ng g class my-new-class |
| Guard | ng g guard my-new-guard |
| Interface | ng g interface my-new-interface |
| Enum | ng g enum my-new-enum |
| Module | ng g module my-module |

### Brakujący routing

plik ```/src/app/app-routing.module.ts```
```ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
oraz import do głównego modułu:
```ts
import { AppRoutingModule } from './app-routing.module';
```
