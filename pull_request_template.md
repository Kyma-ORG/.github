<!--
Regola branch (docs/processi/lavoro.md §1): massimo DUE branch vivi per repo —
`main` e il branch DELLA ISSUE `issue-<NN>-<slug>`, con lo STESSO nome in ogni repo coinvolto.
Chiudere la PR chiude la issue. Niente push diretto su `main`.
-->

## Cosa cambia

<!-- Il deliverable, in due righe. Cosa esiste dopo il merge che prima non esisteva. -->

## Perché

<!-- Il problema che risolve / cosa sblocca. -->

## Issue

Refs #<!-- NN -->  ·  Chiude #<!-- NN -->

## Repo coinvolti nella issue

<!-- Elenca i repo su cui la issue ha un branch issue-<NN>-<slug>; spunta quando TUTTE le PR sono pronte. -->
- [ ] ogni repo coinvolto ha il suo branch `issue-<NN>-<slug>` e la sua PR

## Exit criteria soddisfatti

<!-- Copia gli exit criteria dalla issue e spunta. Se non sono verificabili, non sono exit criteria. -->
- [ ] …

## Controllo

- [ ] `main` non ha ricevuto push diretti (si arriva a `main` solo per merge)
- [ ] nessun branch oltre `main` e `issue-<NN>-<slug>` resta vivo
- [ ] il branch si cancella al merge (`--delete-branch`), PR => issue => `Done`
- [ ] serving un ok umano (C1 uscita esterna / C2 irreversibile): l'ok di `Camuy` è citato qui sotto

<!-- C1/C2: incolla il riferimento all'ok di Camuy (commento sulla issue). C3: scrivi "C3 — nessun gate". -->
C1/C2: <!-- ok di @Camuy: <link al commento> | "C3 — nessun gate" -->
