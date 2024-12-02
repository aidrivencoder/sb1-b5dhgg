<script lang="ts">
  import { onDestroy } from 'svelte';
  import { fade } from 'svelte/transition';
  import Display from './components/Display.svelte';
  import Controls from './components/Controls.svelte';
  import { updateTimeDisplay, calculateTotalSeconds } from './utils/time';

  let hours = 0;
  let minutes = 0;
  let seconds = 0;
  let totalSeconds = 0;
  let intervalId: number;
  let isRunning = false;
  let isPaused = false;
  let showControls = true;
  let almostDone = false;
  let hasCompleted = false;

  function startTimer() {
    if (!totalSeconds) return;
    
    isRunning = true;
    isPaused = false;
    showControls = false;
  
    intervalId = setInterval(() => {
      if (totalSeconds <= 0) {
        clearInterval(intervalId);
        isRunning = false;
        almostDone = false;
        hasCompleted = true;
        showControls = true;
        return;
      }
      
      totalSeconds--;
      if (totalSeconds <= 10) {
        almostDone = true;
      }
      const time = updateTimeDisplay(totalSeconds);
      hours = time.hours;
      minutes = time.minutes;
      seconds = time.seconds;
    }, 1000);
  }

  function pauseTimer() {
    clearInterval(intervalId);
    isPaused = true;
    isRunning = false;
    showControls = true;
  }

  function resetTimer() {
    clearInterval(intervalId);
    totalSeconds = 0;
    hours = 0;
    minutes = 0;
    seconds = 0;
    isRunning = false;
    isPaused = false;
    almostDone = false;
    hasCompleted = false;
    showControls = true;
  }

  function setTime() {
    totalSeconds = calculateTotalSeconds(hours, minutes, seconds);
    hasCompleted = false;
  }

  function toggleControls() {
    if (isRunning) {
      showControls = !showControls;
    }
  }

  function handleKeydown(event: KeyboardEvent) {
    if (event.key === ' ' || event.key === 'Enter') {
      if (!isRunning && !isPaused) {
        startTimer();
      } else if (isPaused) {
        startTimer();
      } else {
        pauseTimer();
      }
    } else if (event.key === 'Escape') {
      resetTimer();
    }
  }

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

<svelte:window on:keydown={handleKeydown}/>

<div class="timer-container" on:click={toggleControls}>
  <Display 
    {hours}
    {minutes}
    {seconds}
    {almostDone}
    {hasCompleted}
  />

  {#if showControls}
    <Controls
      {hours}
      {minutes}
      {seconds}
      {isRunning}
      {totalSeconds}
      onStart={startTimer}
      onPause={pauseTimer}
      onReset={resetTimer}
      onTimeChange={setTime}
    />
  {/if}
</div>

<style>
  .timer-container {
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background: radial-gradient(circle at center, #1a1a1a 0%, #000000 100%);
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }

  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: #000;
  }
</style>