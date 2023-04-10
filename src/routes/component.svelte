<script lang="ts">
  import logo from '$lib/assets/taha_lkobra.jpg';

  export let angle : number ;
  export let rotation : boolean ;
  export let hit : boolean;
  export let bodynumber : number;



</script>
<div class="snake flex flex-reverse-row" style={`transform: rotate(${rotation ? '-90deg' : `${angle}deg`})`}>

{#if hit}

{#each Array(bodynumber) as _, key}


<div   class="border bg-blue-800 w-12 h-12 "></div>

{/each}

{/if}
<img alt="tahalkobra" src={logo} style={`width: 48px; height: 48px`}
/>
</div>

