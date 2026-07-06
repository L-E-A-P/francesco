---
title: interfantasia
layout: page
permalink: /interfantasia/
image:
  path: /images/interfantasia/hero.png
  caption: "Schema del sistema (singolo canale): mic → taratura → analisi → soglie → bicomb → LAR → altoparlanti."
---

*Brano per insieme di musicisti e ambiente in tempo reale, in feedback.*

Abitare un luogo vuol dire prendersene cura, modificarlo, cambiargli forma nel tempo, inserendo e spostando oggetti al suo interno. E cosa è un oggetto, acusticamente? Un filtro. Abitare acusticamente è cambiargli timbro.

Il titolo unisce *inter-* (in mezzo, fra) e *fantasia* (immaginazione): l'opera è l'intreccio delle immaginazioni delle parti coinvolte. In *interfantasia* l'elettronica è pensata come luogo da abitare. I musicisti e l'ambiente fisico in cui sono immersi lo modificano nel tempo, modellandolo.

I musicisti scelgono come suonare ascoltando il circostante: gli altri musicisti e l'ambiente. La relazione si articola in uno spettro che va dall'accordo all'opposizione. Il luogo è stato disegnato dal compositore: è lui a suonare con i musicisti, ascolta e modifica chi lo abita attraverso una rete di filtri.

Il compositore non decide cosa far accadere: predispone le condizioni perché qualcosa accada. L'opera non è fissata: varia con i musicisti coinvolti e con i diversi luoghi in cui viene suonata, e ogni volta riparte da dove si era interrotta. I musicisti non studiano un brano a memoria, studiano un modo di abitare quel luogo: ogni prova è già una sessione. Il concerto non è il punto d'arrivo, ma il momento in cui anche il pubblico ha accesso alla storia del luogo.

## Concept

*interfantasia* è un tentativo di ridistribuzione della presenza creativa fra tutti gli elementi in gioco: compositore, musicisti e ambiente. La ricerca prova a resistere al soggettivismo dell'atto creativo, cercando una creatività che non sia concentrata in un solo individuo, il compositore o il musicista virtuoso.

Tutti i musicisti coinvolti sono invitati ad abitare insieme lo spazio in cui sono inseriti. Il sistema non va inteso con una logica di causa ed effetto diretto, un suono che provoca una replica, ma come uno spazio che si modella nel tempo in base a ciò che ascolta.

Il sistema si lascia abitare. Ma cosa vuol dire abitare musicalmente? Quando si abita un luogo lo si modifica: si aggiungono oggetti, si spostano elementi, e il luogo assume nel tempo la forma che le persone al suo interno gli danno. E cos'è un oggetto, acusticamente? Un filtro: un ostacolo che devia il flusso acustico, lo assorbe o lo riflette. Abitare, allora, è cambiare la risposta acustica di un luogo, la sua forma, il suo timbro.

In *interfantasia* questo abitare si manifesta in un sistema di filtri digitali i cui parametri sono modificati dai musicisti e dall'ambiente fisico in cui il sistema è immerso. Modificando continuamente la forma dei filtri, i musicisti variano la forma dello spazio acustico, creando uno spazio altro.

Il compositore non scrive cosa accade durante la sessione: predispone le condizioni perché qualcosa possa accadere. Stabilisce le soglie, le sensibilità, i modi in cui il sistema si lascia modificare. Una volta configurato, il sistema vive nel tempo della sessione: ciò che avviene al suo interno dipende solo dai musicisti e dall'ambiente. I musicisti, dal canto loro, non producono suoni prescritti ma agiscono in relazione: la partitura indica, parametro per parametro, se ciò che suonano debba essere concorde o opposto rispetto a ciò che ascoltano.

## Il sistema elettronico

