# Tasks: Rimuovere lo sconto

**Input**: [spec.md](spec.md), [plan.md](plan.md), [data-model.md](data-model.md)

Formato: `[ID] [P?] [Story] Descrizione`. `[P]` indica task eseguibili in parallelo.

## Phase 1: Foundational (Domain)

- [ ] T001 [US1] Test: RemoveDiscount azzera percentuale e codice in tests/Orders.UnitTests/RemoveDiscountTests.cs
- [ ] T002 [US1] Order.RemoveDiscount() in src/Orders.Domain/Order.cs: `Order.DiscountPercentage` a 0 e `Order.DiscountCode` a null secondo [data-model.md](data-model.md)
- [ ] T003 [P] [US2] Test: due rimozioni lasciano invariati `Order.Subtotal` (immutabile in [data-model.md](data-model.md)), totale, percentuale e codice

**Checkpoint**: il dominio supporta la rimozione idempotente.

## Phase 2: User Story 1 - Rimuovere uno sconto applicato (P1) 🎯 MVP

- [ ] T004 [US1] Test handler: lookup, null senza SaveAsync per ordine inesistente, SaveAsync chiamato una volta
- [ ] T005 [US1] RemoveDiscountHandler in src/Orders.Application/RemoveDiscountHandler.cs
- [ ] T006 [US1] Registrazione scoped in src/Orders.Web/Program.cs
- [ ] T007 [US1] Pulsante, RemoveAsync e messaggio «Sconto rimosso.» in src/Orders.Web/Components/Pages/Home.razor
- [ ] T008 [P] [US1] bUnit: rimozione aggiorna totale e messaggi in tests/Orders.BlazorTests/HomeTests.cs

**Checkpoint**: la rimozione funziona dalla UI.

## Phase 3: Polish & Verification

- [ ] T009 Browser: percorso 100 → 80 → 100 in tests/browser/discount.spec.js
- [ ] T010 Eseguire `pwsh -NoProfile -File eng/verify.ps1` e leggere artifacts/verification.json
- [ ] T011 Review del diff e CI sulla PR, senza merge automatico

## Dependencies

- T001 → T002 → T004 → T005 → T006 → T007 → T009 → T010 → T011
- T003 dipende da T002; T008 da T007.
- MVP: Phase 1 e Phase 2.

I task restano aperti nella baseline: l'implementazione è in `demo-assets/remove-discount.patch` e nel ciclo di review della PR dedicata.
