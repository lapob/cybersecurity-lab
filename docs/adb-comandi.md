# ADB — comandi essenziali per Android e cybersecurity lab

> Uso etico: questi comandi sono pensati per dispositivi di tua proprietà, emulatori o ambienti di laboratorio autorizzati. Non usarli su telefoni, account o reti senza permesso.

## 1. Che cos'è ADB

ADB, Android Debug Bridge, è uno strumento da riga di comando che permette di comunicare con un dispositivo Android collegato via USB o rete. Serve per installare app, leggere log, aprire una shell, copiare file, fare debug e analisi di sicurezza in laboratorio.

## 2. Prerequisiti

Sul telefono:

1. Attiva **Opzioni sviluppatore**.
2. Attiva **Debug USB**.
3. Collega il dispositivo via USB.
4. Accetta la finestra RSA sul telefono.

Sul computer:

```bash
adb version
adb devices
```

Se il dispositivo compare come `unauthorized`, sblocca il telefono e accetta la richiesta di debug USB.

## 3. Comandi base

```bash
adb version                         # mostra la versione di adb
adb help                            # mostra l'aiuto generale
adb devices                         # lista dispositivi collegati
adb devices -l                      # lista dettagliata dei dispositivi
adb start-server                    # avvia il server adb
adb kill-server                     # ferma il server adb
adb reconnect                       # riconnette il dispositivo
adb reconnect device                # riconnette il lato dispositivo
adb reconnect offline               # prova a recuperare device offline
```

Quando ci sono più dispositivi collegati:

```bash
adb -s SERIAL devices
adb -s SERIAL shell
adb -s SERIAL install app.apk
```

Per recuperare il seriale:

```bash
adb devices
```

## 4. Shell Android

Aprire una shell interattiva:

```bash
adb shell
```

Eseguire un singolo comando:

```bash
adb shell whoami
adb shell id
adb shell pwd
adb shell ls
adb shell ls -la
adb shell cd /sdcard
adb shell uname -a
adb shell getprop
adb shell getprop ro.build.version.release
adb shell getprop ro.product.model
adb shell getprop ro.product.manufacturer
```

Uscire dalla shell:

```bash
exit
```

## 5. Informazioni sul dispositivo

```bash
adb shell getprop ro.build.version.release     # versione Android
adb shell getprop ro.build.version.sdk         # API level
adb shell getprop ro.product.brand             # brand
adb shell getprop ro.product.model             # modello
adb shell getprop ro.product.cpu.abi           # architettura CPU
adb shell getprop ro.boot.verifiedbootstate    # stato verified boot
adb shell getprop ro.debuggable                # build debuggable o no
adb shell getprop ro.secure                    # impostazioni di sicurezza build
adb shell settings list system
adb shell settings list secure
adb shell settings list global
```

Batteria e stato hardware:

```bash
adb shell dumpsys battery
adb shell dumpsys battery | grep level
adb shell dumpsys power
adb shell dumpsys display
adb shell dumpsys sensorservice
```

## 6. File e cartelle

Navigazione:

```bash
adb shell pwd
adb shell ls /sdcard
adb shell ls -la /sdcard/Download
adb shell mkdir /sdcard/lab
adb shell rm /sdcard/lab/file.txt
adb shell rm -r /sdcard/lab/cartella
```

Copiare file da PC a telefono:

```bash
adb push file.txt /sdcard/Download/
adb push app.apk /sdcard/Download/
adb push cartella/ /sdcard/lab/
```

Copiare file da telefono a PC:

```bash
adb pull /sdcard/Download/file.txt .
adb pull /sdcard/DCIM ./DCIM_backup
adb pull /sdcard/Download ./Download_backup
```

Leggere file:

```bash
adb shell cat /sdcard/Download/file.txt
adb shell head /sdcard/Download/file.txt
adb shell tail /sdcard/Download/file.txt
```

Permessi e spazio:

```bash
adb shell df -h
adb shell du -h /sdcard/Download
adb shell ls -lh /sdcard/Download
```

## 7. Installazione e gestione APK

Installare un APK:

```bash
adb install app.apk
adb install -r app.apk                  # reinstalla mantenendo i dati
adb install -d app.apk                  # permette downgrade, se consentito
adb install -g app.apk                  # concede permessi runtime dichiarati
```

Installare APK split:

```bash
adb install-multiple base.apk split_config.arm64_v8a.apk split_config.it.apk
```