Il sistema è costruito in Pure Data (patch `luogo5.pd`). La catena, mostrata nello schema in apertura, è identica per ogni coppia microfono–altoparlante e viene replicata tante volte quanti sono i canali usati. Le diverse istanze operano indipendentemente nei processi interni ma sono accoppiate acusticamente attraverso lo spazio fisico, perché il suono di ciascun altoparlante raggiunge non solo il proprio microfono ma anche gli altri. Nelle sessioni di *interfantasia* sono state usate quattro istanze, ma il numero non è vincolato e si adatta allo spazio e all'organico.

- **T1 (Taratura).** Il segnale del microfono viene abilitato e scalato; raggiunge in parallelo descrittori (T2), bicomb (T4), generatori (T8) e zone (T9).
- **T2 (Descrittori spettrali).** Lo spettro viene analizzato ogni 100 ms: il centroide va alle zone (T9), gli altri cinque descrittori (flatness, spread, irregolarità, flux, onset) alle soglie (T3).
- **T3 (Soglie).** Spread e irregolarità passano prima per un riscalamento; tutti i descrittori attraversano poi un sistema di soglie che produce spostamenti di segno opposto fuori dalla zona morta.
- **T4 (Bicomb).** Accoglie gli ingressi dal microfono, dai generatori e dalle zone; al suo interno gli integratori (T5, in Faust) accumulano gli spostamenti prodotti dalle soglie e li rimappano sui parametri dei quattro filtri bicomb.
- **T7 (LAR ×4).** Quattro istanze parallele di *Level Adaptation and Regulation* (riduzione automatica del livello, ispirata all'*audible ecosystemics* di Di Scipio) mantengono il sistema al limite dell'autooscillazione.
- **T8 (Generatori).** Quando IIR o FIR raggiungono valori specifici viene emesso un bang che attiva rumore bianco o treni di impulsi, che rientrano nel bicomb.
- **T9 (Zone).** I parametri del bicomb attivano gradualmente tre processi: saturazione a soglia oscillante, ritardi multi-tap, modulazione ad anello con il centroide come frequenza portante.

## Il filtro bicomb

Il sistema è fatto di filtri: ma come sono fatti questi filtri? Partono da una base biquadratica, alla quale è però possibile modificare i tempi di ritardo per conferirle le caratteristiche di un filtro comb. Da qui il nome *bicomb*: mantengono tutti i parametri del biquadratico (frequenza di taglio, qualità, tipo di filtro) e insieme possono essere risonanti e avere i ritardi lunghi tipici dei filtri a pettine.

Questa condizione ibrida è stata scelta per evitare un comportamento prevedibile, riconoscibile attraverso le curve canoniche dei filtri. Se fossero biquadratici classici, la risposta sarebbe nota e semplice: di un passabasso si sa già cosa aspettarsi. Così, invece, i musicisti sono spinti a lavorare di ascolto. Pensando i filtri come oggetti, come pareti, sarebbero pareti irregolari e relative, in cui il tempo, i campioni, ne modificano anche la forma.

I parametri del bicomb non sono fissati una volta per tutte: vengono mossi di continuo dagli integratori (T5), che accumulano gli spostamenti prodotti dai descrittori spettrali dopo il passaggio per le soglie (T3). Il filtro reagisce così al contenuto del segnale che lo attraversa e, poiché il LAR (T7) mantiene il sistema al limite dell'autooscillazione, non si comporta come un colore passivo applicato al suono ma come un corpo risonante che ascolta. È in questo senso che il filtro diventa un interlocutore: la sua forma cambia in funzione di ciò che riceve, e ciò che riceve, attraverso il microfono, è insieme il suono dei musicisti e quello dello spazio.

I valori dei parametri raggiunti al termine di ogni sessione, comprese prove e studio, vengono salvati e ricaricati all'avvio successivo: ogni nuova sessione riparte dalla configurazione lasciata dalla precedente, e il sistema porta con sé una memoria di lungo periodo che attraversa le sessioni.

## La partitura

La partitura di *interfantasia* non prescrive suoni, ma relazioni. È costruita come una griglia di tessere, ognuna delle quali contiene cinque parametri timbrici: registro, dinamica, durata, larghezza e inarmonicità. Per ciascun parametro l'indicazione non è un valore assoluto da produrre, ma una posizione su una scala da −10 a +10 che esprime il grado di concordanza o contraddizione rispetto a ciò che il musicista ascolta in quel momento, cioè il suono degli altri musicisti e del sistema. Un valore +10 significa massima adesione al circostante per quel parametro, un valore −10 massima opposizione, lo zero indifferenza. Il suono concreto non è quindi determinato dalla partitura, ma nasce dall'incontro fra l'indicazione relazionale e l'ascolto.

![Griglia della partitura (versione 7×7): le celle a bordo tratteggiato sono pause, le tessere con una sola parola sono indicazioni timbriche, le altre combinano indicazioni relazionali e iconiche.]({{ "/images/interfantasia/partitura-griglia.png" | relative_url }})

Accanto alle indicazioni relazionali, alcune tessere contengono icone assolute (un registro acuto, una dinamica *pp*, una larghezza stretta) che fissano direttamente un parametro, oppure movimenti graduali del tipo *= → −10*, in cui il musicista parte da una posizione di indifferenza e si sposta progressivamente verso l'opposizione durante la durata della tessera. Esistono inoltre tessere di sola pausa, momenti di ascolto puro, e tessere a navigazione condizionale, in cui la direzione di lettura nella griglia è determinata da una condizione di ascolto (per esempio: se prevale il registro grave, scendere).

![Esempi di tessere: i cinque parametri ammettono valori relazionali sulla scala −10/+10, movimenti graduali, o icone assolute.]({{ "/images/interfantasia/partitura-tessera.png" | relative_url }})

Prima della sessione, ogni musicista traccia il proprio percorso indipendente attraverso la griglia: non c'è sincronizzazione fra le voci, e la stessa griglia può essere percorsa in modi diversi nelle diverse sessioni.

## Setup spaziale

**Versione principale (altoparlanti S.T.ONE, 96 kHz).** Quattro microfoni disposti agli angoli di un quadrato e quattro altoparlanti tetraedrici S.T.ONE ai punti cardinali, con routing incrociato fra microfoni e altoparlanti. Lo S.T.ONE è un altoparlante a quattro facce realizzato da Giuseppe Silvi: ciascuna faccia riceve una versione del segnale filtrata da un bicomb con un ritardo diverso, dato da indici di numeri primi in sequenza (i, i+1, i+2, i+3 campioni a 96 kHz, valori primi non commensurabili), così che la stessa sorgente viene irradiata nello spazio con una distribuzione spettrale differenziata per ogni direzione. Il sistema lavora a 96 kHz proprio per avere una grana temporale abbastanza fine da rendere musicalmente significative differenze di pochi campioni nei ritardi.

In questa configurazione il compositore è autonomo dal punto di vista tecnico: porta con sé gli altoparlanti S.T.ONE, l'interfaccia audio, i microfoni e tutto il necessario. Alla sala servono soltanto lo spazio di allestimento e l'alimentazione.

![Disposizione spaziale: microfoni ai quattro angoli, altoparlanti tetraedrici S.T.ONE ai punti cardinali, con routing incrociato.]({{ "/images/interfantasia/messa-in-ascolto.png" | relative_url }})

**Versione alternativa (multicanale, 48 kHz).** Quando non sia possibile usare gli S.T.ONE, ci si appoggia al sistema di diffusione della sala. L'architettura, i filtri bicomb e tutte le elaborazioni restano identiche, ma il segnale lavora a 48 kHz e il numero di canali si adatta all'impianto disponibile. Le quattro versioni filtrate dal bicomb vengono distribuite su un cerchio di altoparlanti tramite un panner mid/side: una configurazione diversa, che dà luogo a un'esperienza spaziale propria.

## Ascolto

Una registrazione di una sessione sarà disponibile qui a breve.

## Crediti

*interfantasia*, di Francesco Ferracuti.
Organico: clarinetto contrabbasso, timpano, sistema elettronico in tempo reale.
Altoparlanti tetraedrici S.T.ONE di Giuseppe Silvi.
