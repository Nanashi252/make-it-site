# make-it-site
Website to display videos from a playlist to get inspired
<html>
<head>
    <script>
        var playlistId = "PLjokz9EGF6rzlAZ4GkGJ_Qqla6QSvlXfZ"; // Deine YouTube-Playlist-ID
        var players = [];

        function onYouTubeIframeAPIReady() {
            var container = document.getElementById("video-grid");
            var maxVideos = 25; // Falls deine Playlist größer ist, anpassen

            // Erstelle eine Liste mit eindeutigen Startpunkten (0, 1, 2, ... 49) und mische sie
            var uniqueStartIndices = Array.from({ length: maxVideos }, (_, i) => i).sort(() => Math.random() - 0.5);
            
            for (var i = 0; i < 12; i++) {
                var div = document.createElement("div");
                div.className = "video-tile";
                div.id = "player" + i;
                container.appendChild(div);

                var startIndex = uniqueStartIndices[i % uniqueStartIndices.length]; // Eindeutigen Startindex auswählen

                players[i] = new YT.Player("player" + i, {
                    height: '250',
                    width: '450',
                    playerVars: {
                        'listType': 'playlist',
                        'list': playlistId,
                        'autoplay': 1,
                        'controls': 1,
                        'mute': 1,
                        'index': startIndex // Jedes Video startet unterschiedlich!
                    },
                    events: {
                        'onStateChange': function(event) {
                            if (event.data === YT.PlayerState.ENDED) {
                                playRandomVideo(event.target);
                            }
                        }
                    }
                });
            }
        }

        // Funktion: Zufälliges Video nach Ende abspielen
        function playRandomVideo(player) {
            var maxVideos = 25;
            var randomIndex = Math.floor(Math.random() * maxVideos);
            player.loadPlaylist({
                list: playlistId,
                index: randomIndex,
                autoplay: 1
            });
        }
    </script>
    <style>
        #video-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }
        .video-tile {
            width: 450px;
            height: 250px;
        }
    </style>
</head>
<body>
    <div id="video-grid"></div>

    <script src="https://www.youtube.com/iframe_api"></script>
</body>
</html>
