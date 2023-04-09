<script lang="ts">
    import Component from "./component.svelte";
    let x_value : number = 0
    let x_top : number = 0 
    let x_direction : string = ""
    let angle : number = 0 ;
    let rotation : boolean = false;

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
</script>


<body class="body h-screen w-screen flex flex-row justify-center ">
    <div class="gameborder grid grid-rows-6 grid-cols-6 bg-emerald-200  h-2/3 w-1/2 self-center ">
        <div class="flex flex-row relative" style="left: {x_value}px; top: {x_top}px" on:keydown={onKeyDown}  >
            <Component angle={angle} rotation={rotation}/>

        </div>


    </div>
    
</body>
<svelte:window on:keydown={onKeyDown}  />
