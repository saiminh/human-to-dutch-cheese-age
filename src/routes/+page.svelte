<script>
import { onMount } from 'svelte';
import CheeseageCalculator from "./CheeseageCalculator.svelte";
import TimeLine from "./TimeLine.svelte";
import { cheeseAgeResults } from './contentCheeseAgeResults.js';
import Logo from './Logo.svelte';

  let humanage = 0;
  let focused = false;
  $: cheeseweeks = humanage * 0.88;
  $: cheeseage =  !cheeseweeks ? 'not-yet' :
                  cheeseweeks < 4 ? 'not-yet' : 
                  cheeseweeks <= 8 ? 'jong' : 
                  cheeseweeks <= 10 ? 'jong-belegen' : 
                  cheeseweeks <= 28 ? 'belegen' : 
                  cheeseweeks <= 40 ? 'extra-belegen' : 
                  cheeseweeks <= 48 ? 'oud' : 
                  cheeseweeks <= 96 ? 'overjarig' :
                  'brokkelkaas';

  
  $: percentage = cheeseweeks / 96 * 100 ;
  let fullResultIsOpen = false;
  function toggleFullResultKeyboard(e){
    if(e.keyCode == 27){
      toggleFullResult()
    }
  }
  function toggleFullResult(){
    fullResultIsOpen = !fullResultIsOpen
  }

  onMount(async () => {
    document.body.addEventListener('click', (e)=>{
      let x = e.clientX;
      let y = e.clientY;
      let splash = document.createElement('div');
      splash.classList.add('splash');
      splash.style.left = x - 25 + 'px';
      splash.style.top = y - 25 + 'px';
      document.body.appendChild(splash);
      setTimeout(()=>{
        splash.remove();
      }, 600)
    });
  });
</script>