Disinstallare:

```bash
adb uninstall com.nome.pacchetto
adb uninstall -k com.nome.pacchetto     # rimuove app ma mantiene dati/cache
```

Elencare app:

```bash
adb shell pm list packages
adb shell pm list packages -3           # app utente
adb shell pm list packages -s           # app di sistema
adb shell pm list packages -f           # mostra percorso APK
adb shell pm list packages | grep nome
```

Trovare path APK:

```bash
adb shell pm path com.nome.pacchetto
```

Estrarre APK installato:

```bash
adb shell pm path com.nome.pacchetto
adb pull /data/app/....../base.apk ./base.apk
```

Dettagli pacchetto:

```bash
adb shell dumpsys package com.nome.pacchetto
adb shell dumpsys package com.nome.pacchetto | grep version
adb shell dumpsys package com.nome.pacchetto | grep permission
```

## 8. Avvio app, Activity, Service, Broadcast

Aprire app tramite launcher:

```bash
adb shell monkey -p com.nome.pacchetto -c android.intent.category.LAUNCHER 1
```

Avviare una Activity specifica:

```bash
adb shell am start -n com.nome.pacchetto/.MainActivity
adb shell am start -n com.nome.pacchetto/com.nome.pacchetto.MainActivity
```

Aprire URL:

```bash
adb shell am start -a android.intent.action.VIEW -d "https://example.com"
```

Inviare extra:

```bash
adb shell am start -n com.nome.pacchetto/.MainActivity --es key "valore"
adb shell am start -n com.nome.pacchetto/.MainActivity --ei numero 123
adb shell am start -n com.nome.pacchetto/.MainActivity --ez flag true
```

Avviare Service autorizzati in lab:

```bash
adb shell am startservice -n com.nome.pacchetto/.NomeService
```

Inviare broadcast in lab:

```bash
adb shell am broadcast -a com.nome.pacchetto.AZIONE_TEST
```

Fermare app:

```bash
adb shell am force-stop com.nome.pacchetto
```

## 9. Logcat

Leggere log in tempo reale:

```bash
adb logcat
```

Pulire buffer log:

```bash
adb logcat -c
```

Salvare log su file:

```bash
adb logcat > logcat.txt
adb logcat -d > logcat_dump.txt
```

Filtrare:

```bash
adb logcat | grep NomeApp
adb logcat *:E                         # solo errori
adb logcat *:W                         # warning e superiori
adb logcat ActivityManager:I *:S       # solo tag ActivityManager
adb logcat -s NomeTag
```

Buffer specifici:

```bash
adb logcat -b main
adb logcat -b system
adb logcat -b events
adb logcat -b crash
adb logcat -b all
```

Log utili per crash:

```bash
adb logcat -b crash -d
adb shell dumpsys dropbox --print
```

## 10. Processi e memoria

```bash
adb shell ps
adb shell ps -A
adb shell top
adb shell top -o PID,USER,CPU,RES,ARGS
adb shell pidof com.nome.pacchetto
adb shell kill PID
adb shell dumpsys meminfo
adb shell dumpsys meminfo com.nome.pacchetto
adb shell dumpsys cpuinfo
```

## 11. Rete

Informazioni rete:

```bash
adb shell ip addr
adb shell ip route
adb shell ifconfig
adb shell netstat
adb shell ss
adb shell dumpsys connectivity
adb shell dumpsys wifi
```

Test rete:

```bash
adb shell ping -c 4 8.8.8.8
adb shell ping -c 4 example.com
```

ADB via Wi-Fi, solo su rete fidata:

```bash
adb tcpip 5555
adb shell ip addr show wlan0
adb connect IP_DEL_TELEFONO:5555
adb devices
adb disconnect IP_DEL_TELEFONO:5555
adb usb
```

Port forwarding utile in laboratorio:

```bash
adb forward tcp:8080 tcp:8080
adb forward tcp:27042 tcp:27042
adb forward --list
adb forward --remove tcp:8080
adb forward --remove-all
```

Reverse forwarding:

```bash
adb reverse tcp:8080 tcp:8080
adb reverse --list
adb reverse --remove tcp:8080
adb reverse --remove-all
```

## 12. Screenshot e registrazione schermo

Screenshot:

```bash
adb shell screencap -p /sdcard/screen.png
adb pull /sdcard/screen.png .
```

Screenshot diretto su PC:

