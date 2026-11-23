# Implementation Plan: Rimuovere lo sconto

**Branch**: `002-remove-discount` | **Date**: 2026-10-10 | **Spec**: [spec.md](spec.md)

## Summary

Aggiungere a Order un'operazione idempotente che azzera percentuale e codice, un handler Application che la invoca e salva tramite IOrderRepository, e un pulsante nella pagina Home. Riutilizzare repository, messaggi e stile esistenti.

## Technical Context

**Language/Version**: C# / .NET 10

**Primary Dependencies**: Blazor Web App (Interactive Server)

**Storage**: InMemoryOrderRepository

**Testing**: xUnit (Domain, Application, architettura), bUnit (componenti), Playwright (browser)

**Target Platform**: browser desktop

**Project Type**: web app a livelli (Domain, Application, Infrastructure, Web)

**Constraints**: nessuna nuova dipendenza NuGet; il contratto `eng/verify.ps1` non cambia

**Scale/Scope**: un handler, un metodo di dominio, un pulsante

## Constitution Check

| Principio | Come viene rispettato | Verifica |
| --- | --- | --- |
| Domain first | RemoveDiscount vive in Order; l'handler dipende solo da IOrderRepository | ARCH001, ARCH002 |
| Comportamento verificabile | ogni FR ha un test; percentuale e codice restano coerenti | unit test, bUnit |
| Un contratto di verifica | stesso `eng/verify.ps1` in locale e in CI | GitHub Actions |
| Separazione delle responsabilità | nessuna modifica a gate, regole o workflow | review |
| Complessità proporzionata | un handler sul modello di ApplyDiscountHandler, nessuna astrazione nuova | review |

Esito: nessuna violazione, Complexity Tracking non necessario.

## Project Structure

### Documentation (this feature)

```text
specs/002-remove-discount/
├── spec.md
├── plan.md
├── data-model.md
├── quickstart.md
├── checklists/requirements.md
└── tasks.md
```

### Source Code (repository root)

```text
src/Orders.Domain/Order.cs                        + RemoveDiscount()
src/Orders.Application/RemoveDiscountHandler.cs   nuovo
src/Orders.Web/Program.cs                         + AddScoped<RemoveDiscountHandler>()
src/Orders.Web/Components/Pages/Home.razor        + pulsante, RemoveAsync
tests/Orders.UnitTests/RemoveDiscountTests.cs     nuovo
tests/Orders.BlazorTests/HomeTests.cs             + rimozione e messaggio
tests/browser/discount.spec.js                    + percorso 100 → 80 → 100
```

**Structure Decision**: la feature segue la struttura esistente; nessun nuovo progetto.

## Complexity Tracking

Nessuna voce.
