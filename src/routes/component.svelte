<script lang="ts">
  import logo from '$lib/assets/taha_lkobra.jpg';

  export let angle : number ;
  export let rotation : boolean ;



</script>

<img alt="tahalkobra" src={logo} class={`w-12 h-12 ${rotation ? '-rotate-90' :`rotate-${angle}`}`} />

