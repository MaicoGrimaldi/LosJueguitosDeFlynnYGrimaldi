<script>
  import { onMount } from 'svelte';
  
  let { game } = $props();
  
  let controllerRef = $state();
  let isHovered = $state(false);
  let isPressed = $state(false);
  let leftStickPosition = $state({ x: 0, y: 0 });
  let rightStickPosition = $state({ x: 0, y: 0 });
  let animationFrame = $state();
  
  // Derived values based on game data
  const difficulty = $derived(game.difficulty / 10); // 0-1
  const popularity = $derived(game.popularity / 100); // 0-1
  const rating = $derived(game.rating / 10); // 0-1
  const releaseYear = $derived(game.releaseYear);
  const normalizedYear = $derived((releaseYear - 1980) / (2024 - 1980)); // 0-1
  
  // Controller style based on platform and era
  const getControllerStyle = $derived.by(() => {
    if (game.platform.includes('Nintendo Switch') || game.platform.includes('Nintendo')) {
      return 'nintendo';
    } else if (game.platform.includes('PlayStation')) {
      return 'playstation';
    } else if (game.platform.includes('Xbox')) {
      return 'xbox';
    } else if (game.platform.includes('PC')) {
      return 'pc';
    } else if (game.platform.includes('Arcade')) {
      return 'arcade';
    } else if (releaseYear < 1990) {
      return 'retro';
    } else {
      return 'modern';
    }
  });
  
  // Genre color mapping
  const genreColors = {
    'Acción': 'from-red-500 to-orange-500',
    'Aventura': 'from-green-500 to-emerald-500',
    'RPG': 'from-purple-500 to-violet-500',
    'Estrategia': 'from-blue-500 to-cyan-500',
    'Deportes': 'from-yellow-500 to-amber-500',
    'Puzzle': 'from-pink-500 to-rose-500',
    'Carreras': 'from-indigo-500 to-blue-500',
    'Lucha': 'from-red-600 to-red-400',
    'Simulación': 'from-teal-500 to-green-500'
  };
  
  const genreColor = $derived(genreColors[game.genre] || 'from-gray-500 to-gray-400');
  
  // Animation based on game characteristics
  onMount(() => {
    const animate = () => {
      const time = Date.now() * 0.001;
      const speed = 0.5 + normalizedYear * 1.5; // Newer games animate faster
      const intensity = difficulty * 8; // Higher difficulty = more movement
      
      leftStickPosition = {
        x: Math.sin(time * speed) * intensity,
        y: Math.cos(time * speed * 0.7) * intensity
      };
      
      rightStickPosition = {
        x: Math.sin(time * speed * 1.3) * intensity * 0.8,
        y: Math.cos(time * speed * 0.9) * intensity * 0.8
      };
      
      animationFrame = requestAnimationFrame(animate);
    };
    
    animate();
    
    return () => {
      if (animationFrame) {
        cancelAnimationFrame(animationFrame);
      }
    };
  });
  
  function handleMouseEnter() {
    isHovered = true;
  }
  
  function handleMouseLeave() {
    isHovered = false;
    isPressed = false;
  }
  
  function handleMouseDown() {
    isPressed = true;
  }
  
  function handleMouseUp() {
    isPressed = false;
  }

  function handleClick() {
    // Dispatch event to parent to open modal
    const event = new CustomEvent('openGameModal', {
      detail: { game }
    });
    controllerRef?.dispatchEvent(event);
  }
</script>

<div 
  bind:this={controllerRef}
  class="relative group cursor-pointer select-none"
  onmouseenter={handleMouseEnter}
  onmouseleave={handleMouseLeave}
  onmousedown={handleMouseDown}
  onmouseup={handleMouseUp}
  onclick={handleClick}
  onkeydown={(e) => { if (e.key === 'Enter' || e.key === ' ') handleClick(); }}
  role="button"
  tabindex="0"
  aria-label="Mando para {game.name} - Haz clic para más información"
