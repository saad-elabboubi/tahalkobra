<script lang="ts">
    import Component from "./component.svelte";
    import { onMount } from 'svelte';
    let x_value : number = 0
    let x_top : number = 0 
    let x_direction : string = ""
    let angle : number = 0 ;
    let rotation : boolean = false;
    let x_row : number = 2;
    let y_col : number = 4;
    const gameBorderPadding = 0;
    let component;


    function onKeyDown(e) {

    switch(e.keyCode) {
      case 39:
      x_direction="right"
      rotation = false;
      angle = 0;
      break;

     case 40:
      x_direction="down"
      rotation = false;
      angle = 90;
      break;

      case 38:
        x_direction="up"
        rotation = true;
        angle = 90;
        break;

      case 37: 
      x_direction="left"
      rotation = false;
      angle = 180;
      break;

      default:
        break;
    }
  }

function moovesnake(){
  if(x_direction=="right"){
    x_value+=16
 
  }
  else if(x_direction=="left"){
    x_value-=16
  }
  
  else if(x_direction=="down"){
    x_top+=16
  }
  else if(x_direction=="up"){
    x_top-=16
  }
  

}
setInterval(moovesnake,50)
$:moovesnake(x_direction)

function randomIntFromInterval(min, max) { // min and max included 
  return Math.floor(Math.random() * (max - min + 1) + min)
}
$:Chicken(x_row,y_col)

function Chicken(){
  let y_col : number = randomIntFromInterval(1, 6)
  let x_row : number = randomIntFromInterval(1, 6)
  return `border border-red-800 h-11 w-11 row-start-${x_row} self-center col-start-${y_col} place-self-center`
}

// this is the collision logic

$: if (component){
  component.style.top = x_top + 'px'
  component.style.left = x_value + 'px'
  console.log(component.style.top, component.style.left)}

  let gameBorder;

  const checkCollision = () => {
    const componentRect = component.getBoundingClientRect();
    const gameBorderRect = gameBorder.getBoundingClientRect();
    if (
      componentRect.left < gameBorderRect.left  ||
      componentRect.right > gameBorderRect.right + 56 ||
      componentRect.top < gameBorderRect.top  ||
      componentRect.bottom > gameBorderRect.bottom + 56
    ) {
      alert('Game over!');
    }
  };

  onMount(() => {
    gameBorder = document.querySelector('.gameborder');
    setInterval(checkCollision, 50);
  });

</script>


<body class="body h-screen w-screen flex flex-row justify-center ">
    <div class="gameborder grid grid-rows-6 grid-cols-6 bg-emerald-200  h-2/3 w-1/2 self-center ">
        <div class="flex flex-row relative" style="left: {x_value}px; top: {x_top}px" on:keydown={onKeyDown} bind:this={component}  >
            <Component angle={angle} rotation={rotation}/>
           

        </div>
        <div class={Chicken()} ></div>

    </div>
    
</body>
<svelte:window on:keydown={onKeyDown}  />
