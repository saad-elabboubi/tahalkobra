<script lang="ts">
  import logo from '$lib/assets/taha_lkobra.jpg';

  export let angle : number ;
  export let rotation : boolean ;
  export let hit : boolean;
  export let bodynumber : number;



</script>
{#if hit}
{#each Array(bodynumber) as _, key}
<div  key={key} class="border bg-blue-800 w-12 h-12 "></div>
{/each}

{/if}
<img alt="tahalkobra" src={logo} class={`w-12 h-12 ${rotation ? '-rotate-90' :`rotate-${angle}`}`} />

