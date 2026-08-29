# FocusFlow: Timer Pomodoro

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Spotify API](https://img.shields.io/badge/Spotify%20API-1DB954?style=for-the-badge&logo=spotify&logoColor=white)](https://developer.spotify.com/documentation/web-api)

FocusFlow è un timer Pomodoro per sessioni di studio senza distrazioni. Ha un timer con animazione "liquid" a forma di tazza di caffè, durate personalizzabili per focus e pausa, e si integra con Spotify per mostrare il brano in riproduzione.

**Demo:** [st0rmosu.github.io/CustomPomodoroTimer](https://st0rmosu.github.io/CustomPomodoroTimer)

## Caratteristiche

- **Timer Pomodoro**: sessioni FOCUS/BREAK con display `MM:SS` e indicatore di sessione.
- **Animazione liquid**: la tazza di caffè si riempie e si svuota in base al tempo rimanente.
- **Durate personalizzabili**: focus (5-60 min) e pausa (1-30 min), applicabili al volo.
- **Controlli**: START, PAUSE, RESET e SKIP della sessione.
- **Integrazione Spotify**: OAuth Implicit Grant, brano in riproduzione (copertina, titolo, artista) e barra di avanzamento sincronizzata.
- **Dark mode**: design per lunghe sessioni di studio.
- **Suoni di notifica**: avvisi al cambio di sessione.

## Demo

Prova la versione live:

[st0rmosu.github.io/CustomPomodoroTimer](https://st0rmosu.github.io/CustomPomodoroTimer)

## Uso

1. Apri la pagina: parte la sessione FOCUS da 25:00 con la tazza piena.
2. Premi **START** per avviare, **PAUSE** per sospendere, **RESET** per ripartire e **SKIP** per passare subito a pausa o focus.
3. Regola i minuti di FOCUS e BREAK negli input.
4. Clicca **CONNECT** per collegare Spotify: la card mostra brano, artista e progresso.

## Tecnologie

- **HTML5 & CSS3** — struttura e animazioni liquid con CSS custom properties
- **JavaScript ES6+** — state machine del timer e cicli Focus/Break
- **Spotify Web API** — autenticazione OAuth e sincronizzazione del brano
- **Web Audio API** — sintesi e riproduzione dei suoni di notifica

## Installazione

Prerequisiti: un browser moderno. Non serve installare nulla.

```bash
git clone https://github.com/St0rmosu/CustomPomodoroTimer.git
cd CustomPomodoroTimer
# apri index.html, oppure
python3 -m http.server 8080
```

### Collegare Spotify (opzionale)

1. Crea un'app su [developer.spotify.com](https://developer.spotify.com/dashboard).
2. Imposta il `Redirect URI` al percorso dove è hostato il timer, per esempio `https://st0rmosu.github.io/CustomPomodoroTimer/`.
3. Aggiorna `client_id` e redirect URI nel codice (`index.html`) con i tuoi valori.

## Architettura

Single-page application senza framework. Un oggetto `Timer` separa il conto alla rovescia (setInterval) dall'aggiornamento della UI e dell'animazione:

![Diagramma architettura](docs/architecture.png)

Spotify riceve il token via Implicit Grant (frammento `#access_token` dell'URL di redirect) e interroga con polling l'endpoint "currently playing" per aggiornare copertina, titolo, artista e progresso.

## Struttura del progetto

```
CustomPomodoroTimer/
├── index.html     # Unica pagina: stile, struttura e logica JS
├── .nojekyll      # Abilita il deployment su GitHub Pages
└── README.md
```

## Documentazione API

Integrazione con la Spotify Web API (sola lettura della riproduzione):

| Flusso | Dettaglio |
|---|---|
| Autorizzazione | OAuth 2.0 Implicit Grant, scopes: `user-read-currently-playing`, `user-read-playback-state` |
| Redirect | Token restituito nel frammento `#access_token` dell'URL |
| Endpoint | `GET https://api.spotify.com/v1/me/player/currently-playing` (Bearer token) |

La risposta usa `item.name` (titolo), `item.artists[0].name` (artista), `item.album.images` (copertina), `progress_ms` e `item.duration_ms` (barra di avanzamento).

*Sviluppato da Lorenzo Recchia.*
