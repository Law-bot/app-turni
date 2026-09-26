# Turni

App per organizzare i turni di lavoro in due (o in squadra). File statico, nessun server da gestire — i dati condivisi vivono su Firestore (Firebase), gratuito per questo volume di dati.

## 1. Crea il database condiviso (una volta sola)

1. Vai su [console.firebase.google.com](https://console.firebase.google.com), accedi con un account Google qualsiasi e crea un nuovo progetto (nome libero, es. "Turni").
2. Nel progetto: **Build → Firestore Database → Crea database** → modalità *produzione* → scegli una regione europea (es. `eur3`).
3. Tab **Regole**, sostituisci tutto con:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
   e premi **Pubblica**.

   ⚠️ Questo rende leggibile/scrivibile il database a chiunque conosca l'indirizzo del progetto — accettabile per dei turni di lavoro tra due persone, ma non mettere qui dati sensibili (password, documenti, ecc.).

4. Icona ingranaggio in alto a sinistra → **Impostazioni progetto** → scorri fino a "Le tue app" → clicca l'icona **`</>`** (Web) → dai un nome (es. "turni-web") → **Registra app**. Ti mostra un blocco `firebaseConfig = {...}`: copialo.
5. Apri `index.html`, cerca il blocco `CONFIGURAZIONE FIREBASE` in cima allo `<script>` e incolla i tuoi valori al posto di `INCOLLA_QUI`.

## 2. Mettila online

Stesso procedimento di "Spese":

1. Crea un repository nuovo su GitHub.
2. Carica `index.html`, `manifest.json`, `icon.png`, `icon-192.png`.
3. Settings → Pages → Source: `Deploy from a branch`, branch `main`, cartella `/ (root)`. Salva.
4. Dopo un minuto l'app è su `https://TUONOME.github.io/NOMEREPO/`.

Manda quel link al collega: aprendolo da telefono, Condividi → Aggiungi a Home (iPhone) o menù → Installa app (Android). Entrambi vedrete e modificherete lo stesso calendario turni in tempo reale.

## Come funziona

- **Impostazioni** (ingranaggio in alto a destra): nome del locale e squadra (persone tra cui dividere i turni, ognuna con un colore).
- Ogni giorno della settimana ha un pulsante **+** per aggiungere un turno (persona, orario, tipo di servizio, nota facoltativa).
- Tocca un turno esistente per modificarlo o eliminarlo.
- In alto: ore totali e numero turni della settimana in corso, più un avviso per il prossimo turno di oggi.
