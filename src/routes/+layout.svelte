<slot></slot>

<style>
  :global(:root){
    --color-yellow: rgb(255, 191, 14);
    --color-brown: rgb(48, 28, 9);
    --font-copy : quiche-text, serif;
    --font-display : scotch-display, serif;
  }
  :global(body) {
    margin: 0;
    padding: 0;
    font-family:  var(--font-copy);
    font-style: italic;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: clamp(14px, 4vw, 24px);
    background-color: white;
    color: var(--color-brown);
    box-sizing: border-box;
  }
  :global(h1, h2, h3, h4) {
    font-family: var(--font-display);   
  }
  :global(body *) {
    box-sizing: inherit;
  }
  :global(*) {
    touch-action: manipulation;
  }
  :global(input) {
    font-family:  scotch-display, sans-serif;
  }
  :global(p + h1) {
    margin-top: 0;
  }
</style>