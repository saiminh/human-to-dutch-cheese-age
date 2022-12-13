<slot></slot>

<style>
  :global(:root){
    --color-yellow: rgb(255, 207, 14);
    --color-brown: rgb(73, 41, 5);
  }
  :global(body) {
    margin: 0;
    padding: 0;
    font-family:  gopher, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: clamp(14px, 4vw, 20px);
    background-color: rgb(255, 207, 14);
    box-sizing: border-box;
  }
  :global(body *) {
    box-sizing: inherit;
  }
  :global(*) {
    touch-action: manipulation;
  }
  :global(input) {
    font-family:  gopher, sans-serif;
  }
  :global(p + h1) {
    margin-top: 0;
    font-weight: 900;
    font-family:  gopher, sans-serif;
  }
  :global(p) {
    max-width: 32em;
  }
  :global(p:first-child) {
    margin-top: 0;
  }
</style>