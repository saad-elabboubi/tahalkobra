<script lang="ts">
    import Component from "./component.svelte";
    import { writable } from 'svelte/store';

    import { onMount } from 'svelte';
    let x_value : number = 0
    let x_top : number = 0 
    let x_direction : string = ""
    let angle : number = 0 ;
    let rotation : boolean = false;
    let x_row : number = 2;
    let y_col : number = 4;
    const gameBorderPadding = 0;
    let chickenX = 6;
    let chickenY = 3;   
    let component;
    let gameBorder;
    let chicken;
    let classchicken;
    let value;
    let hit =false;
    let gameover = false;
    let bodynumber=0;


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
const interval = setInterval(
  ()=>{
if(!gameover){
moovesnake()}
else {clearInterval(interval);}

},50)

$:moovesnake(x_direction)

function randomIntFromInterval(min, max) { // min and max included 
  return Math.floor(Math.random() * (max - min + 1) + min)
}
let rowStart = 200;
let colStart = 200;


 let Chicken = () => {
  return `chicken border border-red-800 h-11 w-11  self-center  place-self-center relative`
 
  

}



//col-start-${n2}
//row-start-${n1}
// this is the collision logic



  const checkCollision = () => {
    const componentRect = component.getBoundingClientRect();
    const gameBorderRect = gameBorder.getBoundingClientRect();
    const chickenRect=chicken.getBoundingClientRect();
    if (
      componentRect.left < gameBorderRect.left  ||
      componentRect.right > gameBorderRect.right + 100 ||
      componentRect.top < gameBorderRect.top  ||
      componentRect.bottom > gameBorderRect.bottom + 56
    ) {
       gameover = true;
       hit=false
      alert('Game over!');
      resetGame()

    }
  else if (
    componentRect.left <= chickenRect.right &&
    componentRect.right >= chickenRect.left &&
    componentRect.top <= chickenRect.bottom &&
    componentRect.bottom >= chickenRect.top
  ) {

   hit = true
   bodynumber+=1 
   console.log(bodynumber)
   rowStart = randomIntFromInterval(50,400);
   colStart = randomIntFromInterval(50,400);
  
  };}

  onMount(() => {
    gameBorder = document.querySelector('.gameborder');
    chicken = document.querySelector('.chicken');

    let interval = setInterval(
        ()=>{
          if(!gameover){
            checkCollision()
            }
        else {clearInterval(interval)
              resetGame()}

              },50)
              });

  function resetGame() {
  x_value = 0;
  x_top = 0;
  x_direction = "";
  angle = 0;
  rotation = false;
  gameover = false;
    }
  
</script>


<body class="body h-screen w-screen flex flex-row justify-center ">
    <div class="gameborder grid grid-rows-6 grid-cols-6 bg-emerald-200  h-2/3 w-1/2 self-center ">
        <div class="flex flex-row relative" style="left: {x_value}px; top: {x_top}px" on:keydown={onKeyDown} bind:this={component}  >
            <Component angle={angle} rotation={rotation} hit={hit} bodynumber={bodynumber}/>
           

        </div>
        <div class={Chicken()} style= "left: {rowStart}px; top: {colStart}px">okay</div>

    </div>
    
</body>
<svelte:window on:keydown={onKeyDown}  />
