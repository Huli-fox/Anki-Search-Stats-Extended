<script lang="ts">
    import { i18n } from "./i18n"

    export let faded = true
</script>

<div style:opacity={faded ? 0.5 : 1}>
    <slot>{i18n("no-data")}</slot>
</div>

<style>
    div {
        width: min(var(--graph-width), 100%);
        height: var(--graph-height);

        opacity: 0.5;

        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
</style>
