<script>
  import { onMount } from 'svelte';
  import Joystick from './Joystick.svelte';
  import { gameData } from './gameData.js';
  
  let gridRef = $state();
  let isVisible = $state(false);
  // Ordenar los juegos por año de lanzamiento ascendente
  let games = $state([...gameData].sort((a, b) => a.releaseYear - b.releaseYear));
  
  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            isVisible = true;
          }
        });
      },
      { threshold: 0.1 }
    );
    
    if (gridRef) {
      observer.observe(gridRef);
    }
    
    return () => observer.disconnect();
  });
</script>

<section id="joystick-grid-section" bind:this={gridRef} class="py-20 px-4 bg-gradient-to-br from-gray-900 via-gray-800 to-gray-900 relative overflow-hidden" style="font-family: 'zrnic rg', sans-serif;">
  <!-- Background effects -->
  <div class="absolute inset-0 opacity-10">
    <div class="absolute top-1/4 left-1/4 w-96 h-96 bg-purple-500 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-cyan-500 rounded-full blur-3xl"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Section header -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent mb-6" style="font-family: 'zrnic rg', sans-serif;">
        <span class="bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent">
          GAME DATA PLAYGROUND
        </span>
      </h2>
      
      <!-- Stats -->
      <div class="flex flex-wrap justify-center gap-8 mb-12">
        <div class="text-center">
          <div class="text-3xl font-bold text-cyan-400" style="font-family: 'zrnic rg', sans-serif;">30</div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Juegos</div>
        </div>
        <div class="text-center">
          <div class="text-3xl font-bold text-purple-400" style="font-family: 'zrnic rg', sans-serif;">6</div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Plataformas</div>
        </div>
        <div class="text-center">
          <div class="text-3xl font-bold text-pink-400" style="font-family: 'zrnic rg', sans-serif;">9</div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Géneros</div>
        </div>
        <div class="text-center">
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">-></div>
        </div>
        <div class="text-center">
          <div class="text-3xl font-bold text-pink-400" style="font-family: 'zrnic rg', sans-serif;">♾️</div>
          <div class="text-gray-400" style="font-family: 'zrnic rg', sans-serif;">Posibilidades</div>
        </div>
      </div>
    </div>
    
    <!-- Controllers grid -->
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-6 md:gap-8" >
      {#each games as game, index}
        <div 
          class="transform transition-all duration-500"
          class:animate-fade-in-scale={isVisible}
          style="animation-delay: {index * 50}ms;"
        >
          <Joystick {game} />
        </div>
      {/each}
    </div>
    
    <!-- Footer message -->
    <div class="text-center mt-16">
      <p class="text-gray-400 text-lg" style="font-family: 'zrnic rg', sans-serif;">
        Visualiza la representación interactiva de cada mando
      </p>
    </div>
  </div>
</section>

<style>
  @keyframes fade-in-scale {
    from {
      opacity: 0;
      transform: scale(0.8) translateY(20px);
    }
    to {
      opacity: 1;
      transform: scale(1) translateY(0);
    }
  }
  
  .animate-fade-in-scale {
    animation: fade-in-scale 0.6s ease-out forwards;
  }
</style>
