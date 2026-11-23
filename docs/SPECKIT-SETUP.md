# Spec Kit

Il repository include gli asset di [Spec Kit](https://github.com/github/spec-kit)
1.4.2 per Claude Code con script PowerShell: skill (`.claude/skills/speckit-*`),
template, script e constitution sono tracciati, quindi un clone basta per usarli.
Non servono Python, `uv` o la CLI `specify`.

Spec Kit organizza il lavoro; il contratto `eng/verify.ps1` lo verifica.

## Uso

Flusso: constitution → specification → plan → tasks → implementation → verifica.

In Claude Code: `/speckit-constitution`, `/speckit-specify`, `/speckit-plan`,
`/speckit-tasks`, `/speckit-implement`, `/speckit-converge`.

Per lavorare su una feature esistente senza creare branch, selezionarla in modo esplicito:

```powershell
$env:SPECIFY_FEATURE_DIRECTORY = Join-Path $PWD 'specs/002-remove-discount'
./.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireSpec -RequireTasks
./.specify/scripts/powershell/setup-plan.ps1 -Json
./.specify/scripts/powershell/setup-tasks.ps1 -Json
```

Gli script preservano il piano esistente. `pwsh -NoProfile -File scripts/demo.ps1 -Step Spec`
esegue il controllo dei prerequisiti e mostra spec, plan e tasks della feature 002.

## Aggiornare gli asset

Rigenerare gli asset con la CLI `specify` in una cartella separata, confrontare i
file prima di copiarli e preservare la constitution. `.specify/scripts/powershell/common.ps1`
è adattato: il parser dei preset YAML usa `ConvertFrom-Yaml` (modulo `powershell-yaml`,
necessario solo per aggiungere preset YAML) invece di un processo Python.
