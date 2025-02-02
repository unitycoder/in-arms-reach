<script lang="ts">
import NeedleEngine from "./NeedleEngine.svelte";
import Menu from "./Menu.svelte";
import { Context, isQuest } from "@needle-tools/engine";
import logo from "./lib/titleImage.png";
import swipe from "./lib/swipe-gesture.png";
import MadeWithNeedle from "./MadeWithNeedle.svelte";
import laurels from "./lib/laurels-dark.png";

let context: Context;
let wasPlaced = false;

$: console.log("Was placed changed", wasPlaced);
$: _isQuest = isQuest();

</script>

<div class="logo">
    <img alt="Logo" src={logo} /><br/>
</div>

<div class="laurels">
  <img class:hidden={wasPlaced} src={laurels} alt="Laurels" class="laurels" />
</div>

<div class="vignette"></div>

<div class="menu" class:hidden={wasPlaced}>
  {#if !_isQuest}
  <img src={swipe} alt="Infographic explaining to draw on the screen"/>
    <div class="anim">
      <p>Draw to reveal</p>
      <p class="subtitle">Sound on!</p>
    </div>
    {:else}
    <div>
      <p>Tap buttons below</p>
      <p class="subtitle">to start the experience</p>
    </div>
    {/if}
  </div>

<div class="touch-blocker"></div>

<NeedleEngine bind:context={context} bind:wasPlaced={wasPlaced}></NeedleEngine>

<Menu bind:context={context}></Menu>

<MadeWithNeedle></MadeWithNeedle>

<style>

div.logo {
    position:absolute;
    left: 10px;
    top: -30px;
    z-index: 1000;

    display: flex;
    flex-direction: row;
    align-items: center;
}

div.logo img  {
    width: min(300px, 50vw);
}

div.laurels {
  position: absolute;
  bottom: 150px;
  z-index: 1000;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 100%;
}

div.laurels img {
  max-width: 100%;
  max-height: 30vh;
}  
@media (max-height: 800px) and (min-width: 500px) {
  div.laurels {
    bottom: 40px;
    left: 10px;
    width: auto;
  }
}

@media (max-width: 750px) and (max-height: 650px) {
  div.laurels {
    display:none;
  }

}

/** this prevents clicking on the scene accidentally in the surrounding of the XR buttons */
div.touch-blocker {
  position:absolute;
  bottom: 0;
  left: calc(50% - 350px);
  width: 700px;
  height: 180px;
  z-index: 140;
}

.vignette {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  box-shadow: 0 0 30vw rgb(255 255 255 / 70%) inset;
  pointer-events: none;
  z-index: 1000;
}

div.menu {
    position: absolute;
    top: 40%;
    left: 50%;
    z-index: 1000;
    transform: translate(-50%, -50%);
    pointer-events: none;
    font-size: 2em;
    letter-spacing: 2px;
    text-align: center;
    text-transform: uppercase;
    line-height: 1.4em;
    font-weight: bold;
    text-shadow: 10px 20px 50px rgb(69, 107, 139);
    color: grey;
    transition: opacity 0.4s linear;
    width: 90vw;
}

div.anim {

}

div.menu p {
    transition: opacity 0.5s ease-in-out;
    margin:0;
    font-weight: bold;
    color: rgb(69, 107, 139);
}

div.menu p.subtitle {
    font-size: 1em;
    font-weight: normal;
}

.hidden {
    opacity: 0;
}

.laurels, .logo {
  pointer-events: none;
}

.menu img {
  width: 4em;
  animation: swipe 3.3s infinite;
}

@keyframes swipe {
  0% {
    transform: translateX(-40px) rotate(-20deg);
  }
  50% {
    transform: translateX(30px) rotate(20deg) scale(1.1);
  }
  100% {
    transform: translateX(-40px) rotate(-20deg);
  }
}

@keyframes pulse {
  0% {
    opacity: 0.2;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0.2;
  }
}

@media (max-height: 500px) {
  div.menu {
    font-size: 1.3em;
  }
}

@media (max-width: 500px) {
  div.menu {
    font-size: 1.3em;
  }

  div.logo {
    flex-direction: column;
    align-items: flex-start;
  }

  div.logo img {
    width: auto;
    height: min(300px, 50vw);
  }

  div.logo img.laurels {
    margin-top: -60px;
  }
}

@media (min-width: 800px) and (min-height: 500px) {
  div.logo img {
    width: auto;
    height: min(300px, 50vw);  
  }
}
</style>