<script lang="ts">
    let timeRemaining = 0; // Timer duration in seconds
    // @ts-ignore
    let timer = null;
    let isRunning = false;
    let minTime = 50;
    let maxTime = 70;
    let volume = 0.50;
  
    let beepAudio: HTMLAudioElement;
    let beepEndAudio: HTMLAudioElement;
  
    function updateRemainingTime() {
      if (!isRunning) {
        timeRemaining = Math.floor(Math.random() * (maxTime - minTime + 1)) + minTime;
      }
    }
  
    // Function to start the timer
    function startTimer() {
      updateRemainingTime();
      if (!isRunning) {
        isRunning = true;
        let interval = 1000; // Start with 1-second interval
        playBeep(); // Start beeping right away
  
        timer = setInterval(() => {
          if (timeRemaining > 0) {
            timeRemaining -= 1;
          } else {
            // @ts-ignore
            clearInterval(timer);
            isRunning = false;
          }
        }, interval);
      }
    }
  
    // Function to play the beep sound and adjust the speed
    function playBeep() {
      let beepInterval = 800;
  
      // Function to handle beeping
      function beep() {
        if (timeRemaining > 0) {
          beepAudio.volume = volume;
          beepAudio.play();
  
          if (timeRemaining >= 40) {
            beepInterval = 800;
          } else if (timeRemaining >= 30) {
            beepInterval = 700;
          } else if (timeRemaining >= 25) {
            beepInterval = 600;
          } else if (timeRemaining >= 20) {
            beepInterval = 500;
          } else if (timeRemaining >= 15) {
            beepInterval = 300;
          } else if (timeRemaining >= 10) {
            beepInterval = 100;
          } else if (timeRemaining > 5) {
            beepInterval = 50;
          } else {
            beepInterval = 25;
          }
  
          // Schedule the next beep
          console.log(beepInterval);
          setTimeout(beep, beepInterval);
        } else {
            beepEndAudio.volume = volume;
            beepEndAudio.play();
        }
      }
  
      // Start the first beep
      beep();
    }
  
    function resetTimer() {
      // @ts-ignore
      clearInterval(timer);
      isRunning = false;
      timeRemaining = 0;
    }
  </script>

  <title>Catchphrase Timer</title>
  
  <div class="flex flex-col items-center justify-center h-screen text-black font-sans">
    <div class="relative">
      <img src="svgs/Button_Icon_White.svg" alt="White button" class="block py-4 px-4  w-60 h-60">
      <div class="absolute inset-0 flex items-center justify-center text-black text-6xl font-bold">{timeRemaining}s</div>
    </div>
    <div class="space-x-4">
      <button 
        on:click={startTimer} 
        class="px-6 py-2 text-lg bg-gray-200 hover:bg-gray-300 rounded disabled:opacity-50" 
        disabled={isRunning}>
        Start
      </button>
      <button 
        on:click={resetTimer} 
        class="px-6 py-2 text-lg bg-gray-200 hover:bg-gray-300 rounded">
        Reset
      </button>
    </div>
    <div>
      <input type="range" bind:value={minTime} min="1" max="60" step="1" class="w-full mt-4 accent-white">
      <p class="text-white">Min: {minTime}s</p>
      <input type="range" bind:value={maxTime} min="60" max="120" step="1" class="w-full mt-4 accent-white">
      <p class="text-white">Max: {maxTime}s</p>
      <input type="range" bind:value={volume} min="0" max="1" step="0.01" class="w-full mt-4 accent-white">
      <p class="text-white">Volume: {Math.round(volume * 100)}%</p>
    </div>
    
    <!-- Audio element for the beep sound -->
    <audio bind:this={beepAudio} preload="auto">
      <source src="/audio/catchphrase_beep.mp3" type="audio/mp3" />
    </audio>
    <audio bind:this={beepEndAudio} preload="auto">
        <source src="/audio/catchphrase_end.mp3" type="audio/mp3" />
      </audio>
  </div>

  
  