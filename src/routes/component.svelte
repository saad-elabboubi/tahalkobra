<script lang="ts">
  import logo from '$lib/assets/taha_lkobra.jpg';

  let angle = 0;
  let negative : boolean = false;

  function onKeyDown(e) {
    switch(e.keyCode) {
      case 38:
        negative = true;
        angle = 90;
        console.log(negative)
        break;
      case 39:
        negative = false;
        angle = 0;
        break;
      case 40:
        negative = false;
        angle = 90;
        break;
      default:
        break;
    }
  }


</script>

<div class="snake-body w-20 h-12 bg-green-600 border border-black"></div>
<div class="snake-body w-20 h-12 bg-green-600 border border-black"></div>
<img alt="tahalkobra" src={logo} class={`w-20 h-12 ${negative ? '-rotate-90' : `rotate-${angle}`}`} />

<svelte:window on:keydown={onKeyDown}  />