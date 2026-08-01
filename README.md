# SpajaNikOpenEvolution

Centralni repozitorijum za migraciju i povezivanje postojeće **SpajaNikOpenEvolution** platforme sa ostalim repozitorijumima u jedan spojeni sistem.

## Cilj

Pošto postojeća platforma već postoji na drugoj lokaciji (izvorni/eksterni repozitorijum), ovaj repozitorijum je pripremljen kao centralno mesto gde se:

1. prenosi postojeći kod platforme,
2. čuva struktura sistema,
3. povezuje rad sa ostalim repozitorijumima.

## Predložena struktura

```text
.
├── platform/                 # postojeći kod platforme koji se prenosi
└── integration/
    ├── repositories.example.yml
    ├── backspace.feature.yml
    └── trikonam.feature.yml
```

## Migracija postojeće platforme

1. Preuzmi postojeći kod platforme iz izvornog okruženja.
2. Kopiraj kompletan sadržaj platforme u direktorijum `platform/`.
3. Ukloni eventualne tajne podatke iz konfiguracionih fajlova pre commita.
4. Commit-uj migrirane fajlove u ovaj repozitorijum.

## Povezivanje sa ostalim repozitorijumima

U fajlu `integration/repositories.example.yml` nalazi se šablon za evidenciju repozitorijuma koji čine spojeni sistem (servisi, biblioteke, front-end, itd.).

Preporuka:

- napravi lokalnu kopiju `integration/repositories.example.yml` kao `integration/repositories.local.yml`,
- popuni realne URL-ove i putanje,
- koristi te podatke za lokalno podizanje i koordinaciju servisa.

## TRIKONAM implementacija (inicijalna specifikacija i rollout paket)

Specifikacija za TRIKONAM — trostepenu selekcijsku/navigacionu akciju — definisana je u `integration/trikonam.feature.yml`.

### Šta je definisano

- **Scope i ciljna ponašanja** za TRIKONAM akciju (trostepena sekvenca sa međustatom).
- **User flow** i acceptance kriterijumi.
- **Implementacioni redosled** (event plumbing → domain/state → UI/consumer).
- **Test strategija** (unit, integration, regresija i edge slučajevi).
- **Operativna spremnost** (telemetrija, rollback, feature flag `feature.trikonam.enabled`).
- **Release faze** (ograničeno puštanje, monitoring, širenje).
- **Sve 3 implementacione opcije** (frontend input layer, domain/state logika, API/event contracts) su mapirane i specificirane.

### Šta je potrebno da bi implementacija krenula na kodu

1. Migrirati postojeći platform kod u `./platform/`.
2. Popuniti lokalne putanje u `integration/repositories.local.yml`.
3. Potvrditi precizno značenje TRIKONAM akcije i definiciju trostepenog trigera.
4. Definisati timeout trajanje za nedovršene TRIKONAM sesije.

---

## EVRYTHING FOR ALL (inicijalna specifikacija i rollout paket)

Ova inicijativa je definisana u `integration/evrything-for-all.feature.yml`.

### Cilj

EVRYTHING FOR ALL je **međuslojni (cross-cutting) rollout paket** čiji je cilj da sve platformske sposobnosti (BACKSPACE, TRIKONAM i buduće funkcionalnosti) budu dostupne **svim korisnicima** na **svim okruženjima** (web, mobilni, desktop) — bez faznih isključivanja ili ograničenja po korisničkoj ulozi.

### Relacija prema BACKSPACE i TRIKONAM

Ova inicijativa upravlja **finalnim full-rollout aktiviranjem** BACKSPACE i TRIKONAM funkcionalnosti:

- Feature flag `feature.evrything-for-all.enabled` aktivira sve podređene flagove (`feature.backspace.enabled`, `feature.trikonam.enabled`) po defaultu.
- Individualni feature flagovi i dalje mogu biti isključeni nezavisno u svrhu rollbacka.

### Šta je definisano

- **Scope i ciljna ponašanja** za inicijativu (univerzalna dostupnost bez faznih isključivanja).
- **User flow pokrivenost**: svi postojeći flows (input editing, trostepena selekcija, navigacija) i budući.
- **Implementacioni redosled**: aktivacija svih flagova → provera paritetnosti između okruženja → full rollout.
- **Test strategija** (unit po funkcionalnosti, integracija između okruženja, regresija svih postojećih flows).
- **Operativna spremnost** (telemetrija, rollback, feature flag `feature.evrything-for-all.enabled`).
- **Release faze** (internal testing → full rollout, bez limited-cohort faze).
- **Sve 3 implementacione opcije** (frontend input adapteri za sva okruženja, uklanjanje environment-phase gateova u core-platform, multi-client API contracts) su mapirane i specificirane.

### Šta je potrebno da bi implementacija krenula na kodu

1. Migrirati postojeći platform kod u `./platform/`.
2. Popuniti lokalne putanje u `integration/repositories.local.yml`.
3. Potvrditi dostupnost mobilnog i desktop adaptera u `frontend-input` repozitorijumu.
4. Potvrditi da API contract verzioning u `api-and-event-contracts` podržava sve tipove klijenata.

---

## BACKSPACE implementacija (inicijalna specifikacija i rollout paket)

Pošto kod platforme još nije migriran u `platform/`, ova faza implementacije uvodi:

1. jedinstvenu specifikaciju ponašanja,
2. mapu zavisnosti i granica odgovornosti,
3. rollout/validacioni plan spreman za izvršavanje čim se izvorni kod unese.

Detalji su u `integration/backspace.feature.yml`.

### Šta je definisano

- **Scope i ciljna ponašanja** za BACKSPACE akciju.
- **User flow** i acceptance kriterijumi.
- **Implementacioni redosled** (event plumbing → domain/state → UI/consumer).
- **Test strategija** (unit, integration, regresija i edge slučajevi).
- **Operativna spremnost** (telemetrija, rollback, feature flag).
- **Release faze** (ograničeno puštanje, monitoring, širenje).
- **Sve 3 implementacione opcije** (frontend input layer, domain/state logika, API/event contracts) su mapirane i specificirane.

### Šta je potrebno da bi implementacija krenula na kodu

1. Migrirati postojeći platform kod u `./platform/` (u okviru ovog repozitorijuma).
2. Popuniti lokalne putanje i URL-ove u `integration/repositories.local.yml` na osnovu `integration/repositories.example.yml`.
   - Placeholder vrednosti `<SET_FRONTEND_PATH>` i `<SET_SERVICE_PATH>` znače da putanju moraš eksplicitno uneti lokalno.
3. Podesiti lokalne putanje za sva 3 repozitorijuma/oblasti:
   - frontend input handling (`frontend-input`),
   - domain state management (`core-platform`),
   - API/event contracts (`api-and-event-contracts`).

Dok se platform kod ne migrira i lokalne putanje ne popune, implementacija je pripremljena i specificirana, ali ne može biti izvršena nad realnim modulima.
