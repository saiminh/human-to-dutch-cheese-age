<script>
import { onMount } from 'svelte';
import CheeseageCalculator from "./CheeseageCalculator.svelte";
import TimeLine from "./TimeLine.svelte";
import { cheeseAgeResults } from './contentCheeseAgeResults.js';

  let humanage = 0;
  $: cheeseweeks = humanage * 0.88;
  $: cheeseage =  !cheeseweeks ? 'none' :
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
  
    <div class="page-content--counter">
      <h2>Check Your Cheese Age</h2>
      <CheeseageCalculator bind:humanage={humanage} />
    </div>

    <div class="page-content--timeline">
      <TimeLine bind:timelinefill={percentage} />
    </div>
    
    <div class="page-content--results">
      <p>{ humanage ? 'Your Dutch Cheese Age is:' : '' }</p>
      <div class="cheeseage-results">
        {#each cheeseAgeResults as { slug, title, buttontext }, i}
          <div class="cheeseage-result cheeseage-result-{i} {slug == cheeseage ? 'current' : ''}">
            <h1>{title.original}<span class="translation">{ cheeseweeks >= 4 ? ' ('+title.english+')' : ''}</span></h1>
            <button on:click={toggleFullResult}>{buttontext}</button>
          </div>
        {/each}
      </div>
    </div>
    <div class="cheeseage-results-full {fullResultIsOpen ? 'open' : ''}">
      {#each cheeseAgeResults as { slug, title, description, image }, i}
        <div 
          class="cheeseage-result-full cheeseage-result-full-{i} {slug == cheeseage ? 'current' : ''} {fullResultIsOpen ? 'open' : ''}"
          on:click={toggleFullResult}
          on:keydown={toggleFullResultKeyboard}
        >
        <h1>{title.original}<span class="translation">{ cheeseweeks >= 4 ? ' ('+title.english+')' : ''}</span></h1>
        <div class="cheeseage-result-description">
          {@html description}
        </div>
        {#if image}
          <img src={image} alt={title.original} />
        {/if}
        </div>
      {/each}
    </div>
</div>


<style>
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
    padding: 1.5rem;
    margin: 0 auto;
    position: relative;
    overflow-x: visible;
    min-height: 100svh;
    display: flex;
    flex-direction: column;
    max-width: 900px;
    perspective: 1000px;
    transform-style: preserve-3d;
  }
  .page-content--counter {
    flex: 15% 0 1;  
  }
  .page-content--timeline {
    flex: 15% 0 1;
  }
  .page-content--results {
    flex: 70% 1 1;
  }
  .page-content--counter , 
  .page-content--timeline ,
  .page-content--results {
    transform-origin: 50% 50%;
    transform: translate3d(0, 0, 0) rotateX(0deg);
    transition: transform .3s ease-out;
  }
  .result-full-open .page-content--counter , 
  .result-full-open .page-content--timeline ,
  .result-full-open .page-content--results {
    transform: translate3d(0, 0, -100px) rotateX(10deg);
    transition: transform .2s ease-in-out;
  }
  .page-content--results > p {
    margin-bottom: .5em;
  }
  .cheeseage-results {
    position: relative;
  } 
  .cheeseage-results-full {
    position: fixed;
    width: 100vw;
    max-width: 900px;
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
  .cheeseage-result-full {
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
    border-radius: 5px;
    box-shadow: 8px 8px 0 0 var(--color-brown);
    padding: 1rem;
    z-index: 40000;
    overflow-x: hidden;
    overflow-y: scroll;
    transform-origin: 50% 100%;
    transform: rotateX(-25deg) translate3d(0, 110%, 0);
    transition: transform .4s cubic-bezier(.36,.29,.26,1);
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
  @keyframes result-zoom-in {
    0% {
      transform: rotateX(-10deg) translate3d(0, 300px, 0);
    }
    100% {
      transform: rotateX(0deg) translate3d(0, 0, 0);
    }
  }
  .cheeseage-result-full h1 {
    text-align: left;
    padding-right: 1.5em;
    padding-top: 0;
    position: sticky;
    top: 0;
    /* background-color: var(--color-yellow); */
    background-image: linear-gradient(to bottom, var(--color-yellow) 70%, rgba(255, 191, 14, 0) 100%);
  }
  .cheeseage-result-full h1:after {
    content: "\00d7";
    font-family: 'Times New Roman', Times, serif;
    position: absolute;
    top: 1rem;
    right: 2rem;
    line-height: 0.5;
    font-size: 4rem;
    color: var(--color-brown);
    cursor: pointer;
  }
  
  .cheeseage-result {
    position: absolute;
    opacity: 0;
    visibility: hidden;
    width: 100%;
    border: 2px solid;
    border-radius: 5px;
    padding: 1rem;
  }
  .cheeseage-result-3 .translation ,
  .cheeseage-result-5 .translation {
    display: block;
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
  h1 {
    margin: -1rem -1rem 0 -1rem;
    padding: .45em;
    font-size: clamp(20px, 9vw, 5rem);
    font-weight: 600;
    line-height: 1;
    font-family:  gopher, sans-serif;
    letter-spacing: -.02em;
    text-align: center;
    color: var(--color-brown);
  }
  h2 {
    color: var(--color-brown);
    text-align: center;
    margin: 0 0 .5em 0;
    letter-spacing: -.02em;
    line-height: 1.2;
  }
  button {
    background-color: var(--color-brown);
    color: var(--color-yellow);
    display: block;
    padding: .75em 3em;
    margin: 0 auto;
    border: none;
    border-radius: 10px;
    font-size: 1.25rem;
    font-family: gopher, sans-serif;
    font-weight: bold;
    letter-spacing: -.0125em;
    cursor: pointer;
  }
  .translation {
    font-size: .75em;
    font-weight: 400;
    font-family:  gopher, sans-serif;
    letter-spacing: -.02em;
    margin-top: .25em;
    display: inline;
  }
  img {
    width: 100%;
    height: auto;
    max-width: 460px;
    display: block;
    margin: 1rem auto;
  }
  /* @media (min-width: 900px) {
    .page-wrapper { 
      display: grid;
      grid-template-columns: 1fr 2fr;
      grid-template-rows: 1fr 3fr;
    }
    .page-content--counter {
      grid-column: 1 / 1;
      grid-row: 1 / 4;
    }
    .page-content--timeline {
      grid-column: 2 / 2;
      grid-row: 1 / 1;
    }
    .page-content--results {
      grid-column: 2 / 2;
      grid-row: 2 / 3;
    }
    h1 {
       text-align: left;
    }
    h2 {
      text-align: left;
    }
  } */
</style>