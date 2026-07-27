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
    └── repositories.example.yml
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

### Šta je potrebno da bi implementacija krenula na kodu

1. Migrirati postojeći platform kod u `./platform/` (u okviru ovog repozitorijuma).
2. Popuniti lokalne putanje i URL-ove u `integration/repositories.local.yml` na osnovu `integration/repositories.example.yml`.
   - Napomena: `./_external/...` putanje u example fajlu su samo prenosivi placeholder-i.
3. Potvrditi koji repozitorijum sadrži:
   - frontend input handling,
   - domain state management,
   - API/event contracts.

Dok se to ne potvrdi, implementacija je pripremljena i specificirana, ali ne može biti izvršena nad realnim modulima.
