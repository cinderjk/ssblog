<script lang="ts">
    import Layout from "../Layouts/App.svelte";
    import { onMount } from "svelte";
    import axios from "axios";
    let users = [];
    let error = null;
    onMount(async () => {
        try {
            const res = await axios.get(
                "https://63f70447833c7c9c607b3807.mockapi.io/api/v1/users"
            );
            users = res.data;
        } catch (e) {
            error = e;
        }
    });
</script>

<Layout header hideHeader headerHeight={56} let:scroller>
    <div slot="header">
        <div class="header" class:shadow={!!scroller.scroll}>Page title</div>
    </div>

    {#if error}
        <p>Something went wrong</p>
    {:else}
        <ul>
            {#each users as user}
                <li>Name: {user.name}</li>
            {/each}
        </ul>
    {/if}
</Layout>
