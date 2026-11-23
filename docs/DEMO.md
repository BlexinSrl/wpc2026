# Esercizi di verifica

Eseguire i comandi dalla radice del repository Orders, dove si trova `Orders.slnx`. Per provare una violazione architetturale utilizzare un checkout dedicato con un commit iniziale. Il comando `Reset` scarta anche le modifiche locali in `src/Orders.Application`, inclusi i file non tracciati.

```sh
git worktree add -b esercizio/arch001 ../orders-arch001 HEAD
cd ../orders-arch001
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Prepare
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Verifica
```

## Avviare l'applicazione

L'app è disponibile su <http://localhost:5078>. Ctrl+C la arresta.

```sh
pwsh -NoProfile -File scripts/demo.ps1 -Step App
```

## Riprodurre ARCH001

```sh
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Senza
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Sensore
```

Il primo comando introduce un riferimento da Application a Infrastructure e una classe che usa direttamente il repository in memoria. Il codice può compilare, ma il vincolo sulle dipendenze viene violato. Il secondo comando esegue la verifica e controlla che il fallimento sia quello previsto. In `artifacts/verification.json` il controllo `architecture-tests` deve riportare `failed` con il codice ARCH001.

Per leggere il report, ripristinare l'esercizio e verificare di nuovo:

```sh
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Feedback
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Reset
pwsh -NoProfile -File scripts/esercizi.ps1 -Step Verifica
```

## Altri comandi

| Valore di `-Step` | Funzione |
| --- | --- |
| `Status` | Indica branch, commit, modifiche locali e stato della patch. |
| `Guida` | Individua le regole sulle dipendenze nelle istruzioni del repository. |
| `Guardrail` | Invia all'hook una richiesta di modifica del test architetturale e verifica che la decisione sia `deny`. |
| `Requisito` | Collega l'idempotenza richiesta dalla specifica 002 ai test della patch di esempio. |
| `Persone` | Individua il comando di CI e riepiloga i passaggi della review. Se GitHub CLI è disponibile, elenca le PR aperte. |

I [report di esempio](../demo-assets/evidence/) illustrano una verifica riuscita e una violazione ARCH001. Per interpretare un'esecuzione corrente consultare il [contratto di verifica](VERIFICATION-CONTRACT.md).
