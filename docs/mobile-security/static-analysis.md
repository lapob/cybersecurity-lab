# Analisi statica Mobile

## Cos'è

L'analisi statica consiste nello studiare un'app senza eseguirla, osservando file, configurazioni, Manifest, risorse e codice decompilato.

## Obiettivi

- Capire come è fatta l'app
- Trovare configurazioni rischiose
- Individuare permessi eccessivi
- Cercare endpoint API
- Cercare chiavi hardcoded
- Controllare componenti esportati
- Analizzare WebView e deep link

## Tool

- JADX
- APKTool
- MobSF
- unzip
- strings
- grep

## Workflow base

```bash
file app.apk
unzip -l app.apk
jadx-gui app.apk
apktool d app.apk -o output_apktool
strings app.apk | grep -i "http"
```

## Cose da cercare

- URL ed endpoint
- token o chiavi hardcoded
- file di configurazione
- log sensibili
- permessi rischiosi
- `android:exported="true"`
- `android:debuggable="true"`
- `android:allowBackup="true"`

## Documentazione

Per ogni finding annotare:

- dove si trova
- perché è importante
- impatto potenziale
- evidenza
- remediation