```bash
adb exec-out screencap -p > screen.png
```

Registrazione schermo:

```bash
adb shell screenrecord /sdcard/video.mp4
adb pull /sdcard/video.mp4 .
```

Con limite tempo:

```bash
adb shell screenrecord --time-limit 30 /sdcard/video.mp4
```

## 13. Input: tap, swipe, testo, tasti

Tap:

```bash
adb shell input tap 500 1000
```

Swipe:

```bash
adb shell input swipe 500 1500 500 500
adb shell input swipe 500 1500 500 500 300
```

Scrivere testo:

```bash
adb shell input text "ciao"
adb shell input text "ciao%smondo"      # %s = spazio
```

Tasti:

```bash
adb shell input keyevent KEYCODE_HOME
adb shell input keyevent KEYCODE_BACK
adb shell input keyevent KEYCODE_APP_SWITCH
adb shell input keyevent KEYCODE_POWER
adb shell input keyevent KEYCODE_ENTER
adb shell input keyevent KEYCODE_DEL
adb shell input keyevent KEYCODE_VOLUME_UP
adb shell input keyevent KEYCODE_VOLUME_DOWN
```

Codici numerici frequenti:

```bash
adb shell input keyevent 3      # Home
adb shell input keyevent 4      # Back
adb shell input keyevent 26     # Power
adb shell input keyevent 66     # Enter
adb shell input keyevent 67     # Delete
```

## 14. Permessi Android

Elencare permessi di un'app:

```bash
adb shell dumpsys package com.nome.pacchetto | grep permission
```

Concedere permesso runtime, se l'app lo dichiara:

```bash
adb shell pm grant com.nome.pacchetto android.permission.CAMERA
adb shell pm grant com.nome.pacchetto android.permission.ACCESS_FINE_LOCATION
adb shell pm grant com.nome.pacchetto android.permission.READ_CONTACTS
```

Revocare permesso:

```bash
adb shell pm revoke com.nome.pacchetto android.permission.CAMERA
```

Resettare permessi:

```bash
adb shell pm reset-permissions
```

Controllare AppOps:

```bash
adb shell appops get com.nome.pacchetto
adb shell appops set com.nome.pacchetto CAMERA allow
adb shell appops set com.nome.pacchetto CAMERA deny
```

## 15. Dati app, cache e stato

Pulire dati app:

```bash
adb shell pm clear com.nome.pacchetto
```

Fermare app:

```bash
adb shell am force-stop com.nome.pacchetto
```

Aprire impostazioni app:

```bash
adb shell am start -a android.settings.APPLICATION_DETAILS_SETTINGS -d package:com.nome.pacchetto
```

Percorsi utili:

```bash
/data/data/com.nome.pacchetto/          # dati privati app, accesso limitato
/sdcard/Android/data/com.nome.pacchetto/
/sdcard/Android/media/com.nome.pacchetto/
/sdcard/Download/
```

Nota: su dispositivi non rootati, `/data/data/...` non è leggibile con ADB normale. Usa solo dispositivi di lab, emulatori o build debuggable autorizzate.

## 16. Backup e restore

Su versioni Android recenti alcuni metodi sono limitati o deprecati.

```bash
adb backup -apk -shared -all -f backup.ab
adb restore backup.ab
```

Per lab moderni è spesso più affidabile usare emulatori, snapshot o esportare manualmente i dati accessibili.

## 17. Activity, intent e superfici di test

Trovare Activity dichiarate:

```bash
adb shell dumpsys package com.nome.pacchetto | grep -i activity
```

Trovare intent-filter:

```bash
adb shell dumpsys package com.nome.pacchetto | grep -i "intent"
```

Testare deep link autorizzati:

```bash
adb shell am start -a android.intent.action.VIEW -d "myapp://percorso/test"
adb shell am start -a android.intent.action.VIEW -d "https://dominio-lab.example/path"
```

Esempio con extra:

```bash
adb shell am start -n com.nome.pacchetto/.MainActivity --es username "test" --ez debug true
```

## 18. Content provider in laboratorio

Elencare info dal package dump:

```bash
adb shell dumpsys package com.nome.pacchetto | grep -i provider
```

Query su provider esportati e autorizzati:

```bash
adb shell content query --uri content://authority/path
adb shell content query --uri content://authority/path --projection colonna1:colonna2
```

Insert/update/delete solo su app di test o ambienti autorizzati:

