# Quickstart: Rimuovere lo sconto

Dalla cartella `Orders`.

## Test mirati

```sh
dotnet test tests/Orders.UnitTests --filter FullyQualifiedName~RemoveDiscount
dotnet test tests/Orders.BlazorTests --filter FullyQualifiedName~HomeTests
```

## Contratto completo

```sh
pwsh -NoProfile -File eng/verify.ps1
```

Leggere `artifacts/verification.json`: tutti i gate devono essere `passed`.

## Prova manuale

```sh
dotnet run --project src/Orders.Web --launch-profile http
```

1. Aprire <http://localhost:5078>: totale 100 euro.
2. Applicare SAVE20 al 20%: totale 80 euro.
3. Premere «Rimuovi sconto»: totale 100 euro, percentuale 0%, messaggio «Sconto rimosso.».
