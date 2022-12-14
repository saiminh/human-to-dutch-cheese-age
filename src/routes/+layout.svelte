<slot></slot>

<style>
  :global(:root){
    --color-yellow: rgb(255, 191, 14);
    --color-brown: rgb(107, 77, 62);
  }
  :global(body) {
    margin: 0;
    padding: 0;
    font-family:  gopher, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: clamp(14px, 4vw, 24px);
    background-color: var(--color-yellow);
    color: var(--color-brown);
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
</style>