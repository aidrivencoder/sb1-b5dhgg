<script lang="ts">
  import { fade } from 'svelte/transition';
  
  export let hours: number;
  export let minutes: number;
  export let seconds: number;
  export let isRunning: boolean;
  export let totalSeconds: number;
  export let onStart: () => void;
  export let onPause: () => void;
  export let onReset: () => void;
  export let onTimeChange: () => void;
</script>

<div class="controls" transition:fade>
  <div class="time-input">
    <div class="input-group">
      <input
        type="number"
        min="0"
        max="23"
        bind:value={hours}
        on:change={onTimeChange}
        placeholder="HH"
      />
      <span>h</span>
    </div>
    <div class="input-group">
      <input
        type="number"
        min="0"
        max="59"
        bind:value={minutes}
        on:change={onTimeChange}
        placeholder="MM"
      />
      <span>m</span>
    </div>
    <div class="input-group">
      <input
        type="number"
        min="0"
        max="59"
        bind:value={seconds}
        on:change={onTimeChange}
        placeholder="SS"
      />
      <span>s</span>
    </div>
  </div>

  <div class="buttons">
    {#if !isRunning}
      <button class="start" on:click={onStart} disabled={!totalSeconds}>
        <span class="icon">▶</span>
        Start
      </button>
    {:else}
      <button class="pause" on:click={onPause}>
        <span class="icon">⏸</span>
        Pause
      </button>
    {/if}
    <button class="reset" on:click={onReset}>
      <span class="icon">↺</span>
      Reset
    </button>
  </div>
</div>

<style>
  .controls {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(20, 20, 20, 0.95);
    padding: 2rem;
    border-radius: 1rem;
    backdrop-filter: blur(10px);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .time-input {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .input-group {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .input-group span {
    color: #666;
    font-size: 1.2rem;
  }

  input {
    background: #222;
    border: 1px solid #333;
    border-radius: 0.5rem;
    color: #fff;
    font-size: 1.5rem;
    padding: 0.5rem;
    width: 4rem;
    text-align: center;
    transition: all 0.2s;
  }

  input:focus {
    outline: none;
    border-color: #666;
    background: #2a2a2a;
  }

  input::-webkit-inner-spin-button,
  input::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  .buttons {
    display: flex;
    justify-content: center;
    gap: 1rem;
  }

  button {
    font-size: 1.2rem;
    padding: 0.8rem 2rem;
    border-radius: 0.5rem;
    border: none;
    cursor: pointer;
    transition: all 0.2s;
    min-width: 120px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }

  button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .icon {
    font-size: 1rem;
  }

  .start {
    background: linear-gradient(to bottom right, #00cc00, #008800);
    color: white;
  }

  .start:hover:not(:disabled) {
    background: linear-gradient(to bottom right, #00bb00, #007700);
    transform: translateY(-1px);
  }

  .pause {
    background: linear-gradient(to bottom right, #ffaa00, #cc8800);
    color: white;
  }

  .pause:hover {
    background: linear-gradient(to bottom right, #ff9900, #bb7700);
    transform: translateY(-1px);
  }

  .reset {
    background: linear-gradient(to bottom right, #cc0000, #990000);
    color: white;
  }

  .reset:hover {
    background: linear-gradient(to bottom right, #bb0000, #880000);
    transform: translateY(-1px);
  }

  @media (max-width: 600px) {
    .time-input {
      flex-direction: column;
      align-items: center;
    }

    .buttons {
      flex-direction: column;
    }

    button {
      width: 100%;
    }
  }
</style>