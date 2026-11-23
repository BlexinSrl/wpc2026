# Feature Specification: Rimuovere lo sconto

**Feature Branch**: `002-remove-discount`

**Created**: 2026-10-10

**Status**: Draft

**Input**: User description: "Come utente voglio rimuovere lo sconto da un ordine per ripristinare il totale originale senza ricaricare la pagina."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Rimuovere uno sconto applicato (Priority: P1)

L'utente ha applicato SAVE20 a un ordine e vuole tornare al prezzo pieno senza ricaricare la pagina.

**Why this priority**: è il valore della feature. Senza questa storia non c'è nulla da consegnare.

**Independent Test**: su un ordine da 100 euro con SAVE20 al 20%, premere «Rimuovi sconto» e verificare che il totale torni a 100 euro.

**Acceptance Scenarios**:

1. **Given** un ordine da 100 euro con SAVE20 al 20% (totale 80 euro), **When** l'utente preme «Rimuovi sconto», **Then** il totale è 100 euro, la percentuale è 0% e il codice non è più mostrato.
2. **Given** lo stesso ordine, **When** la rimozione va a buon fine, **Then** compare «Sconto rimosso.» e i messaggi di errore o conferma precedenti spariscono.
3. **Given** lo sconto appena rimosso, **When** l'ordine viene riletto dal repository nella stessa sessione, **Then** resta senza sconto perché la modifica è stata salvata. Un ricaricamento della pagina apre un nuovo circuito e riparte dall'ordine iniziale.

---

### User Story 2 - Rimozione ripetuta senza effetti (Priority: P2)

Una seconda rimozione, per esempio dovuta a un doppio clic o a una richiesta ripetuta, non deve cambiare l'ordine.

**Why this priority**: protegge da doppi clic e da richieste ripetute.

**Independent Test**: eseguire il comando di rimozione due volte sullo stesso ordine.

**Acceptance Scenarios**:

1. **Given** un ordine senza sconto, **When** si esegue di nuovo la rimozione, **Then** subtotale, totale, percentuale e codice non cambiano e non viene sollevato alcun errore. Il comando è idempotente.

### Edge Cases

- Ordine inesistente: il comando restituisce null e non salva nulla; la UI mostra «Ordine non disponibile.».
- Rimozione dopo un errore di validazione, per esempio uno sconto del 35%: l'errore precedente viene cancellato.
- Sconto applicato, rimosso e riapplicato: il totale torna a 80 euro.
- Arrotondamento: senza sconto il totale coincide con il subtotale, senza differenze di centesimi.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Order MUST esporre un'operazione che porta DiscountPercentage a 0 e DiscountCode a null.
- **FR-002**: L'operazione MUST essere idempotente e MUST NOT modificare Subtotal.
- **FR-003**: Application MUST offrire un comando che carica l'ordine tramite IOrderRepository, lo modifica e lo salva con SaveAsync.
- **FR-004**: Se l'ordine non esiste, il comando MUST restituire null senza chiamare SaveAsync.
- **FR-005**: La UI MUST invocare solo Application, mai Infrastructure.
- **FR-006**: La UI MUST mostrare «Sconto rimosso.» e cancellare errori e conferme precedenti.
- **FR-007**: La modifica MUST NOT aggiungere dipendenze NuGet né modificare gate o regole architetturali.

### Key Entities

- **Order**: ordine con subtotale fisso. DiscountPercentage (tra 0 e 30) e DiscountCode cambiano solo tramite applicazione o rimozione dello sconto; il totale è calcolato.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Nel browser il percorso 100 euro → 80 euro → 100 euro si completa senza ricaricare la pagina.
- **SC-002**: `pwsh -NoProfile -File eng/verify.ps1` termina con tutti i gate `passed`.
- **SC-003**: Ogni requisito da FR-001 a FR-006 è coperto da almeno un test Domain, Application, bUnit o browser.
- **SC-004**: ARCH001 e ARCH002 restano verdi.

## Assumptions

- Un ordine ha al più uno sconto alla volta; il cumulo degli sconti è fuori scope.
- La persistenza è in memoria: nessun database e nessuno storico sconti.
- Non ci sono autorizzazioni: chi vede l'ordine può rimuovere lo sconto.
- Il pulsante si trova nella card «Applica uno sconto», accanto al form esistente.
- La versione iniziale permette solo di applicare lo sconto; la rimozione è oggetto di questa modifica.
