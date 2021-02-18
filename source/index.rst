Benvenuto nella documentazione di octopus!
=============================================
Octopus è molto facile da usare. Una volta installato potrai configurare ogni aspetto tramite un semplice e veloce pannello web. Ci sono solo 3 passaggi da seguire:

   * Attivazione dei connettori software (interfaccia all'ERP e al PLC)
   * Creazione di modelli di dati
   * Configurazione di flussi automatizzati tra modelli di dati

Ogni connettore trasforma i campi del database di gestione o le aree di memoria del PLC in “variabili”

Il modello dati è una rappresentazione digitale di un sistema fisico, come un nastro trasportatore, che ha gli attributi di “velocità di riferimento” e “consumo di energia”. Il modello dati rappresenta anche la quantità di dati nel database di gestione. È quindi necessario creare almeno 2 modelli di dati.

Nel modello dati ogni attributo è poi connesso ad una variabile tra quelle generate dai connettori.

Infine, il configuratore di flusso permette di creare una copia automatizzata dei dati da un modello dati ad un altro, ad esempio provocando l'invio del set-point di velocità dal gestionale alla macchina oppure l'invio delle letture dei consumi al gestionale.

In 3 semplici passaggi è possibile creare qualsiasi flusso di informazioni tra diversi sistemi anche se non sono nativamente in grado di comunicare tra loro. 


.. toctree::
   :maxdepth: 2
   :caption: Contents:

   pages/Deploy
   pages/Drivers
   pages/Actions


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
