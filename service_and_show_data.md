# Nasz pierwszy service (tag: v3)

Generujemy service poleceniem: ```ng g service app.service```. W tym przypadku ```angular-cli``` wygenerował nam także testy do serwisu.

Pierwszym krokiem będzie zmiana nazwy serwisu z ```AppService``` na ```BeerService```.

W serwisie piszemy funkcje pomocnicze, które będą potrzebne do wykonywania zapytań:
```ts
public getPublic (url: string): Observable<any> {
    return this._http
        .get(url, this.getOptions())
        .map(this.extractData)
        .catch(error => {
            console.log(error);
            return Observable.throw(error);
        });
}

private getOptions (headers = {}): RequestOptions {
    headers['Accept'] = 'application/json';
    return new RequestOptions({ 'headers': new Headers(headers) });
}

private extractData (res: Response): any {
    if (res.status < 200 || res.status >= 300) {
        throw new Error('BAD RESPONSE STATUS: ' + res.status);
    }
    if (res) {
        try {
            return res.json();
        } catch (e) {}
    }
    return null;
}
```

Dodajemy "mappery" - funkcje, dzięki którym będziemy mapować dane przychodzące z serwera:
```ts
private mapBeer = (response: any) => new Beer(<IBeerRaw> response);

private mapBeers = (response: any) => {
    const rawBeers = <IBeerRaw[]> response;
    return rawBeers.map(this.mapBeer);
}
```

Na koniec funkcje, które będziemy bezpośrednio wywoływać w komponencie i dzięki nim pobierać dane:
```ts
public getBeers (): Observable<Beer[]> {
    return this.getPublic(BASE_BEER_URL).map(this.mapBeers);
}

public getBeer (id: number): Observable<Beer[]> {
    return this.getPublic(BASE_BEER_URL + '/' + id).map(this.mapBeers);
}

public getRandomBeer () {
    return this.getPublic(RANDOM_BEER_URL).map(this.mapBeer);
}
```

Do ```src/app/app.module.ts``` dodajemy ```HttpModule```:
najpierw import:
```ts
import { HttpModule } from '@angular/http';
```
a potem sam moduł ```HttpModule``` do ```imports``` i ```providers```

Na koniec podłączamy nasz ```service``` do komponentu:
```ts
import { Component } from '@angular/core';

import { Beer } from './app.model';
import { BeerService } from './app.service';


@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
    providers: [BeerService] // musimy dodać serwis do providerów
})
export class AppComponent implements OnInit {
    public beers: Beer[]; // tworzymy zmienną, która będzie przechowywała nasze pobrane piwa

    public isInitialized: boolean = false; // zmienna określająca czy pobraliśmy wszystkie dane potrzebne do wyświetlenia strony

    constructor (
        private _beerService: BeerService // wstrzykujemy serwis do komponentu
    ) {}

    (...)

}
```
