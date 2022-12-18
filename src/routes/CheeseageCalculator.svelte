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
    let interval = setInterval(handlePlusBtnClick, 100);
    this.addEventListener('mouseup', () => clearInterval(interval));
    this.addEventListener('touchend', () => clearInterval(interval));
  }
  function handleMinBtnClick() {
    if (humanage > 0) {
      humanage = humanage - 1;
    }
  }
  function handleMinBtnHold() {
    let interval = setInterval(handleMinBtnClick, 100);
    this.addEventListener('mouseup', () => clearInterval(interval));
    this.addEventListener('touchend', () => clearInterval(interval));
  }
  function handleInputFocus() {
    input.select();
  }
</script>

  <div class="cheeseage-input">
    <input 
      type="number" 
      name="age" 
      bind:value={humanage} 
      bind:this={input} 
      on:click={() => handleInputFocus()} 
      min="0" 
      max="110" 
      pattern="[0-9]*"
    >
    <div class="yearbuttons">
      <button on:click={handlePlusBtnClick} on:mousedown={handlePlusBtnHold} on:touchstart={handlePlusBtnHold}><span>+</span></button>
      <button on:click={handleMinBtnClick} on:mousedown={handleMinBtnHold} on:touchstart={handleMinBtnHold}><span>-</span></button>
    </div>
    {#if !humanage}
      <div class="helper">Enter your age here, then check result below</div>
    {/if}
  </div>
  
  

<style>
  .helper {
    position: absolute;
    display: flex;
    font-size: 13px;
    padding: .25em;
    right: calc(100% + 8px);
    width: 8em;
    height: 100%;
    font-family: 'Times New Roman', Times, serif;
    line-height: 1.1;
    letter-spacing: -0.01em;
    justify-content: center;
    align-content: center;
    flex-direction: column;
    background-color: var(--color-yellow);
    border-radius: 3px;
    animation: helperin 0.3s 1s cubic-bezier(.79,0,.49,1.28) both;
  }
  .helper:after {
    content: '';
    position: absolute;
    top: 50%;
    left: 100%;
    width: 0;
    height: 0;
    border: 4px solid transparent;
    border-left-color: var(--color-yellow);
    margin-top: -4px;
  }
  @media (min-width: 600px) {
    .helper {
      right: 0;
      left: 0;
      top: calc(100% + 8px);
      width: 100%;
      min-width: 8em; 
      height: auto ;
    }
    .helper:after {
      content: '';
      position: absolute;
      top: 0;
      left: 1.5em;
      width: 0;
      height: 0;
      border: 4px solid transparent;
      border-left-color: transparent;
      border-bottom-color: var(--color-yellow);
      margin-top: -8px;
      margin-left: -4px;
    }
  }
  @keyframes helperin {
    0% {
      opacity: 0;
      transform: translateY(-10px);
    }
    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }
  .cheeseage-input {
    --input-height: 14vw;
    --input-width: 18vw;
    font-size: 1em;
    color: var(--color-brown);
    width: calc(var(--input-width) + var(--input-height) / 2);
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0;
    position: relative;
    margin: 1rem auto .75rem auto;
  }
  @media (min-width: 600px) {
    .cheeseage-input {
      --input-height: 4vw;
      --input-width: 4.5vw;
      display: inline-flex;
      margin: 0 .25em;
    }
  }
  input[type=number] {
    width: var(--input-width);
    height: var(--input-height);
    line-height: 1;
    padding: 0 0 0 0;
    display: inline;
    font-family: var(--font-display);
    font-size: calc(var(--input-height) / 1.5);
    font-weight: 500;
    text-align: center;
    color: var(--color-brown);
    border: 1px solid;
    border-right-width: 1px;
    border-radius: 0;
    background-color: transparent;
    transition: all .3s ease-in-out;
    position: relative;
    z-index: 4;
  }
  input[type=number]::selection {
    background-color: rgba(0, 0, 0, 1);
    color: var(--color-yellow);
  }
  input[type=number]:focus {
    outline: none;
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
    position: relative;
    text-align: center;
    color: var(--color-brown);
    border: 1px solid;
    border-width: 1px 1px .5px 0;
    background-color: var(--color-yellow);
    padding: 0;
    -webkit-user-select: none; /* Safari */
    -ms-user-select: none; /* IE 10 and IE 11 */
    user-select: none; /* Standard syntax */
  }
  button:last-child {
    border-width: .5px 1px 1px 0;
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
    background-color: #e6962f;
  }
  button:active span {
    font-weight: 600;
  }
  
  
</style>