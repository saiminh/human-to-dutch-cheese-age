<script>
  export let humanage;
  
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
  }
  function handleMinBtnClick() {
    if (humanage > 0) {
      humanage = humanage - 1;
    }
  }
  function handleMinBtnHold() {
    let interval = setInterval(handleMinBtnClick, 200);
    this.addEventListener('mouseup', () => clearInterval(interval));
  }
</script>

  <div class="cheeseage-calculator">
    <div class="ageinput">
      <input type="number" name="age" bind:value={humanage} min="0" max="110">
      <span class="description">years</span>
      <div class="yearbuttons">
        <button on:click={handlePlusBtnClick} on:mousedown={handlePlusBtnHold}><span>+</span></button>
        <button on:click={handleMinBtnClick} on:mousedown={handleMinBtnHold}><span>-</span></button>
      </div>
    </div>
  </div>
  
  

<style>
  .cheeseage-calculator {
    font-size: 1.75rem;
    color: rgb(73, 41, 5);
  }
  .ageinput {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0;
    position: relative;
  } 
  input[type=number] {
    width: 100%;
    height: 200px;
    line-height: 1;
    padding: 0;
    display: inline;
    font-size: 3.25em;
    text-align: center;
    border-radius: 10px 0 0 10px;
    border: 2px solid rgb(73, 41, 5);
    border-width: 4px 2px 4px 4px;
    background-color: rgba(73, 41, 5, 0);
    color: rgb(73, 41, 5);
    transition: all .3s ease-in-out;
  }
  .description {
    position: absolute;
    font-size: .6em;
    /* font-weight: 600; */
    text-transform: uppercase;
    bottom: .75em;
    left: calc( 50% - 25px);
    transform: translate(-50%, 0);
  }
  .yearbuttons {
    display: flex;
    flex-direction: column;
    gap: 0;
    margin: 0;
    vertical-align: bottom;
  }
  button {
    height: 100px;
    width: 50px;
    line-height: 100px;
    display: block;
    font-size: 1em;
    cursor: pointer;
    transition: all .3s ease-out;
    background-color: transparent;
    position: relative;
    text-align: center;
    background-color: rgba(73, 41, 5, .1);
    border-radius: 0 10px 0px 0;
    border: 2px solid rgb(73, 41, 5);
    border-width: 4px 4px 2px 2px;
    background-color: rgba(73, 41, 5, 0);
  }
  button:last-child {
    border-radius: 0 0 10px 0;
    border-width: 2px 4px 4px 2px;
  }
  button span {
    font-weight: 400;
    position: relative;
    top: -.125em;
  }
  button:active {
    background-color: rgba(73, 41, 5, .2);
  }
  button:active span {
    font-weight: 600;
  }
  input[type=number]::selection {
    background-color: rgba(73, 41, 5, 1);
    color: yellow;
  }
  input[type=number]:focus {
    outline: none;
    background-color: rgba(73, 41, 5, .2);  
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
  
</style>