<script>
    import { onMount } from "svelte";
    let loadig = true;
    onMount(() => {
        const texts = ["Loading", "Loading.", "Loading..", "Loading..."];
        let i = 0;

        const interval = setInterval(() => {
            // @ts-ignore
            document.querySelector(".italic").textContent = texts[i];
            i = (i + 1) % texts.length;
        }, 500);

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
                loadig = false;
            })
            .catch((err) => {
                console.error("Błąd podczas uruchamiania gry:", err);
            });
    }
</script>

{#if loadig}
    <div class="h-screen flex flex-col gap-4 justify-center items-center">
        <div class="loader"></div>
        <div class="italic opacity-30 select-none">Loading</div>
    </div>
{/if}
<canvas id="gameCanvas" style="width: 100%; height: 100vh;"></canvas>

<style>
    /* HTML: <div class="loader"></div> */
    .loader {
        width: 120px;
        height: 20px;
        transform: skewX(-45deg);
        background: linear-gradient(#000000 0 0) left -30px top 0/30px 20px no-repeat
            #ccc;
        animation: l3 1s infinite linear;
    }

    @keyframes l3 {
        100% {
            background-position: right -30px top 0;
        }
    }
</style>
