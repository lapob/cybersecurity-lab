# Cybersecurity Hacking Lab

Knowledge base personale per studiare cybersecurity, ethical hacking, Android security, mobile pentesting, Kali Linux, networking, web security, reverse engineering e reporting professionale.

> Uso previsto: studio, laboratorio personale, CTF, macchine virtuali, emulatori e ambienti autorizzati. Non usare queste note su sistemi, account, reti o dispositivi senza permesso.

## Obiettivo del repository

Questo repository serve a costruire un percorso ordinato da principiante a junior ethical hacker, con:

- teoria spiegata semplice,
- cheatsheet di comandi,
- laboratori pratici autorizzati,
- checklist operative,
- template per report,
- roadmap di studio,
- appunti utili per portfolio GitHub.

## Struttura

```text
cybersecurity-hacking-lab/
├── README.md
├── ETHICS.md
├── docs/
│   ├── adb-comandi.md
│   ├── ethical-hacking/
│   ├── android/
│   ├── kali/
│   ├── mobile-security/
│   ├── networking/
│   ├── reverse-engineering/
│   └── web-security/
├── cheatsheets/
│   ├── android-adb.md
│   └── linux-essentials.md
├── checklists/
│   ├── lab-safety-checklist.md
│   └── junior-ethical-hacker-checklist.md
├── labs/
│   └── android/
│       └── adb-first-lab.md
├── roadmap/
│   ├── completed.md
│   ├── studying.md
│   ├── future.md
│   └── ethical-hacker-path.md
└── templates/
    ├── lab-template.md
    ├── notes-template.md
    └── report-template.md
```

## Etica e regole

Prima di studiare o testare qualsiasi cosa leggere:

- [Codice etico del laboratorio](ETHICS.md)
- [Checklist sicurezza laboratorio](checklists/lab-safety-checklist.md)

Regola principale: testare solo ciò che è mio o ciò per cui ho autorizzazione esplicita.

## Percorso Ethical Hacker

- [Indice Ethical Hacking](docs/ethical-hacking/README.md)
- [Fasi di un test etico](docs/ethical-hacking/phases.md)
- [Percorso completo Ethical Hacker](roadmap/ethical-hacker-path.md)
- [Checklist Junior Ethical Hacker](checklists/junior-ethical-hacker-checklist.md)

## Android Security

- [ADB — guida completa](docs/adb-comandi.md)
- [Indice Android Security](docs/android/README.md)
- [Android Manifest](docs/android/android-manifest.md)
- [Cheatsheet ADB](cheatsheets/android-adb.md)
- [Primo laboratorio ADB](labs/android/adb-first-lab.md)

Argomenti da continuare:

- Activity
- Service
- Broadcast Receiver
- Content Provider
- Intent
- Deep Link
- Logcat avanzato
- Permessi Android

## Mobile Application Security

- [Indice Mobile Security](docs/mobile-security/README.md)
- [Analisi statica Mobile](docs/mobile-security/static-analysis.md)

Argomenti da continuare:

- MobSF
- JADX
- APKTool
- Frida
- Objection
- Burp Suite
- OWASP MASVS
- OWASP MASTG
- Analisi dinamica
- WebView security
- Secure storage

## Kali Linux

- [Indice Kali Linux](docs/kali/README.md)
- [Cheatsheet Linux Essentials](cheatsheets/linux-essentials.md)

Argomenti da continuare:

- Bash
- permessi Linux
- processi
- servizi
- SSH
- cron
- gestione pacchetti

## Networking

- [Indice Networking](docs/networking/README.md)
- [TCP/IP basi](docs/networking/tcp-ip-basics.md)

Argomenti da continuare:

- DNS
- DHCP
- TCP e UDP
- porte
- NAT
- routing
- Wireshark
- nmap in laboratorio autorizzato

## Web Security

- [Indice Web Security](docs/web-security/README.md)
- [HTTP e HTTPS basi](docs/web-security/http-basics.md)

Argomenti da continuare:

- cookie
- sessioni
- autenticazione
- autorizzazione
- OWASP Top 10
- API security
- JWT

## Reverse Engineering

- [Indice Reverse Engineering](docs/reverse-engineering/README.md)

Argomenti da continuare:

- struttura APK
- JADX
- APKTool
- Smali base
- ricerca segreti hardcoded
- Ghidra base

## Roadmap

- [Argomenti completati](roadmap/completed.md)
- [Argomenti in studio](roadmap/studying.md)
- [Argomenti futuri](roadmap/future.md)
- [Percorso Ethical Hacker](roadmap/ethical-hacker-path.md)

## Template

- [Template appunti](templates/notes-template.md)
- [Template laboratorio](templates/lab-template.md)
- [Template report sicurezza](templates/report-template.md)

## Metodo di studio

Per ogni nuovo argomento:

1. Creo una guida teorica in `docs/`.
2. Creo una cheatsheet se ci sono comandi.
3. Creo un laboratorio se l'argomento è pratico.
4. Aggiorno la roadmap.
5. Aggiungo eventuali errori incontrati e soluzioni.
6. Se trovo un problema di sicurezza in laboratorio, lo documento con il template report.

## Stato attuale

Repository trasformato in una base di studio e portfolio per ethical hacking. La parte Android/ADB è già avviata; sono state aggiunte etica, checklist, reporting, networking, HTTP, Android Manifest e analisi statica mobile.