<script lang="ts">
    import type { Writable } from "svelte/store"
    import { getQuery, type SearchPieData } from "./CustomPie"
    import { searchJoin } from "./root"
    import { custom_pie_mode, searchString } from "./stores"
    import CancelablePromise, { cancelable } from "cancelable-promise"
    import { i18n } from "./i18n"

    export let data: Writable<SearchPieData>
    export let promise: CancelablePromise<number> = CancelablePromise.resolve(0)
    let last_search: string
    let last_mode: string

    $: $data.label = searchJoin($searchString, $data.search)
    $: if (last_search !== $data.label || last_mode !== $custom_pie_mode) {
        promise.cancel()
        promise = cancelable(getQuery($data.label, $custom_pie_mode))
        promise.then((result) => {
            $data.value = result
        })
        last_search = $data.label
        last_mode = $custom_pie_mode
    }
</script>

<input type="text" bind:value={$data.search} placeholder={i18n("search-string")} />
<input type="text" bind:value={$data.colour} placeholder={i18n("css-colour")} />
