<script>
    import { onMount } from "svelte";

    onMount(() => {
        const canvas = document.getElementById("gameCanvas");

        // Załaduj `game.js`
        const script = document.createElement("script");
        script.src = "/game/game.js"; // Upewnij się, że ścieżka jest poprawna
        script.onload = () => {
            console.log("Plik game.js został załadowany!");
            startGame(canvas);
        };
        script.onerror = () => {
            console.error("Nie udało się załadować pliku game.js");
        };
        document.body.appendChild(script);
    });

    function startGame(canvas) {
        // Sprawdź, czy `Engine` istnieje
        if (typeof Engine === "undefined") {
            console.error(
                "Engine nie jest zdefiniowany! Upewnij się, że plik game.js jest poprawnie załadowany.",
            );
            return;
        }

        const wasmPath = "/game/game";
        const pckPath = "/game/game.pck";

        const engine = new Engine();

        Promise.all([engine.init(wasmPath), engine.preloadFile(pckPath)])
            .then(() => {
                return engine.start({
                    canvas: canvas,
                    args: ["--main-pack", pckPath],
                });
            })
            .then(() => {
                console.log("Silnik Godot został uruchomiony!");
            })
            .catch((err) => {
                console.error("Błąd podczas uruchamiania gry:", err);
            });
    }
</script>

<canvas id="gameCanvas" style="width: 100%; height: 100vh;"></canvas>
