<script>
    import { onMount } from "svelte";
    import {resultstore} from "../stores/resultsfbsc";
    import SearchBar from "../components/searchBar.svelte";
    import algoliasearch from "algoliasearch/lite"
    import {querystore} from"../stores/querystore"
    export let params;
    $: results = []

    onMount(() => {
        const unsubscribe = resultstore.subscribe((data) => {
            results = data
        })

        if (params.query === "") {
            resultstore.set([])
        }
    })
    let searchClient;
    let index;
    let algoliaAppId = "SG8XZAM7JF"
    let algoliaPublicKey = "bc5c6545619049347419ee4574613e3c"
    $: query = ""
    $: resultlist = []
    onMount(() => {
        searchClient = algoliasearch(
            algoliaAppId, algoliaPublicKey
        )
        index = searchClient.initIndex("camps")
        const unsubsribe = querystore.subscribe(data => {
            query = data 
            return
        })

    })
    

</script>

<div class="search-page">
    <SearchBar/>
    {#if results.length < 1}
        <h3>No results found for "{params.query}"</h3>
    {:else}
    <ul>
        {#each results as result}
        <li class="resultitem">
            {result.name}
        </li>
        {/each}
    </ul>
    {/if}
</div>
<style>
    .resultitem{
        color:white;
        font-size:24px;
        font-weight:100;
        list-style:none;
        margin-bottom:25px;
    }

    ul {
        text-align: center;
    }
</style>