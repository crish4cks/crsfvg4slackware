# crsfvg4slackware
Il software in questo repository è stato testato sul lettore di smart card [miniLector 
EVO](https://www.bit4id.com/en/smart-card-reader-minilector-evo/).

**NOTA:** Il vendor ID e il Device ID posso essere ricavati facilmente tramite il comando **lsusb** (lanciato con privilegi di root):
```
# lsusb
Bus 001 Device 007: ID 25dd:3111
```
## Configurazione e installazione del software
Per prima cosa è necessario creare un utente **pcscd** e un gruppo **pcscd** tramite i seguenti comandi:
```
# groupadd -g 257 pcscd
# useradd -u 257 -g pcscd -d /var/run/pcscd -s /bin/false pcscd
```

Successivamente vanno installati i pacchetti pcsc-lite e ccid, reperibili su SlackBuilds.org:
- [pcsc-lite](http://slackbuilds.org/repository/14.2/system/pcsc-lite/)
- [ccid](http://slackbuilds.org/repository/14.2/system/ccid/)

A questo punto è possibile installare **libbit4xpki** utilizzando l'apposito SlackBuild script, oppure semplicemente installando il 
pacchetto precompilato per la propria architettura (entrambi sono presenti in questo repository).

Una volta fatto questo, avviare il demone pcscd con il comando:
```
# /etc/rc.d/rc.pcscd start
```

È possibile renderlo avviabile automaticamente aggiungendo le seguenti righe
all'interno del file /etc/rc.d/rc.local:
```
if [ -x /etc/rc.d/rc.pcscd ]; then
  /etc/rc.d/rc.pcscd start
fi
```

Il pacchetto **libbit4xpki**, oltre alle librerie necessarie, installa anche un software con interfaccia grafica chiamato **Pin Manager**, 
il quale dovrebbe servire per sbloccare e/o cambiare il Pin della carta [NON TESTATO!].

## Configurazione del browser (Firefox)
1. Collegare il lettore ad una porta USB
2. Aprire la pagina delle preferenze al percorso **Preferences->Advanced->Certificates**, selezionare **Select one automatically** e 
cliccare su **Security Devices**
3. Posizionare il cursore sulla voce **NSS Internal PKCS #11 Module** e cliccare su **Load**
4. Nel campo **Module Name** inserire *bit4id*, poi cliccare su **Browse** e selezionare il file /usr/lib/bit4id/libbit4xpki.so
5. Cliccare su **OK**.
