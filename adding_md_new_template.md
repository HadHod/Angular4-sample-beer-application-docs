# Konfiguracja Material Designe & pierwsze zmiany w template (tag: v1)

Wchodzimy na stronę [Material Designe dla Angular](https://material.angular.io/guide/getting-started) i integrujemy go z naszym projektem.

Importujemy pierwszy moduł z dodatkowej biblioteki. W pliku ```src/app/app.module.ts``` dodajemy szablon (pod importami) dla łatwiejszego zarządzania importami:
```ts
const MD_COMPONENTS = [];
const DIRECTIVES = [];
const PIPES = [];
```
uzupełniamy dyrektywę modułu:
```ts
@NgModule({
    declarations: [
        AppComponent
    ],
    imports: [
        BrowserModule,
        MD_COMPONENTS, DIRECTIVES, PIPES
    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule {}
```

Teraz możemy dodać ```component``` z ```meterial designe``` i go używać:

```ts
import { MdToolbarModule } from '@angular/material';

const MD_COMPONENTS = [MdToolbarModule];

```

Aktualizujemy ```src/app/app.component.ts``` i ```src/app/app.component.html``` aby zobaczyć czy wszystko poprawnie działa. Z komponentu usuwamy tylko niepotrzebne zmienne.

Przykładowy template:
```html
<md-toolbar color="primary">
    <span>Beers</span>
</md-toolbar>

<div>
    Content
</div>
```

Wybieramy [API](https://github.com/toddmotto/public-apis)! ja wybrałem: [Piwa](https://punkapi.com/)