# SpajaNikOpenEvolution

Centralni repozitorijum za migraciju i povezivanje postojeće **SpajaNikOpenEvolution** platforme sa ostalim repozitorijumima u jedan spojeni sistem.

## Cilj

Pošto postojeća platforma već postoji na drugoj lokaciji (OpenAI okruženje), ovaj repozitorijum je pripremljen kao centralno mesto gde se:

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
