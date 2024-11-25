<script lang="ts">
    import { goto } from "$app/navigation";
    let moving = false;

    function isTransitioning(element: HTMLElement): boolean {
        const computedStyle = getComputedStyle(element);
        const transitionDuration = parseFloat(computedStyle.transitionDuration);
        const transitionDelay = parseFloat(computedStyle.transitionDelay);
        return transitionDuration + transitionDelay > 0;
    }

    function changePos(event: any, arg: boolean) {
        const element = event.target as HTMLElement;
        if (arg) {
            element.style.transform = "translatey(-25px)";
            element.style.transition =
                "transform 0.3s ease, box-shadow 0.3s ease";

            const onTransitionEnd = () => {
                element.removeEventListener("transitionend", onTransitionEnd);
            };
            element.addEventListener("transitionend", onTransitionEnd);
        }
        if (!arg && !moving) {
            element.style.transform = "translatey(0px)";
            element.style.transition =
                "transform 0.3s ease, box-shadow 0.3s ease";
        }
    }

    function moveOut(event: any) {
        moving = true;
        const element = event.target as HTMLElement;

        element.style.transition = "transform 0.5s ease";
        element.style.transform = "translatey(-500px)";

        const onTransitionEnd = () => {
            element.removeEventListener("transitionend", onTransitionEnd);
            element.remove();
            goto("/game");
        };

        element.addEventListener("transitionend", onTransitionEnd);
    }
</script>

<div class="h-screen flex justify-center items-center select-none">
    <button
        class="text-4xl p-6"
        on:mouseenter={() => changePos(event, true)}
        on:mouseleave={() => changePos(event, false)}
        on:click={() => moveOut(event)}
    >
        Play
    </button>
</div>
