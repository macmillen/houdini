<script lang="ts">
  import { browser } from '$app/environment';
  import { graphql } from '$houdini';
  import cache from '$houdini/runtime/cache';
  import { routes } from '$lib/utils/routes.js';

  // leave this in to make sure we don't break relative imports from
  // the root layout. see: https://github.com/HoudiniGraphql/houdini/issues/629
  import Test from './Test.svelte';

  if (browser) {
    // @ts-ignore
    window.cache = cache;
  }

  let routesKvp = Object.keys(routes).map((key: string) => {
    return { key, value: (routes as Record<string, string>)[key] };
  });

  $: info = graphql(`
    query LayoutSession @load {
      session
    }
  `);
</script>

<svelte:head>
  <title>Houdini • e2e • SvelteKit</title>
</svelte:head>

<slot />

<Test />

<nav>
  {#each routesKvp as { key, value }}
    <div>
      <a href={value}>{key}</a>
    </div>
  {/each}
</nav>

<div id="layout-session">
  {$info.data?.session}
</div>
