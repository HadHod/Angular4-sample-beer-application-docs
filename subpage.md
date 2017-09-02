# Nowa podstrona z detalami piwa (tag: v5)

Tworzymy nową podstronę w której wyświetlimy więcej informacji o konkretnym rekordzie. Zacznijmy od wygenerowania komponentu ```ng g component beer-details```.

### Podłączamy nowy komponent do routingu
* Dodajemy nowe importy
    * ```import { AppRoutingModule } from './app-routing.module';```
    * ```import { BeerDetailsComponent } from './beer-details/beer-details.component';```
* Do ```declarations``` i ```exports``` dodajemy ```BeerDetailsComponent```
* ```AppRoutingModule``` dodajemy do ```imports```
* W pliku ```src/app/app-routing.module.ts``` importujemy nasz nowy komponent ```import { BeerDetailsComponent } from './beer-details/beer-details.component';``` i dodajemy go do ścieżek:
```ts
const routes: Routes = [
    {
        path: 'details/:beerId',
        component: BeerDetailsComponent,
        pathMatch: 'full'
    }
];
```
* Do ```app.component.html``` dodajemy ```<router-link>```:
```html
<div>
    <router-outlet></router-outlet>
</div>
```
i link do obrazka do każdego rekordu:
```html
<img md-list-avatar [routerLink]="['/details/' + beer.id]" [src]="beer.imageUrl" />
```

### Wyświetlanie szczegółów piwa

* Dodajemy do głównego modułu ```MdCardModule```. Od teraz możemy używać karty z ```Material Designe```.
* Modyfikujemy wygenerowane pliki

##### beer-details.component.ts
```ts
import { Component, OnInit } from '@angular/core';
import { Router, ActivatedRoute } from '@angular/router';

import { Beer } from './../app.model';
import { BeerService } from './../app.service';


@Component({
  templateUrl: './beer-details.component.html',
  styleUrls: ['./beer-details.component.css'],
  providers: [BeerService]
})
export class BeerDetailsComponent implements OnInit {
    public beer: Beer;

    public isInitialized: boolean = false;

    constructor (
        private _router: ActivatedRoute,
        private _beerService: BeerService
    ) { }

    ngOnInit() {
        this._router.params.subscribe(params => this.init(params));
    }

    private init (params) {
        const id: number = params['beerId'];

        this._beerService.getBeer(id).subscribe(([beer]) => {
            this.beer = beer;
            this.isInitialized = true;
        });
    }

}
```

##### beer-details.component.html
```html
<div *ngIf="isInitialized">
    <md-card class="beer-card">
        <md-card-header class="beer-card__name">
            <a>{{ beer.name }}</a>
        </md-card-header>
        <img md-card-image class="header-image" src="{{ beer.imageUrl }}">
        <md-card-content>
            <p>{{ beer.description }}</p>
        </md-card-content>
    </md-card>
</div>
```

##### beer-details.component.css
```css
.beer-card {
    width: 400px;
    height: 750px;
    margin: 12px;
    float: left;

    position: fixed;
    top: 72px;
    right: 10px;
}

.beer-card .beer-card__name {
    margin-bottom: 8px;
}

.header-image {
    width: 300px;
    height: 600px;
    background-size: cover;
}
```