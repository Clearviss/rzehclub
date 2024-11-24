<script lang="ts">
    import { onMount } from "svelte";

    onMount(() => {
        const texts = ["Loading", "Loading.", "Loading..", "Loading..."];
        let i = 0;

        const interval = setInterval(() => {
            document.querySelector(".italic")!.textContent = texts[i];
            i = (i + 1) % texts.length;
        }, 500);

        setTimeout(() => {
            window.location.href = "/";
        }, 5000);
    });
</script>

<div class="h-screen flex flex-col gap-4 justify-center items-center">
    <div class="loader"></div>
    <div class="italic opacity-30 select-none">Loading</div>
</div>

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
