# Wyświetlamy nasze dane (tag: v4)

Dodajemy proste wywołanie komponencie ```src/app/app.component.ts```:
```ts
import { Component, OnInit } from '@angular/core'; // dodajemy Interface OnInit

import { Beer } from './app.model';
import { BeerService } from './app.service';


@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
    providers: [BeerService]
})
export class AppComponent implements OnInit { // <- UWAGA dodajemy "implements OnInit"
    public beers: Beer[];

    public isInitialized: boolean = false;

    constructor (
        private _beerService: BeerService
    ) {}

    ngOnInit () { // nowa funkcja która wywoła się po załadowaniu komponentu
        this._beerService.getBeers().subscribe(b => {
            this.beers = b;
            console.log(this.beers);
            this.isInitialized = true;
        });
    }

}
```

Teraz w konsoli powinna pokazać się lista naszych danych. Gdy mamy dane w zmiennej możemy ją przekazać do ```template``` i ładnie wyświetlić. Przed tym dodajemy moduły ```MdProgressSpinnerModule``` i ```MdListModule``` do importów. Teraz możemy wyświetlić listę naszych piwerek.

Jak możemy bindować zmienne w templacie:
```html
<img src="{{ angularLogo }}">
<img [src]="angularLogo">
<img bind-src="angularLogo">
```

Lista:
```html
<md-toolbar color="primary">
    <span>Beers</span>
</md-toolbar>

<md-spinner *ngIf="!isInitialized"></md-spinner>

<div *ngIf="isInitialized">
    <div>
        <md-list>
            <md-list-item *ngFor="let beer of beers">
                <img md-list-avatar [src]="beer.imageUrl" />
                <h4 md-line>{{ beer.name}}</h4>
                <p md-line>{{ beer.getShortDescription(35) }}</p>
            </md-list-item>
        </md-list>
    </div>

</div>
```

Dodajmy naszej apce trochę polotu i finezji dodając klasy do elementów poniżej:
```html
<md-list-item *ngFor="let beer of beers" class="item-beer">
    <img md-list-avatar [src]="beer.imageUrl" />
    <h4 md-line class="item-beer__name">{{ beer.name}}</h4>
    <p md-line class="item-beer__decription">{{ beer.getShortDescription(35) }}</p>
</md-list-item>
```

... i odpowiednie definicje klas w pliku ```src/app/app.component.css```
```css
.item-beer {
    cursor: pointer;
}

.item-beer .item-beer__name:hover {
    text-decoration: underline;
}

.item-beer .item-beer__description {
    // wypełnij sam / sama
}
```