Se face singura instanta a serviciului la nivelul intregii aplicatii.

```typescript
@Injectable({
  providedIn: 'root'
})
class HeroService {}
```

Mai exista optiunea `providedIn: 'platform'` care se foloseste pentru cazul de multiaplicatie. 

## OPTIUNI PENTRU PROVIDERS

Factory:

```typescript
class HeroService {
  constructor(
    private logger: Logger,
    private isAuthorized: boolean) { }
  getHeroes() {
    const auth = this.isAuthorized ? 'authorized' : 'unauthorized';
    this.logger.log(`Getting heroes for ${auth} user.`);
    return HEROES.filter(hero => this.isAuthorized || !hero.isSecret);
  }
}

const heroServiceFactory = (logger: Logger, userService: UserService) =>
  new HeroService(logger, userService.user.isAuthorized);
  
export const heroServiceProvider = {
  provide: HeroService,
  useFactory: heroServiceFactory,
  deps: [Logger, UserService]
};
```

UseValue - cand se doreste folosirea unui serviciu parametrizat: 

```typescript
providers: [{ provide: FlowerService, useValue: { emoji: '🌷' } }]
```

## DECORATORS

Cand se include un serviciu intr-un constructorul unei entitati, acesta isi cauta definirea in lista de `providers` a componentei. Daca nu este gasit acolo, se cauta in componenta parinte, si tot asa pana la radacina aplicatiei. Aceste cautari se pot modifica dupa urmatorii decoratori:

- `Optional()`:  specifica faptul ca serviciul este optional, si daca nu este gasit in lista de providers, acestuia ii va fi atribuita valuarea `null`, programul nu va da o eroare
- `Self()`: serviciul se cauta doar in lista de providers a componentei de baza, daca nu se gaseste programul da eroare.
- `SkipSelf()`: opusul lui `Self()`
- `Host()`: ??



