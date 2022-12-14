<script>
  import { each } from "svelte/internal";
import CheeseageCalculator from "./CheeseageCalculator.svelte";
import TimeLine from "./TimeLine.svelte";

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

  let cheeseAgeResults =  [ {
                    slug: 'none',
                    title: {
                      original: 'Tell us your age...',
                      english: 'Tell us your age...'
                    },
                    description: '<p>... then we will calculate your Dutch Cheese Age. </p><p><strong>Why is that useful information?</strong> Well, your dutch cheese age provides a different perspective on the concept of ageing. It is reassuring to learn that while any age is considered to have it\s own benefits, the more hours and days you let yourself mature, you will always be better and more valueable for it.</p>',
                    buttontext: 'Why would I do that?'
                  } , {
                    slug: 'not-yet',
                    title: {
                      original: 'Not a cheese yet!',
                      english: 'Not a cheese yet!'
                    },
                    description: '<p>But you\'re still a baby, so we\'ll give you a pass.</p><p>Basically at this stage, you are still just a metaphorical sludge of milk, acid and bacteria trying to negotiate your future. The better regulated the environment you are in, the higher your chances of becoming flavourful and fragrant. What happens in this phase is hugely important for the future, but at this stage, you really don\'t have anything to give yet.</p>',
                    buttontext: 'Then what am I?'
                  } , {
                    slug: 'jong',
                    title: {
                      original: 'Jong',
                      english: 'Young'
                    },
                    description: '<p>Young, fresh, and full of potential.</p><p>For some people, this is the only cheese they\'ll ever be able to enjoy. Pure, creamy and innocent, in this phase of life you are easy to enjoy, if maybe a little limited and unoriginal.</p> <p>You melt easily. And when you do melt you are a bit rubbery.</p>',
                    buttontext: 'Tell me more',
                    image: 'illu-jong.png'
                  } , {
                    slug: 'jong-belegen',
                    title: {
                      original: 'Jong belegen',
                      english: 'Matured young'
                    },
                    description: '<p>This is the shortest age of all. By definition an in-between state. You are neither here nor there. Not young and innocent anymore, but too green to have made a major dent anywhere in life either. But don\'t worry it only lasts 2 years...</p>',
                    buttontext: 'Tell me more'
                  } , {
                    slug: 'belegen',
                    title: {
                      original: 'Belegen',
                      english: 'Matured'
                    },
                    description: '<p>You have reached a really nice and balanced ratio between water, salt and eggwhites.</p><p>Slowly, the finer details of what makes life exciting become clear. You look back at your earlier years and realize they were merely a lead-up to this. But you also realize there\'s still so much more potential. Don\'t stop now. Don\'t take yourself too serious. You\'ve got a long way to go.</p>',
                    buttontext: 'Tell me more'
                  } , {
                    slug: 'extra-belegen',
                    title: {
                      original: 'Extra Belegen',
                      english: 'Extra Matured'
                    },
                    description: '<p>In this phase you realize that your appearance is slowly loosing some of its glow and sheen, BUT it\'s totally worth it as while you keep loosing water, you gain flavour. It\'s an exciting process during which you develop more and more finesse, sass and grace while also catching a first glimpse of your own mortality on the horizont.</p>',
                    buttontext: 'Tell me more'
                  } , {
                    slug: 'oud',
                    title: {
                      original: 'Oud',
                      english: 'Old'
                    },
                    description: '<p>In the Netherlands, people love to state facts without considering anyone\'s feelings. Calling someone old is not an offence here, it\'s just a reference to the fact you\'ve been around for a while.</p><p>By now, you have earned status and respect.</p><p>People that bring you to parties proudly introduce you by pointing out where you\'re from and which circumstances of your life are responsible for the way you smell.</p>',
                    buttontext: 'Tell me more'
                  } , {
                    slug: 'overjarig',
                    title: {
                      original: 'Overjarig',
                      english: 'Perennial'
                    },
                    description: '<p>You are now a top-shelf cheese. You have hardened quite a bit, oh yes. But salt crystals are also flamboyantly sparkling on your skin and proclaim you are strange, fragrant and full of flavour. And you are ready.</p><p>Enjoy and celebrate yourself. The next (and ultimate) step is purely optional.</p>',
                    buttontext: 'Tell me more'
                  } , {
                    slug: 'brokkelkaas', 
                    title: {
                      original: 'Brokkelkaas',
                      english: 'Crumble Cheese'
                    },
                    description: '<p>You are very dry and very salty now.</p><p>While your body cracks and parts fall off spontaneously, you have reached peak flavour. There\'s very few like you, but the impact you can have with a small contribution is immense. There\'s certainly no way anyone is going to try and slice you now.</p>',
                    buttontext: 'Tell me more'
                  } ];
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
    
    {#each cheeseAgeResults as { slug, title, description, image }, i}
      <div 
        class="cheeseage-result-full cheeseage-result-full-{i} {slug == cheeseage ? 'current' : ''} {fullResultIsOpen ? 'open' : ''}"
        on:click={toggleFullResult}
        on:keydown={toggleFullResultKeyboard}
      >
        <h1>{title.original}<span class="translation">{ cheeseweeks >= 4 ? ' ('+title.english+')' : ''}</span></h1>
        {#if image}
          <img src={image} alt={title.original} />
        {/if}
        <div class="cheeseage-result-description">
          {@html description}
        </div>
      </div>
    {/each}
</div>


<style>
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
    transform: translate3d(0, 0, 0);
    transition: transform .3s ease-out;
  }
  .result-full-open .page-content--counter , 
  .result-full-open .page-content--timeline ,
  .result-full-open .page-content--results {
    transform: translate3d(0, 0, -100px);
  }
  @keyframes page-zoom-in {
    0% {
      transform: translate3d(0, 300px, 0px);
    }
    50% {
      transform: translate3d(0, 150px, -100px);
    }
    100% {
      transform: translate3d(0, 0, 0);
    }
  }
  .page-content--results > p {
    margin-bottom: .5em;
  }
  .cheeseage-results {
    position: relative;
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
  }
  .cheeseage-result-full:after {
    content: "\00d7";
    font-family: 'Times New Roman', Times, serif;
    position: absolute;
    top: 1rem;
    right: 1rem;
    line-height: 0.5;
    font-size: 4rem;
    color: var(--color-brown);
    cursor: pointer;
  }
  .cheeseage-result-full.current.open {
    opacity: 1;
    visibility: visible;
  }
  .cheeseage-result-full.current.open {
    opacity: 1;
    visibility: visible;
    transform-origin: 50% 100%;
    animation: result-zoom-in .6s cubic-bezier(.36,.29,.46,1.13) forwards;
  }
  @keyframes result-zoom-in {
    0% {
      transform: rotateX(-10deg) translate3d(0, 300px, 0px);
    }
    /* 50% {
      transform: translate3d(0, 150px, 50px);
    } */
    100% {
      transform: rotateX(0deg) translate3d(0, 0, 0);
    }
  }
  .cheeseage-result-full h1 {
    text-align: left;
    padding-right: 1.5em;
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
    max-width: 250px;
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