<script>

  export let timelinefill;

  let markers = [
    { name: 'Jong', cheeseweek: 4},
    { name: 'Jong Belegen', cheeseweek: 8},
    { name: 'Belegen', cheeseweek: 10},
    { name: 'Extra Belegen', cheeseweek: 28},
    { name: 'Oud', cheeseweek: 40},
    { name: 'Overjarig', cheeseweek: 48},
    { name: 'Brokkelkaas', cheeseweek: 96},
  ];

</script>

<div class="timeline-container">
  <div class="timeline-markers">
    {#each markers as marker, index}
      <div class="timeline-marker timeline-marker-{ index }" style="left: calc({ marker.cheeseweek / 96 * 100 }% )">
        <span class="timeline-marker-name">{ marker.name }</span>
      </div>
    {/each}
  </div>
  <div class="timeline">
    <div class="timeline-fill" style="width: {timelinefill}%"></div>
  </div>
</div>

<style>
  .timeline-container {
    position: relative;
    font-size: clamp(9px, 2.5vw, .9rem);
    font-family: var(--font-display);
    /* font-style: normal; */
    margin-bottom: .66rem;
  }
  .timeline {
    position: relative;
    height: 1.5em;
    /* background-color: var(--color-yellow); */
    border: 2px solid var(--color-brown);
    border-radius: 3px;
  }
  .timeline-fill {
    position: absolute;
    top: 1px;
    left: 1px;
    width: 0;
    max-width: calc(100% - 2px);
    height: calc(100% - 2px);
    background-color: var(--color-yellow);
    transition: width .2s linear;
  }
  .timeline-markers {
    position: relative;
    top: 1.5em;
    height: 6em;
    left: 2px;
    width: calc(100% - 6px);
    z-index: 32;
  }
  .timeline-marker:not(:last-child) {
    position: absolute;
    bottom: 0%;
    left: 0px;
    width: 1px;
    height: 2em;
    background-color: var(--color-brown);
  }
  .timeline-marker:nth-child(2) {
    height: 2em;
  }
  @media (max-width: 1200px) {
    .timeline-marker:nth-child(1) {
      height: 3em;
    }
    .timeline-marker:nth-child(2) {
      height: 2.75em;
    }
  }
  
  .timeline-marker:not(:last-child) .timeline-marker-name {
    position: absolute;
    left: 0;
    bottom: 100%;
    white-space: nowrap;
    z-index: 2;
    transform-origin: 0 100%;
    transform: translateX(.5em) rotate(-33deg);
  }
  .timeline-marker:last-child {
    position: absolute;
    bottom: 0%;
    width: 1px;
    height: 2em;
    background-color: var(--color-brown);
  }
  .timeline-marker:last-child .timeline-marker-name {
    position: absolute;
    bottom: 100%;
    white-space: nowrap;
    z-index: 2;
    left: auto;
    right: 0;
  }
</style>