<div class="page-wrapper {fullResultIsOpen ? 'result-full-open' : ''}">
  
  <div class="page-content--timeline">
    <h2 class="age-intro">Ever wonder what <strong>Dutch Cheese Age</strong> you are with your <CheeseageCalculator bind:humanage={humanage} bind:focused={focused} /> human years?  Well, you are:</h2>
    <TimeLine bind:timelinefill={percentage} />
  </div>
  
  <div class="page-content--results">
    <div class="cheeseage-results">
      {#each cheeseAgeResults as { slug, title, buttontext }, i}
        <div class="cheeseage-result cheeseage-result-{i} {slug == cheeseage ? 'current' : ''}">
          <h2 class="cheeseage-result-title">{title.original}<span class="translation">({ title.english })</span></h2>
          <button class="cheeseage-result-morebtn" on:click={toggleFullResult}>{buttontext}</button>
        </div>
      {/each}
    </div>
  </div>

</div>

<div class="cheeseage-results-full {fullResultIsOpen ? 'open' : ''}">
  {#each cheeseAgeResults as { slug, title, description, image }, i}
    <div class="cheeseage-result-full cheeseage-result-full-{i} {slug == cheeseage ? 'current' : ''} {fullResultIsOpen ? 'open' : ''}">
      <h2 class="cheeseage-result-full-title">{title.original}<span class="translation">({ title.english })</span></h2>
      {#if image}
        <img src={image} alt={title.original} />
      {/if}
      <div class="cheeseage-result-description">
        {@html description}
      </div>
      <button class="cheeseage-result-full-closebtn" on:click={toggleFullResult}>Close</button>
    </div>
  {/each}
</div>


<div class="logo {focused ? 'hide-on-touch' : ''}">
  <Logo />
</div>

<style>
  @media (pointer: coarse){
    .hide-on-touch {
      display: none;
    }
  }
  :global(.splash){
    position: fixed;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    border: 12px solid var(--color-brown);
    animation: splash .5s ease-out forwards;
    pointer-events: none;
  }
  @keyframes splash {
    0% {
      transform: scale(0);
      opacity: 1;
      border-color: var(--color-brown);
    }
    100% {
      transform: scale(1);
      opacity: 0;
      border-color: var(--color-yellow);
    }
  }
  .page-wrapper {
    padding: 0 5vw;
    margin: .5rem;
    position: relative;
    overflow-x: visible;
    overflow-y: scroll;
    min-height: calc(100svh - 1rem);
    display: flex;
    flex-direction: column;
    perspective: 1000px;
    transform-style: preserve-3d;
    border: 5px solid;
    /* border-radius: 10px; */
    background-color: #FCF4E9;
  }
  .page-content--header {
    flex: 15% 0 1;  
  }
  .page-content--timeline {
    flex: 15% 0 1;
  }
  .page-content--results {
    flex: 70% 1 1;
  }
  .page-content--header , 
  .page-content--timeline ,
  .page-content--results {
    position: relative;
    transform-origin: 50% 50%;
    transform: translate3d(0, 0, 0) rotateX(0deg);
    transition: transform .3s ease-out;
  }
  .result-full-open .page-content--header , 
  .result-full-open .page-content--timeline ,
  .result-full-open .page-content--results {
    transform: translate3d(0, 0, -100px) rotateX(10deg);
    transition: transform .2s ease-in-out;
  }
  .cheeseage-results {
    position: absolute;
    width: 100%;
    height: calc(100% - 1.5rem);
  } 
  .cheeseage-result {
    position: absolute;
    opacity: 0;
    visibility: hidden;
    width: 100%;
    height: 100%;
  }
  .cheeseage-result-title {
    text-align: center;
    font-style: normal;
    font-weight: 800;
    font-size: 12vw;
    margin: .25em 0 0 0;
    letter-spacing: -0.02em;
    text-transform: lowercase;
  }
  .cheeseage-result-1 .cheeseage-result-title ,
  .cheeseage-result-5 .cheeseage-result-title {
    font-size: 11vw;
  }
  .cheeseage-result-7 .cheeseage-result-title {
    font-size: 15vw;
  }
  .cheeseage-result-6 .cheeseage-result-title ,
  .cheeseage-result-4 .cheeseage-result-title ,
  .cheeseage-result-2 .cheeseage-result-title {
    font-size: 18vw;
  }
  @media (min-width: 600px) {
    .cheeseage-result-7 .cheeseage-result-title ,
    .cheeseage-result-6 .cheeseage-result-title ,
    .cheeseage-result-4 .cheeseage-result-title ,
    .cheeseage-result-2 .cheeseage-result-title {
      font-size: 13vw;
    }
  }
  .cheeseage-result .translation {
    display: block;
    font-size: .5em;
    font-weight: 400;
    font-style: italic;
    letter-spacing: 0;
  }
  @media (min-width: 600px) {
    .cheeseage-result .translation {
      font-size: .25em;
    }
  }
  .cheeseage-result-morebtn,
  .cheeseage-result-full-closebtn {
    background-color: var(--color-yellow);
    color: var(--color-brown);
    display: block;
    padding: .7em 1.75em;
    margin: 0 auto;
    border: 1px solid;
    border-radius: 40px;
    font-family: var(--font-display);
    font-size: 4vw;
    font-weight: 500;
    letter-spacing: -.0125em;
    cursor: pointer;
    display: block;
    margin: 1em auto;
  }
  .cheeseage-result-full-closebtn {
    width: 100%;
    margin-top: 1.5em;
    font-size: 5.5vw;
  }
  @media (min-width: 600px) {
    .cheeseage-result-morebtn,
    .cheeseage-result-full-closebtn {
      font-size: 2vw;
      width: auto;
      min-width: 200px;
    }
    .cheeseage-result-full-closebtn {
      position: absolute;
      top: 0rem;
      right: 2.5rem;
    }
  }
  .cheeseage-results-full {
    position: fixed;
    width: 100vw;
    height: 100vh;
    top:0;
    left: 0;
    overflow: hidden;
    perspective: 1000px;
    transform-style: preserve-3d;
    pointer-events: none;
  }
  .cheeseage-results-full.open {
    pointer-events: auto;
  }
  .cheeseage-result-full{
    position: fixed;
    opacity: 0;
    visibility: hidden;
    top: 1rem;
    left: 1rem;
    width: calc(100% - 2rem);
    height: calc(100vh - 2rem);
    height: calc(100svh - 2rem);
    border: 2px solid;
    background-color: var(--color-yellow);
    /* border-radius: 5px; */
    /* box-shadow: 8px 8px 0 0 var(--color-brown); */
    padding: 1rem;
    z-index: 40000;
    overflow-x: hidden;
    overflow-y: scroll;
    transform-origin: 50% 100%;
    transform: rotateX(-25deg) translate3d(0, 100vh, 0);
    transition: transform .4s cubic-bezier(.36,.29,.26,1);
  }
  @media (min-width: 600px) {
    .cheeseage-result-full {
      padding: 2rem;
    }
  }
  .cheeseage-result-full.current {
    opacity: 1;
    visibility: visible;  
  }
  .cheeseage-result-full.current.open {
    opacity: 1;
    visibility: visible;
    transform: rotateX(0deg) translate3d(0, 0, 0);
    /* animation: result-zoom-in .4s cubic-bezier(.36,.29,.46,1.13) forwards; */
  }
  /* @keyframes result-zoom-in {
    0% {
      transform: rotateX(-10deg) translate3d(0, 300px, 0);
    }
    100% {
      transform: rotateX(0deg) translate3d(0, 0, 0);
    }
  } */
  .cheeseage-result-full h2 {
    text-align: center;
    padding-bottom: .25em;
    border-bottom: 1px solid;
    font-style: normal;
    font-weight: 800;
    font-size: 10vw;
    letter-spacing: -0.02em;
    margin-top: 0;
    margin-bottom: .5em;
    text-transform: lowercase;  
  }
  .cheeseage-result-full h2 span {
    /* font-family: 'Times New Roman', Times, serif; */
    font-style: italic;
    font-weight: 500;
    font-size: .5em;
    letter-spacing: -0.02em;
    display: block;
  }
  .cheeseage-result-full img {
    width: 50svh;
    height: 50svh;
    max-width: 460px;
    display: block;
    margin: 1rem auto;
    filter: invert(10%) sepia(10%) saturate(4629%) hue-rotate(352deg) brightness(95%) contrast(95%);
  }
  @media (min-width: 800px) {
    .cheeseage-result-full img {
      float: left;
      width: calc(50% - 1em);
      height: auto;
      margin-right: 1em;
    }
    .cheeseage-result-full img + .cheeseage-result-description {
      width: 50%;
      float: left;
    }
  }
  .cheeseage-result-description {
    max-width: 900px;
    margin: 0 auto;
    font-family: 'Times New Roman', Times, serif;
    font-style: italic;
    font-size: 5vw;
  }
  @media (min-width: 600px) {
    .cheeseage-result-full h2 {
      font-size: 6vw;
    }
    .cheeseage-result-description {
      font-size: clamp(16px, 2.5vw, 1.75rem);
    }
  }
  :global(.cheeseage-result-description > p:last-child) {
    margin-bottom: 0;
  }
  .cheeseage-result.current {
    opacity: 1;
    visibility: visible;
  }
  .cheeseage-result.current > * {
    transform-origin: 50% 50%;
  }
  .cheeseage-result.current {
    animation:  cheeseage-result-slide-in .6s cubic-bezier(.7,.08,.34,.98) forwards, 
                cheeseage-result-fade-in .4s ease-out forwards;
              }
  @keyframes cheeseage-result-slide-in {
    0%   { transform: translateY(30px) scale(1); }
    100% { transform: translateY(0) scale(1); }
  }
  @keyframes cheeseage-result-fade-in {
    0%   { opacity: 0; }
    100% { opacity: 1; }
  }
  
  .page-title {
    margin: 0 auto .5em auto;
    padding: .75em 0 0 0;
    font-size: 5.5vw;
    font-weight: 400;
    font-style: italic;
    line-height: 1.5;
    letter-spacing: -.02em;
    text-align: center;
    color: var(--color-brown);
  }
  .page-title span {
    font-weight: 800;
    font-style: normal;
    display: block;
    font-size: 1.5em;
    line-height: 1;
  }
  @media (min-width: 600px) {
    .page-title {
      font-size: clamp(24px, 3vw, 5rem);
      line-height: 1;
    }
    .page-title span {
      font-size: 1em;
      display: inline;
    }
  }
  .age-intro {
    color: var(--color-brown);
    text-align: center;
    margin: 1em 0 0 0;
    line-height: 1.2;
    font-family: var(--font-display);
    font-weight: 500;
    font-style: italic;
    font-size: 5.5vw;
  }
  .age-intro strong {
    font-weight: 900;
    font-style: normal;
  }
  @media (min-width: 600px) {
    .age-intro {
      font-size: clamp(16px, 2vw, 4rem);
    }
  }
  
</style>