<script lang="ts">
    import fried from '$lib/assets/fried.png';
    import logo from '$lib/assets/taha_lkobra.jpg';
    import tahaloose from '$lib/assets/gametaha.jpeg';

    import { onMount } from 'svelte';
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
    let prevHeadX = 0;
    let prevHeadY = 0;
    let score = 0;
    let highscore=0;
    let popimage=false;
    let interval;


    // Initialize the head position
      let x_value = 100;
      let x_top = 100;

      // Initialize the bodyParts array with two body elements
      let bodyParts = [
  { x: x_value - 48, y: x_top },
  { x: x_value - 64, y: x_top },
];


function onKeyDown(e) {
  switch(e.keyCode) {
    case 39:
      if (x_direction !== "left") {
        x_direction = "right";
        rotation = false;
        angle = 0;
      }
      break;

    case 40:
      if (x_direction !== "up") {
        x_direction = "down";
        rotation = false;
        angle = 90;
      }
      break;

    case 38:
      if (x_direction !== "down") {
        x_direction = "up";
        rotation = true;
        angle = 90;
      }
      break;

    case 37:
      if (x_direction !== "right") {
        x_direction = "left";
        rotation = false;
        angle = 180;
      }
      break;

    default:
      break;
  }
}

$: {
  if (!popimage) {
    if (interval) clearInterval(interval);
    interval = setInterval(() => {
      if (!gameover) {
        moovesnake();
        checkSelfCollision();
        checkCollision();
      } else {
        clearInterval(interval);
        resetGame(); // No need to call handlePlayAgain() here
      }
    }, 50);
  }
}


  
 function moovesnake() {
  const prevHeadX = x_value;
  const prevHeadY = x_top;

  if (x_direction === "right") {
    x_value += 16;
  } else if (x_direction === "left") {
    x_value -= 16;
  } else if (x_direction === "down") {
    x_top += 16;
  } else if (x_direction === "up") {
    x_top -= 16;
  }

  if (hit) {
    hit = false;
    bodynumber += 1;
    bodyParts.unshift({ x: prevHeadX, y: prevHeadY });
    rowStart = randomIntFromInterval(50, 400);
    colStart = randomIntFromInterval(50, 400);
  }

  let newBodyParts = [{ x: prevHeadX, y: prevHeadY }];
  for (let i = 1; i < bodyParts.length; i++) {
    newBodyParts.push({ x: bodyParts[i - 1].x, y: bodyParts[i - 1].y });
  }
  bodyParts = newBodyParts;
}







function randomIntFromInterval(min, max) { // min and max included 
  return Math.floor(Math.random() * (max - min + 1) + min)
}
let rowStart = 200;
let colStart = 200;


 let Chicken = () => {
  return `chicken border h-16 w-16 self-center  place-self-center relative`
 
  

}

// this is the collision logic
function checkSelfCollision() {
  const headRect = {
    left: x_value + 18,
    right: x_value + 30,
    top: x_top + 18,
    bottom: x_top + 30
  };

  for (let i = 4; i < bodyParts.length; i++) {
    const bodyRect = {
      left: bodyParts[i].x,
      right: bodyParts[i].x + 12,
      top: bodyParts[i].y,
      bottom: bodyParts[i].y + 12
    };

    if (
      headRect.left <= bodyRect.right &&
      headRect.right >= bodyRect.left &&
      headRect.top <= bodyRect.bottom &&
      headRect.bottom >= bodyRect.top
    ) {
      gameover = true;
      hit = false;
      popimage = true;
      break;
    }
  }
}







  const checkCollision = () => {
    if (!component) return;
    const componentRect = component.getBoundingClientRect();
    const gameBorderRect = gameBorder.getBoundingClientRect();
    const chickenRect=chicken.getBoundingClientRect();
          if (
        componentRect.left < gameBorderRect.left  ||
        componentRect.right  > gameBorderRect.right + 12 ||
        componentRect.top < gameBorderRect.top  ||
        componentRect.bottom  > gameBorderRect.bottom+12
      ){
       gameover = true;
       hit=false

      popimage=true;
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
   if(!popimage){ 
   if(score>=highscore){
    highscore=score+1
    score+=1

   }
   else{
    score+=1
   }
  }

   rowStart = randomIntFromInterval(50,400);
   colStart = randomIntFromInterval(50,400);
   bodyParts.push({ x: x_value, y: x_top }); // Add a new body part when the chicken is hit

  
  }

}
onMount(() => {
  gameBorder = document.querySelector('.gameborder');
  chicken = document.querySelector('.chicken');


  let interval = setInterval(
    ()=>{
      if(!gameover){
        checkSelfCollision();
        checkCollision();
      } else {
        clearInterval(interval);
        resetGame();
      }
    }, 50)
});


function resetGame() {
  if (score > highscore) {
    highscore = score;
  }

  bodynumber = 0;
  x_value = 100;
  x_top = 100;
  hit = false;
  x_direction = "";
  angle = 0;
  rotation = false;
  gameover = false;
  score = 0;
  bodyParts = [];
}



function handlePlayAgain() {
  console.log("handleplayer"+ "/"+ popimage)

  resetGame();
  popimage = false;

}


</script>

<body class="body h-screen w-screen flex flex-row justify-center bg-gray-900">
  <div class="score flex items-center justify-center text-white font-bold text-1xl bg-gray-800 h-16 w-full fixed top-0">
    <div class="mx-32">Score: {score}</div>
    <div class="mx-32">High Score: {highscore}</div>
  </div>
  {#if popimage}
  <div class="gameover flex items-center justify-center flex-col">
    <img src={tahaloose} alt="you loose" class="w-2/3 md:w-1/3 my-8">
    <button class="btn bg-blue-500 text-white rounded-lg px-4 py-2 my-4" on:click={handlePlayAgain}>Play Again</button>
  </div>
  {/if}
  <div class={`gameborder grid grid-rows-6 grid-cols-6 bg-gradient-to-br from-sky-300 to-sky-100 rounded-lg h-2/3 w-2/3 self-center ${popimage ? 'hidden' : ''}`}>
    <div class="snake-container relative" on:keydown={onKeyDown} style={`left: ${x_value}px; top: ${x_top}px`}>
      {#each bodyParts as part, i (i)}
      <div
        class={`snake-body-part w-12 h-12 absolute rounded-full ${i % 2 === 0 ? 'bg-green-700' : 'bg-green-500'}`}
        style={`left: ${part.x - x_value}px; top: ${part.y - x_top}px`}
      ></div>
      {/each}
      <img
        alt="tahalkobra"
        class="lkobra"
        src={logo}
        style={`width: 48px; height: 48px; transform: rotate(${rotation ? '-90deg' : `${angle}deg`}); z-index: 10; border-radius: 8px; box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.3);`}
        bind:this={component}
      />
    </div>
    <div class={Chicken()} style="left: {rowStart}px; top: {colStart}px">
      <img src={fried} alt="fried chicken"  ></div>
  </div>
</body>

<svelte:window on:keydown={onKeyDown}  />



