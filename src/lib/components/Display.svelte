<script lang="ts">
  export let hours: number;
  export let minutes: number;
  export let seconds: number;
  export let almostDone: boolean;
  export let hasCompleted: boolean;
</script>

{#if hasCompleted && !hours && !minutes && !seconds}
  <div class="time-up" transition:fade>Time's Up!</div>
{:else}
  <div class="display" class:almost-done={almostDone}>
    <div class="time">
      {#if hours > 0}
        <span>{hours.toString().padStart(2, '0')}:</span>
      {/if}
      <span>{minutes.toString().padStart(2, '0')}:</span>
      <span>{seconds.toString().padStart(2, '0')}</span>
    </div>
  </div>
{/if}

<style>
  .display {
    padding: 2rem;
    transition: color 0.3s ease;
  }

  .time {
    font-family: 'Courier New', monospace;
    font-size: min(20vw, 20vh);
    font-weight: bold;
    color: #00ff00;
    text-align: center;
    text-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
    line-height: 1;
    letter-spacing: -2px;
  }

  .almost-done .time {
    color: #ff3333;
    text-shadow: 0 0 20px rgba(255, 0, 0, 0.8);
    animation: pulse 1s ease-in-out infinite;
  }

  .time-up {
    font-family: 'Courier New', monospace;
    font-size: 15vw;
    font-weight: bold;
    color: #ff3333;
    text-shadow: 0 0 20px rgba(255, 0, 0, 0.8);
    text-align: center;
    animation: appear 0.5s ease-out;
  }

  @keyframes pulse {
    0% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.8; transform: scale(1.05); }
    100% { opacity: 1; transform: scale(1); }
  }

  @keyframes appear {
    from { opacity: 0; transform: scale(0.8); }
    to { opacity: 1; transform: scale(1); }
  }
</style>