# AndroidManifest.xml

## Cos'è

`AndroidManifest.xml` è uno dei file più importanti di un'app Android. Descrive identità, componenti, permessi e configurazioni dell'app.

## Informazioni importanti

Nel Manifest si trovano:

- package name
- permessi richiesti
- Activity
- Service
- Broadcast Receiver
- Content Provider
- intent-filter
- deep link
- configurazioni di backup
- impostazioni di debug
- componenti esportati

## Elementi da controllare in laboratorio

### Permessi

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Permessi da osservare con attenzione:

- accesso a contatti
- posizione
- fotocamera
- microfono
- storage
- SMS
- internet

### Componenti esportati

```xml
android:exported="true"
```

Un componente esportato può essere raggiungibile da altre app. Va verificato solo in laboratorio e con autorizzazione.

### Debuggable

```xml
android:debuggable="true"
```

In produzione dovrebbe essere `false`.

### Backup

```xml
android:allowBackup="true"
```

Può essere rilevante se l'app gestisce dati sensibili.

### Deep link

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="myapp" />
</intent-filter>
```

## Tool utili

```bash
jadx-gui app.apk
apktool d app.apk -o output
adb shell dumpsys package com.nome.pacchetto
```

## Checklist Manifest

- [ ] Ho identificato il package name.
- [ ] Ho letto i permessi.
- [ ] Ho controllato componenti esportati.
- [ ] Ho controllato deep link.
- [ ] Ho controllato debug e backup.
- [ ] Ho annotato rischi e possibili remediation.
