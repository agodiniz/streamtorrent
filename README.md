# player-sdk-js
Player SDK para streaming de torrent online

## Recursos
* Suporta magnet-uri e arquivos torrent externos
* Suporta vídeo (avi, mkv, mp4, m4v) com legendas (vtt, srt)
* Não são necessários plug-ins / extensões adicionais

## Uso básico
```html
...
    <div id="player" class="webtor" />
    <script>
        window.webtor = window.webtor || [];
        window.webtor.push({
            id: 'player',
            magnet: 'magnet:?xt=urn:btih:08ada5a7a6183aae1e09d831df6748d566095a10&dn=Sintel&tr=udp%3A%2F%2Fexplodie.org%3A6969&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Ftracker.empire-js.us%3A1337&tr=udp%3A%2F%2Ftracker.leechers-paradise.org%3A6969&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337&tr=wss%3A%2F%2Ftracker.btorrent.xyz&tr=wss%3A%2F%2Ftracker.fastcast.nz&tr=wss%3A%2F%2Ftracker.openwebtorrent.com&ws=https%3A%2F%2Fwebtorrent.io%2Ftorrents%2F',
            on: function(e) {
                if (e.name == window.webtor.TORRENT_FETCHED) {
                    console.log('Torrent fetched!', e.data);
                }
                if (e.name == window.webtor.TORRENT_ERROR) {
                    console.log('Torrent error!');
                }
            },
            poster: 'https://via.placeholder.com/150/0000FF/808080?Text=Sintel',
            subtitles: [
                {
                    srclang: 'en',
                    label: 'test',
                    src: 'https://raw.githubusercontent.com/andreyvit/subtitle-tools/master/sample.srt',
                }
            ],
            lang: 'en',
            i18n: {
                en: {
                    common: {
                        "prepare to play": "Preparing Video Stream... Please Wait...",
                    },
                    stat: {
                        "seeding": "Seeding",
                        "waiting": "Client initialization",
                        "waiting for peers": "Waiting for peers",
                        "from": "from",
                    },
                },
            },
        });
    </script>
    <script src="https://cdn.jsdelivr.net/npm/@webtor/player-sdk-js/dist/index.min.js" charset="utf-8"></script>
...
```
[Here is live example](https://webtor.io/sdk-example.html)

## Configuração do Player
Atributo  | Descrição
-----------|-------------
id         | ID do elemento onde o player será incorporado
magnet     | Magnet-uri (torrentUrl ot magnet é necessário)
width      | Largura de um iframe (pode ser qualquer valor de largura css: 800px, 100%, etc... 800px por padrão)  
height     | Altura de um iframe (pode ser qualquer valor de largura css: 800px, 100%, etc... opcional)
torrentUrl | Url do arquivo torrent (servidor HTTP ** DEVE ** incluir o header "Access-Control-Allow-Origin: *" para permitir o download do arquivo torrent no lado do cliente)
theme      | Pode ser "claro" ou "escuro". Se não for definido, o tema será selecionado automaticamente (opcional)
poster     | Url para a imagem do pôster (opcional)
on         | Callback-function para capturar eventos do jogador (opcional)
subtitles  | Array of subtitles. Every subtitle **MUST** include `srclang`, `label`, `src` (optional)
title      | Replaces original file name in a header with specific title (optional)
imdbId     | Helps to find subtitles and additional metadata (example values 'tt0133093' or '0133093' or even '133093', optional)
header     | Shows header with current progress and title (true/false, default: true)
pwd        | Selected directory in torrent (optional)
file       | Selected file in torrent (optional)
lang       | Override language (optional)
i18n       | Override i18n messages (optional)
features   | enables or disables specific player features (optional)

## Player features
Name        | Description
------------|-------------
title       | displays movie title
p2pProgress | displays p2p progress
subtitles   | enables subtitles control
settings    | enables settings control (cog)
fullscreen  | enables fullscreen control
playpause   | enables plays/pause control
currentTime | displays current time
timeline    | enables timeline control
duration    | displays total duration
volume      | enables volume control
chromecast  | enables chromecast support
browse      | enables file browser menu
download    | enables download button

Defaults:
```
features: {
    p2pProgress: true,
    title:       true,
    settings:    true,
    fullscreen:  true,
    subtitles:   true,
    continue:    true,
    playpause:   true,
    currentTime: true,
    timeline:    true,
    duration:    true,
    volume:      true,
    chromecast:  true,
    browse:      true,
    download:    true,
}
```

## Player events

Event types:

Name            | Description
----------------|-------------
TORRENT_FETCHED | Indicates that torrent has been fetched and also returns fetched torrent in `data` field
TORRENT_ERROR   | Indicates if some error has occured during loading (timeout, wrong url, etc...)
INITED          | Indicates if player can play content
OPEN            | Indicates that specific file in torrent was opened
PLAYER_STATUS   | Indicates current player status
CURRNET_TIME    | Indicates current player time
DURATION        | Indicates full content duration

Every event has following fields:

Name      | Description
----------|-------------
name      | Name of the event
data      | Specific data for this event
player    | Player representation that can be used after initialization to control playback

Player methods:

Name        | Description
------------|-------------
play        | starts playback (available only after first play-click in the player)
pause       | pauses playback (available only after first play-click in the player)
setPosition | sets player position in seconds (available only after first play-click in the player) 
open        | opens another file in torrent, full file-path should be provided
