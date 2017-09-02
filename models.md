# Model (tag: v3)

Generujemy model poleceniem: ```ng g class app.model``` (na razie dużo nie oczekujemy). Kod naszego modelu powinien wyglądać mniej więcej tak (nie po wygenerowaniu - sami piszemy):
```ts
export interface IVolumeRaw {
    value: number;
    unit: string;
}

export interface IIngredientRaw {
    name: string;
    amount: IVolumeRaw;
    add?: string;
    attribute?: string;
}

export interface IIngredientsRaw {
    hops: IIngredientRaw[];
    malt: IIngredientRaw[];
    yeast: string;
}

export interface IBeerRaw {
    id: number;
    name: string;
    description: string;
    image_url: string;

    attenuation_level: number;
    volume: IVolumeRaw;
    boil_volume: IVolumeRaw;
    brewers_tips: string;
    contributed_by: string;
    first_brewed: string;
    food_pairing: string[];
    ingredients: IIngredientsRaw;
    tagline: string;

    abv: number;
    ebc: number;
    ibu: number;
    srm: number;
    ph: number;
}

export class Volume {
    value: number;
    unit: string;

    constructor (iv: IVolumeRaw) {
        this.value = iv.value;
        this.unit = iv.unit;
    }
}

export class Igrendient {
    name: string;
    amount: Volume;
    add: string = '';
    attribute: string = '';

    constructor (ii: IIngredientRaw) {
        this.name = ii.name;
        this.amount = ii.amount;
        this.add = ii.add || '';
        this.attribute = ii.attribute || '';
    }
}

export class Ingredients {
    hops: Igrendient[];
    malt: Igrendient[];
    yeast: string;

    constructor (iis: IIngredientsRaw) {
        this.hops = iis.hops.map(i => new Igrendient(i));
        this.malt = iis.malt.map(i => new Igrendient(i));
        this.yeast = iis.yeast;
    }
}

export class Beer {
    id: number;
    name: string;
    description: string;
    imageUrl: string;

    attenuationLevel: number;
    volume: Volume;
    boilVolume: Volume;
    brewersTips: string;
    contributedBy: string;
    firstBrewedBy: string;
    foodPairing: string[];
    ingredients: Ingredients;
    tagline: string;

    abv: number;
    ebc: number;
    ibu: number;
    srm: number;
    ph: number;

    constructor (b: IBeerRaw) {
        this.id = b.id;
        this.name = b.name;
        this.description = b.description;
        this.imageUrl = b.image_url;

        this.attenuationLevel = b.attenuation_level;
        this.volume = new Volume(b.volume);
        this.boilVolume = new Volume(b.boil_volume);
        this.brewersTips = b.brewers_tips;
        this.contributedBy = b.contributed_by;
        this.firstBrewedBy = b.first_brewed;
        this.foodPairing = b.food_pairing;
        this.ingredients = new Ingredients(b.ingredients);
        this.tagline = b.tagline;

        this.abv = b.abv;
        this.ebc = b.ebc;
        this.ibu = b.ibu;
        this.srm = b.srm;
        this.ph = b.ph;
    }

    public getShortDescription = (chars: number) => this.description.substring(0, chars);

}
```