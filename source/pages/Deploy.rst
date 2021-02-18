
=============================================
DEPLOY
=============================================
Questa sezione mostrerà come svolgere una procedura di deploy completa.

----------------------------------------------
Preparazione Lattepanda
----------------------------------------------

Procurarsi:
  * USB con l'immagine di octopus
  * USB con una distribuzione di linux live
  * Una tastiera
  * Un alimentatore
  * Cavo HDMI
  * Cavo di rete

A questo punto colleghiamo il tutto (nell'uscita USB 2.0 è preferibile collegarci l'USB con l'immagine di octopus) e durante l'avvio accediamo al bios.(Ctrl + Alt + Canc ) 

Recarsi nella sezione Boot e portare il valore di "Machine Status AC/Battery In" su Power On in modo da far avviare il Lattepanda in autonomia dopo averlo collegato alla corrente.
Prima di salvare e chiudere controllare che la boot Option #1 sia la nostra USB poi procedere.

Avviamo preferibilmente l'immagine nella versione solo terminale per velocizzare il processo selezionando la versione x.

---------------------------------------------------
Montaggio dell'immagine di Octopus sul Lattepanda
---------------------------------------------------

Scollegare e ricollegare la USB con l'immagine di octopus e digitare il comando 'dmesg'.
Il risultato che occorre a noi sta nell'ultima riga e solitamente è sdb1 che rappresenta la nostra chiavetta con l'immagine octopus.
Procedere quindi con il comando:

``mount /dev/sdb1/  /mnt/``

Controllare che nella cartella mnt sia presente la cartella octopus.2020_12_07.img.gz.

``cd /mnt/``

``ls``

Fatto ciò digitiamo il comando:

``sudo gunzip -c /mnt/octopus.2020_12_07.img.gz | sudo dd of=/dev/mmcblk0 status=progres``

Attendiamo il completamento (Tempo stimato: 40 min).
Terminato il tutto arrestiamo la macchina ( c'è la possibilità che non si riesca ad arrestare nel modo più sicuro in quel caso agire forzatamente staccando l'alimentazione) 

---------------------------------------------------
Scheda di rete Octopus
---------------------------------------------------

Ora non avremo più bisogno delle USB con le immagini e se tutti i passaggi precedenti sono stati eseguiti con esattezza il Lattepanda si avvierà con l'immagine di Octopus installata.
Il primo passaggio è sistemare la scheda di rete quindi collegare l'uscita ethernet al nostro pc e seguire questi comandi:
L'immagine di octopus ha una scheda di rete che non corrisponde a quella del Lattepanda quindi bisogna sostituire con quella attuale mostrata attraverso il comando:

``ip a``

``mv ifcfg-(old) ifcfg-(new)``

Al suo interno modificare tutti i valori della vecchia scheda di rete con quella nuova che visualiziamo nel titolo del nano

``nano ifcfg-(new)``
  
Il comando seguente potrebbe essere necessario eseguirlo due volte:

``ifdown enx00... && ifup enx00...``

``reboot``

---------------------------------------------------
Deploy della versione aggiornata
---------------------------------------------------

Dopo il reboot dobbiamo usare il nostro pc per deployare la versione aggiornata di octopus.
Per prima cosa dobbiamo andare in Impostazioni -> Network -> Impostazioni -> IPv4
Da qui dobbiamo cambiare il metodo  in manuale e inserire un IP appartenente alla stessa sottorete dell'octopus che di default ha l'IP 172.16.0.199.
Sucessivamente bisogna accettare la fingerprint del Lattepanda nei known hosts del nostro pc attraverso questo comando:

``ssh-keyscan -H (IP del Lattepanda) >> ~/.ssh/known_hosts``
 
Recarsi quindi nella directory master del nostro progetto e eseguire il comando:

``./deploy_x86.sh CSW0ct0pus1557 (IP della scheda di rete del Lattepanda)  22(porta)``

A questo punto eseguiamo il reboot del Lattepanda.

---------------------------------------------------
Migrate e Seed
---------------------------------------------------

Il primo passo è recarsi nella directory esatta:

``cd /usr/local/crossover_octopus_core/webserver/www``

Da qui possiamo eseguire i comandi per i migrate e i seed:

``bin/cake deploy migrate --createdb --coreplugins``

``bin/cake deploy seed --coreplugins``

In questo caso ho fatto il migrate e il seed solo dei plugins necessari al funzionamento del nostro sistema nel caso volessimo aggiungere dei pugins al di fuori di quelli core possiamo aggiungere in fondo al comando - -add (nomedelplugin) , (nomedelplugin)... esempio:

``bin/cake deploy migrate --createdb --add PipelineIndustry``

``bin/cake deploy seed --add PipelineIndustry``

