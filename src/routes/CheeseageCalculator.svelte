<script>
  export let humanage;
  let input;
  
  function handlePlusBtnClick() {
    if (!humanage) {
      humanage = 1;
    }
    else if (humanage < 110) {
      humanage = humanage + 1;
    }
  }
  function handlePlusBtnHold() {
    let interval = setInterval(handlePlusBtnClick, 200);
    this.addEventListener('mouseup', () => clearInterval(interval));
    this.addEventListener('touchend', () => clearInterval(interval));
  }
  function handleMinBtnClick() {
    if (humanage > 0) {
      humanage = humanage - 1;
    }
  }
  function handleMinBtnHold() {
    let interval = setInterval(handleMinBtnClick, 200);
    this.addEventListener('mouseup', () => clearInterval(interval));
    this.addEventListener('touchend', () => clearInterval(interval));
  }
  function handleInputFocus() {
    input.select();
  }
</script>

  <div class="cheeseage-calculator">
    <div class="ageinput">
      <span class="description description-pre">I am</span>
      <input type="number" name="age" bind:value={humanage} bind:this={input} on:click={() => handleInputFocus()} min="0" max="110" pattern="[0-9]*">
      <span class="description description-post">Human Year{ humanage == 1 ? '' : 's'} Old</span>
      <div class="yearbuttons">
        <button on:click={handlePlusBtnClick} on:mousedown={handlePlusBtnHold} on:touchstart={handlePlusBtnHold}><span>+</span></button>
        <button on:click={handleMinBtnClick} on:mousedown={handleMinBtnHold} on:touchstart={handleMinBtnHold}><span>-</span></button>
      </div>
    </div>
  </div>
  
  

<style>
  .cheeseage-calculator {
    --input-height: 125px;
    --input-width: 150px;
    font-size: 1.75rem;
    background-color: var(--color-brown);
    color: var(--color-yellow);
    width: calc(var(--input-width) + var(--input-height) / 2);
    margin: 0 auto;
    border-radius: 10px;
  }
  .ageinput {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0;
    position: relative;
  } 
  input[type=number] {
    width: var(--input-width);
    height: var(--input-height);
    line-height: 1;
    padding: 0 0 0 0;
    display: inline;
    font-size: calc(var(--input-height) / 2);
    text-align: center;
    border-radius: 10px 0 0 10px;
    color: var(--color-yellow);
    border: 2px solid;
    border-width: 0 1px 0 0;
    background-color: transparent;
    transition: all .3s ease-in-out;
  }
  input[type=number]::selection {
    background-color: var(--color-brown);
    color: var(--color-yellow);
  }
  input[type=number]:focus {
    outline: none;
    /* background-color: rgba(73, 41, 5, .2);   */
  }
  input[type="number"] {
    -webkit-appearance: textfield;
       -moz-appearance: textfield;
            appearance: textfield;
  }
  input[type=number]::-webkit-inner-spin-button, 
  input[type=number]::-webkit-outer-spin-button { 
    -webkit-appearance: none;
  }
  .description {
    position: absolute;
    font-size: calc(var(--input-height) / 9);
    font-weight: bold;
    bottom: .75em;
    left: calc( var(--input-width) / 2 );
    transform: translate(-50%, 0);
  }
  .description-pre {
    bottom: auto;
    top: .75em;
  }
  .yearbuttons {
    display: flex;
    flex-direction: column;
    gap: 0;
    margin: 0;
    vertical-align: bottom;
  }
  button {
    height: calc(var(--input-height) / 2);
    width: calc(var(--input-height) / 2);
    line-height: calc(var(--input-height) / 2);
    display: block;
    font-size: 1em;
    cursor: pointer;
    transition: all .3s ease-out;
    background-color: transparent;
    position: relative;
    text-align: center;
    background-color: transparent;
    color: var(--color-yellow);
    border: 2px solid;
    border-radius: 0 10px 0px 0;
    border-width: 0 0 1px 1px;
    background-color: rgba(73, 41, 5, 0);
    padding: 0;
    -webkit-user-select: none; /* Safari */
    -ms-user-select: none; /* IE 10 and IE 11 */
    user-select: none; /* Standard syntax */
  }
  button:last-child {
    border-radius: 0 0 10px 0;
    border-width: 1px 0 0 1px;
  }
  button span {
    font-weight: 400;
    position: relative;
    top: -.125em;
    -webkit-user-select: none; /* Safari */
    -ms-user-select: none; /* IE 10 and IE 11 */
    user-select: none; /* Standard syntax */
  }
  button:active {
    background-color: rgba(73, 41, 5, .2);
  }
  button:active span {
    font-weight: 600;
  }
  
  
</style>