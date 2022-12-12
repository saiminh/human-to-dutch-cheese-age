<script>
  import { each } from "svelte/internal";
import CheeseageCalculator from "./CheeseageCalculator.svelte";
import TimeLine from "./TimeLine.svelte";

  let humanage;
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

  $: cheeseAgeResults =  [ {
                    slug: 'none',
                    title: {
                      original: 'Tell us your age...',
                      english: 'Tell us your age...'
                    },
                    description: '<p>... then we will calculate your Dutch Cheese Age. </p><p><strong>Why is that useful information?</strong> Well, your dutch cheese age provides a different perspective on the concept of ageing. It is reassuring to learn that while any age is considered to have it\s own benefits, the more hours and days you let yourself mature, you will always be better and more valueable for it.</p>'
                  } , {
                    slug: 'not-yet',
                    title: {
                      original: 'Not a cheese yet!',
                      english: 'Not a cheese yet!'
                    },
                    description: '<p>But you\'re still a baby, so we\'ll give you a pass.</p><p>Basically at this stage, you are still just a metaphorical sludge of milk, acid and bacteria trying to negotiate your future. The better regulated the environment you are in, the higher your chances of becoming flavourful and fragrant. What happens in this phase is hugely important for the future, but at this stage, you really don\'t have anything to give yet.</p>'
                  } , {
                    slug: 'jong',
                    title: {
                      original: 'Jong',
                      english: 'Young'
                    },
                    description: '<p>Young, fresh, and full of potential.</p><p>For some people, this is the only cheese they\'ll ever be able to enjoy. Pure, creamy and innocent, in this phase of life you are easy to enjoy, if maybe a little limited and unoriginal.</p> <p>You melt easily. And when you do melt you are a bit rubbery.</p>'
                  } , {
                    slug: 'jong-belegen',
                    title: {
                      original: 'Jong belegen',
                      english: 'Matured young'
                    },
                    description: '<p>This is the shortest age of all. By definition an in-between state. You are neither here nor there. Not young and innocent anymore, but too green to have made a major dent anywhere in life either. But don\'t worry it only lasts 2 years...</p>'
                  } , {
                    slug: 'belegen',
                    title: {
                      original: 'Belegen',
                      english: 'Matured'
                    },
                    description: '<p>You have reached a really nice and balanced ratio between water, salt and eggwhites.</p><p>Slowly, the finer details of what makes life exciting become clear. You look back at your earlier years and realize they were merely a lead-up to this. But you also realize there\'s still so much more potential. Don\'t stop now. Don\'t take yourself too serious. You\'ve got a long way to go.</p>'
                  } , {
                    slug: 'extra-belegen',
                    title: {
                      original: 'Extra Belegen',
                      english: 'Extra Matured'
                    },
                    description: '<p>In this phase you realize that your appearance is slowly loosing some of its glow and sheen, BUT it\'s totally worth it as while you keep loosing water, you gain flavour. It\'s an exciting process during which you develop more and more finesse, sass and grace while also catching a first glimpse of your own mortality on the horizont.</p>'
                  } , {
                    slug: 'oud',
                    title: {
                      original: 'Oud',
                      english: 'Old'
                    },
                    description: '<p>In the Netherlands, people love to state facts without considering anyone\'s feelings. Calling someone old is not an offence here, it\'s just a reference to the fact you\'ve been around for a while.</p><p>By now, you have earned status and respect.</p><p>People that bring you to parties proudly introduce you by pointing out where you\'re from and which circumstances of your life are responsible for the way you smell.</p>'
                  } , {
                    slug: 'overjarig',
                    title: {
                      original: 'Overjarig',
                      english: 'Perennial'
                    },
                    description: '<p>You are now a top-shelf cheese. You have hardened quite a bit, oh yes. But salt crystals are also flamboyantly sparkling on your skin and proclaim you are strange, fragrant and full of flavour. And you are ready.</p><p>Enjoy and celebrate yourself. The next (and ultimate) step is purely optional.</p>'
                  } , {
                    slug: 'brokkelkaas', 
                    title: {
                      original: 'Brokkelkaas',
                      english: 'Crumble Cheese'
                    },
                    description: '<p>You are very dry and very salty now.</p><p>While your body cracks and parts fall off spontaneously, you have reached peak flavour. There\'s very few like you, but the impact you can have with a small contribution is immense. There\'s certainly no way anyone is going to try and slice you now.</p>'
                  } ];
  $: percentage = cheeseweeks / 96 * 100 ;
</script>

<div class="page-wrapper">
  
  <div class="cheeseage-app">
    <p>How old are you?</p>
    <CheeseageCalculator bind:humanage={humanage} />
    
    <div class="cheeseage-results">
      <TimeLine bind:timelinefill={percentage} />
      <p>{humanage ? 'Your Dutch Cheese Age is:' : ''}</p> 
      {#each cheeseAgeResults as { slug, title, description }, i}
        <div class="cheeseage-result cheeseage-result-{i} {slug == cheeseage ? 'current' : ''}">
          <h1>{title.original}<span class="translation">{ cheeseweeks >= 4 ? ' ('+title.english+')' : ''}</span></h1>
          <div class="cheeseage-result-description">
            {@html description}
          </div>
        </div>
      {/each}
    </div>
  
  </div>

</div>


<style>
  .page-wrapper {
    padding: 1.5rem;
    margin: .5rem;
    border: 4px solid var(--color-brown);
    border-radius: 5px;
    position: relative;
    width: calc( 100vw - 1rem );
    height: calc( 100vh - 1rem );
    overflow-y: scroll;
    overflow-x: hidden;
  }
  .cheeseage-app {
    width: 100%;
    margin: auto;
    /* display: flex;
    flex-direction: column; */
    align-items: flex-start;
  }
  .cheeseage-results {
    /* height: 100vh; */
    /* flex-basis: 75%;
    overflow-y: scroll;
    overflow-x: hidden; */
    position: relative;
    
  } 
  .cheeseage-result {
    position: absolute;
    opacity: 0;
    visibility: hidden;
    /* background-color: var(--color-brown);
    color: var(--color-yellow); */
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
    0% {
      transform: translateX(30px) scale(1);
    }
    100% {
      transform: translateX(0) scale(1);
    }
  }
  @keyframes cheeseage-result-fade-in {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }
  h1 {
    margin-top: 0;
    margin-bottom: .3em;
    font-size: clamp(20px, 10vw, 5em);
    font-weight: 600;
    line-height: 1;
    font-family:  gopher, sans-serif;
    letter-spacing: -.02em;
  }
  .translation {
    font-size: .5em;
    font-weight: 400;
    font-family:  gopher, sans-serif;
    letter-spacing: -.02em;
    margin-left: .5em;
  }
</style>