```bash
adb shell content insert --uri content://authority/path --bind name:s:"test"
adb shell content update --uri content://authority/path --bind name:s:"nuovo" --where "id=1"
adb shell content delete --uri content://authority/path --where "id=1"
```

## 19. Emulatori

Lista emulatori/dispositivi:

```bash
adb devices
```

Connettersi a emulatori locali:

```bash
adb -s emulator-5554 shell
adb -s emulator-5556 shell
```

Installare su un emulatore specifico:

```bash
adb -s emulator-5554 install app.apk
```

Riavviare:

```bash
adb reboot
adb reboot bootloader
adb reboot recovery
```

## 20. Root, remount e lab avanzato

Solo su emulatori o dispositivi di test autorizzati:

```bash
adb root
adb unroot
adb remount
adb disable-verity
adb enable-verity
adb reboot
```

Verificare utente:

```bash
adb shell whoami
adb shell id
```

## 21. Bug report e diagnostica

```bash
adb bugreport bugreport.zip
adb shell dumpsys
adb shell dumpsys activity
adb shell dumpsys window
adb shell dumpsys package
adb shell dumpsys usagestats
adb shell dumpsys notification
```

Output mirato:

```bash
adb shell dumpsys activity activities
adb shell dumpsys window windows
adb shell dumpsys package com.nome.pacchetto
```

## 22. Comandi utili per mobile security lab

Identificare app installate:

```bash
adb shell pm list packages -3
```

Trovare APK e salvarlo:

```bash
adb shell pm path com.nome.pacchetto
adb pull /data/app/.../base.apk ./base.apk
```

Osservare log durante l'uso dell'app:

```bash
adb logcat -c
adb logcat | grep -i "com.nome.pacchetto"
```

Controllare permessi e componenti:

```bash
adb shell dumpsys package com.nome.pacchetto
```

Aprire Activity o deep link di test:

```bash
adb shell am start -n com.nome.pacchetto/.MainActivity
adb shell am start -a android.intent.action.VIEW -d "myapp://test"
```

Pulire stato app tra un test e l'altro:

```bash
adb shell pm clear com.nome.pacchetto
adb shell am force-stop com.nome.pacchetto
```

Usare proxy locale in laboratorio:

```bash
adb reverse tcp:8080 tcp:8080
adb forward tcp:8080 tcp:8080
```

## 23. Troubleshooting

Dispositivo non visto:

```bash
adb kill-server
adb start-server
adb devices
```

Dispositivo `unauthorized`:

1. Sblocca il telefono.
2. Revoca autorizzazioni debug USB dalle opzioni sviluppatore.
3. Ricollega il cavo.
4. Accetta la chiave RSA.

```bash
adb kill-server
adb start-server
adb devices
```

Dispositivo `offline`:

```bash
adb reconnect offline
adb kill-server
adb start-server
```

Installazione fallisce:

```bash
adb install -r app.apk
adb install -d app.apk
adb uninstall com.nome.pacchetto
adb install app.apk
```

Controllare ABI se APK incompatibile:

```bash
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abilist
```

## 24. Mini checklist pratica

Quando analizzi un'app Android in laboratorio:

1. Collega dispositivo/emulatore.
2. Verifica `adb devices`.
3. Installa APK con `adb install`.
4. Trova package con `adb shell pm list packages -3`.
5. Salva informazioni con `adb shell dumpsys package PACKAGE`.
6. Leggi log con `adb logcat`.
7. Testa Activity/deep link con `adb shell am start`.
8. Controlla permessi con `pm grant`, `pm revoke`, `appops`.
9. Pulisci stato con `pm clear`.
10. Documenta comandi, output e risultati.

## 25. Comandi rapidi da ricordare

```bash
adb devices
adb shell
adb install app.apk
adb uninstall com.nome.pacchetto
adb shell pm list packages -3
adb shell pm path com.nome.pacchetto
adb pull /percorso/remoto ./locale
adb push ./locale /percorso/remoto
adb logcat
adb logcat -c
adb shell dumpsys package com.nome.pacchetto
adb shell am start -n com.nome.pacchetto/.MainActivity
adb shell am force-stop com.nome.pacchetto
adb shell pm clear com.nome.pacchetto
adb exec-out screencap -p > screen.png
adb shell input tap X Y
adb reverse tcp:8080 tcp:8080
adb forward tcp:8080 tcp:8080
```