>
  <!-- Controller container -->
  <div class="relative w-32 h-20 mx-auto">
    <!-- Controller shadow -->
    <div class="absolute inset-0 bg-black rounded-full opacity-20 transform translate-y-1 scale-110"></div>
    
    <!-- Controller body based on style -->
    {#if getControllerStyle === 'nintendo'}
      <!-- Nintendo Switch Pro Controller Style -->
      <div 
        class="relative w-full h-full bg-gradient-to-br {genreColor} rounded-2xl border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- ABXY buttons -->
        <div class="absolute right-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute top-0 left-1/2 w-2 h-2 bg-yellow-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2 - rating}s infinite"></div>
            <div class="absolute bottom-0 left-1/2 w-2 h-2 bg-blue-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2.2 - rating}s infinite"></div>
            <div class="absolute left-0 top-1/2 w-2 h-2 bg-red-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.4 - rating}s infinite"></div>
            <div class="absolute right-0 top-1/2 w-2 h-2 bg-green-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.6 - rating}s infinite"></div>
          </div>
        </div>
        
        <!-- Analog sticks -->
        <div class="absolute left-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({leftStickPosition.x}px, {leftStickPosition.y}px)"
          ></div>
        </div>
        <div class="absolute right-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({rightStickPosition.x}px, {rightStickPosition.y}px)"
          ></div>
        </div>
      </div>
    {:else if getControllerStyle === 'playstation'}
      <!-- PlayStation DualShock Style -->
      <div 
        class="relative w-full h-full bg-gradient-to-br {genreColor} border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="border-radius: 40% 40% 60% 60%; box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- PlayStation symbols -->
        <div class="absolute right-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute top-0 left-1/2 w-2 h-2 bg-pink-400 rounded-sm transform -translate-x-1/2 rotate-45" style="animation: pulse {2 - rating}s infinite"></div>
            <div class="absolute bottom-0 left-1/2 w-2 h-2 bg-blue-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2.2 - rating}s infinite"></div>
            <div class="absolute left-0 top-1/2 w-2 h-2 bg-green-400 transform -translate-y-1/2 rounded-sm" style="animation: pulse {2.4 - rating}s infinite"></div>
            <div class="absolute right-0 top-1/2 w-2 h-2 bg-red-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.6 - rating}s infinite"></div>
          </div>
        </div>
        
        <!-- Analog sticks -->
        <div class="absolute left-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({leftStickPosition.x}px, {leftStickPosition.y}px)"
          ></div>
        </div>
        <div class="absolute right-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({rightStickPosition.x}px, {rightStickPosition.y}px)"
          ></div>
        </div>
        
        <!-- DualShock grips -->
        <div class="absolute -left-2 bottom-0 w-4 h-6 bg-gradient-to-br {genreColor} rounded-b-full border-l-2 border-b-2 border-gray-700"></div>
        <div class="absolute -right-2 bottom-0 w-4 h-6 bg-gradient-to-br {genreColor} rounded-b-full border-r-2 border-b-2 border-gray-700"></div>
      </div>
    {:else if getControllerStyle === 'xbox'}
      <!-- Xbox Controller Style -->
      <div 
        class="relative w-full h-full bg-gradient-to-br {genreColor} border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="border-radius: 60% 60% 30% 30%; box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- ABXY buttons -->
        <div class="absolute right-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute top-0 left-1/2 w-2 h-2 bg-yellow-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2 - rating}s infinite"></div>
            <div class="absolute bottom-0 left-1/2 w-2 h-2 bg-green-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2.2 - rating}s infinite"></div>
            <div class="absolute left-0 top-1/2 w-2 h-2 bg-blue-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.4 - rating}s infinite"></div>
            <div class="absolute right-0 top-1/2 w-2 h-2 bg-red-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.6 - rating}s infinite"></div>
          </div>
        </div>
        
        <!-- Analog sticks -->
        <div class="absolute left-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({leftStickPosition.x}px, {leftStickPosition.y}px)"
          ></div>
        </div>
        <div class="absolute right-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({rightStickPosition.x}px, {rightStickPosition.y}px)"
          ></div>
        </div>
        
        <!-- Xbox bumpers -->
        <div class="absolute top-0 left-1/4 w-4 h-2 bg-gray-700 rounded-t-lg border-t-2 border-gray-600"></div>
        <div class="absolute top-0 right-1/4 w-4 h-2 bg-gray-700 rounded-t-lg border-t-2 border-gray-600"></div>
      </div>
    {:else if getControllerStyle === 'pc'}
      <!-- PC Gamepad Style -->
      <div 
        class="relative w-full h-full bg-gradient-to-br {genreColor} rounded-xl border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- Action buttons -->
        <div class="absolute right-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute top-0 left-1/2 w-2 h-2 bg-cyan-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2 - rating}s infinite"></div>
            <div class="absolute bottom-0 left-1/2 w-2 h-2 bg-purple-400 rounded-full transform -translate-x-1/2" style="animation: pulse {2.2 - rating}s infinite"></div>
            <div class="absolute left-0 top-1/2 w-2 h-2 bg-orange-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.4 - rating}s infinite"></div>
            <div class="absolute right-0 top-1/2 w-2 h-2 bg-pink-400 rounded-full transform -translate-y-1/2" style="animation: pulse {2.6 - rating}s infinite"></div>
          </div>
        </div>
        
        <!-- Analog sticks -->
        <div class="absolute left-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({leftStickPosition.x}px, {leftStickPosition.y}px)"
          ></div>
        </div>
        <div class="absolute right-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({rightStickPosition.x}px, {rightStickPosition.y}px)"
          ></div>
        </div>
      </div>
    {:else if getControllerStyle === 'arcade'}
      <!-- Arcade Controller Style -->
      <div 
        class="relative w-full h-16 bg-gradient-to-br {genreColor} rounded-lg border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-4 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- Action buttons -->
        <div class="absolute right-4 top-1/2 transform -translate-y-1/2 flex space-x-1">
          <div class="w-2 h-2 bg-red-400 rounded-full" style="animation: pulse {2 - rating}s infinite"></div>
          <div class="w-2 h-2 bg-blue-400 rounded-full" style="animation: pulse {2.2 - rating}s infinite"></div>
        </div>
        
        <!-- Start/Select buttons -->
        <div class="absolute top-2 left-1/2 transform -translate-x-1/2 flex space-x-2">
          <div class="w-3 h-1 bg-gray-800 rounded-full"></div>
          <div class="w-3 h-1 bg-gray-800 rounded-full"></div>
        </div>
      </div>
    {:else if getControllerStyle === 'retro'}
      <!-- Retro NES/SNES Style -->
      <div 
        class="relative w-full h-16 bg-gradient-to-br {genreColor} rounded-md border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-4 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- A/B buttons -->
        <div class="absolute right-4 top-1/2 transform -translate-y-1/2 flex space-x-2">
          <div class="w-3 h-3 bg-red-400 rounded-full" style="animation: pulse {2 - rating}s infinite"></div>
          <div class="w-3 h-3 bg-red-400 rounded-full" style="animation: pulse {2.2 - rating}s infinite"></div>
        </div>
        
        <!-- Start/Select -->
        <div class="absolute top-2 right-8 flex space-x-2">
          <div class="w-2 h-1 bg-gray-800 rounded-full"></div>
          <div class="w-2 h-1 bg-gray-800 rounded-full"></div>
        </div>
      </div>
    {:else}
      <!-- Modern/Default Controller -->
      <div 
        class="relative w-full h-full bg-gradient-to-br {genreColor} rounded-2xl border-2 border-gray-700 transition-all duration-300 transform"
        class:scale-110={isHovered}
        class:scale-95={isPressed}
        style="box-shadow: 0 0 {popularity * 20}px {popularity * 8}px rgba(59, 130, 246, {popularity * 0.4})"
      >
        <!-- D-pad -->
        <div class="absolute left-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute inset-x-1 inset-y-0 bg-gray-800 rounded-sm"></div>
            <div class="absolute inset-y-1 inset-x-0 bg-gray-800 rounded-sm"></div>
          </div>
        </div>
        
        <!-- Action buttons -->
        <div class="absolute right-3 top-1/2 transform -translate-y-1/2">
          <div class="relative w-4 h-4">
            <div class="absolute top-0 left-1/2 w-2 h-2 bg-white rounded-full transform -translate-x-1/2" style="animation: pulse {2 - rating}s infinite"></div>
            <div class="absolute bottom-0 left-1/2 w-2 h-2 bg-white rounded-full transform -translate-x-1/2" style="animation: pulse {2.2 - rating}s infinite"></div>
            <div class="absolute left-0 top-1/2 w-2 h-2 bg-white rounded-full transform -translate-y-1/2" style="animation: pulse {2.4 - rating}s infinite"></div>
            <div class="absolute right-0 top-1/2 w-2 h-2 bg-white rounded-full transform -translate-y-1/2" style="animation: pulse {2.6 - rating}s infinite"></div>
          </div>
        </div>
        
        <!-- Analog sticks -->
        <div class="absolute left-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({leftStickPosition.x}px, {leftStickPosition.y}px)"
          ></div>
        </div>
        <div class="absolute right-6 bottom-2">
          <div 
            class="w-3 h-3 bg-gray-800 rounded-full border border-gray-600"
            style="transform: translate({rightStickPosition.x}px, {rightStickPosition.y}px)"
          ></div>
        </div>
      </div>
    {/if}
  </div>
  
  <!-- Game name -->
  <div class="text-center mt-3">
    <h3 class="text-sm font-semibold text-white truncate px-2">
      {game.name}
    </h3>
    <p class="text-xs text-gray-400 mt-1">
      {game.genre} • {game.releaseYear}
    </p>
  </div>
  
  <!-- Tooltip -->
  {#if isHovered}
    <div class="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 z-50 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
      <div class="bg-gray-800 text-white p-3 rounded-lg shadow-xl border border-gray-600 min-w-48">
        <h4 class="font-bold text-sm mb-2">{game.name}</h4>
        <div class="space-y-1 text-xs">
          <div class="flex justify-between">
            <span>Género:</span>
            <span class="text-cyan-400">{game.genre}</span>
          </div>
          <div class="flex justify-between">
            <span>Plataforma:</span>
            <span class="text-cyan-400">{game.platform}</span>
          </div>
          <div class="flex justify-between">
            <span>Año:</span>
            <span class="text-cyan-400">{game.releaseYear}</span>
          </div>
          <div class="flex justify-between">
            <span>Puntaje:</span>
            <span class="text-yellow-400">{game.rating}/10</span>
          </div>
          <div class="flex justify-between">
            <span>Dificultad:</span>
            <span class="text-red-400">{game.difficulty}/10</span>
          </div>
          <div class="flex justify-between">
            <span>Popularidad:</span>
            <span class="text-green-400">{game.popularity}%</span>
          </div>
        </div>
        <!-- Tooltip arrow -->
        <div class="absolute top-full left-1/2 transform -translate-x-1/2 border-4 border-transparent border-t-gray-800"></div>
      </div>
    </div>
  {/if}
</div>

<style>
  @keyframes pulse {
    0%, 100% { 
      opacity: 1; 
      transform: scale(1);
    }
    50% { 
      opacity: 0.7; 
      transform: scale(0.9);
    }
  }
</style>
