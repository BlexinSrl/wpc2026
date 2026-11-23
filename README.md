# Orders

Blazor Web App su .NET 10 con gestione degli sconti e controlli automatici sulle dipendenze tra progetti. La [specifica 002](specs/002-remove-discount/spec.md) descrive la rimozione di uno sconto.

## Avvio locale

```sh
dotnet run --project src/Orders.Web --launch-profile http
```

L'applicazione è disponibile su http://localhost:5078.

## Verifica

Il repository è pubblicato su [github.com/BlexinSrl/wpc2026](https://github.com/BlexinSrl/wpc2026):

```sh
git clone https://github.com/BlexinSrl/wpc2026.git
cd wpc2026
```

Dal clone, con Git, Bash, SDK .NET 10, PowerShell 7.4 o successivo, Node 22 e npm:

```sh
pwsh -NoProfile -File scripts/demo.ps1 -Step Prepare
pwsh -NoProfile -File eng/verify.ps1
```

La verifica genera il report `artifacts/verification.json`. Il [workflow CI](.github/workflows/verify.yml) esegue lo stesso script sulle pull request. La [guida agli esercizi](docs/DEMO.md) descrive come riprodurre il controllo architetturale ARCH001.
