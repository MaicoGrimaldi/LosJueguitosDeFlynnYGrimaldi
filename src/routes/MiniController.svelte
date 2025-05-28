<script>
  let { example } = $props();
  
  // Get the appropriate visual properties based on example type
  const getControllerProps = $derived.by(() => {
    const props = {
      platform: example.platform || 'modern',
      genre: example.genre || 'Action',
      difficulty: example.difficulty || 5,
      popularity: example.popularity || 50,
      rating: example.rating || 7,
      year: example.year || 2020,
      color: example.color || 'from-gray-500 to-gray-400'
    };
    
    // Override color if provided directly
    if (example.color) {
      props.color = example.color;
    } else {
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
      props.color = genreColors[props.genre] || 'from-gray-500 to-gray-400';
    }
    
    return props;
  });
  
  const controllerProps = $derived(getControllerProps);
</script>

<div class="relative w-20 h-12">
  <!-- Controller based on platform -->
  {#if controllerProps.platform === 'nintendo'}
    <!-- Nintendo Switch Pro Controller Style -->
    <div 
      class="relative w-full h-full bg-gradient-to-br {controllerProps.color} rounded-xl border border-gray-600"
      style="box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- ABXY buttons -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute top-0 left-1/2 w-1 h-1 bg-yellow-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute bottom-0 left-1/2 w-1 h-1 bg-blue-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute left-0 top-1/2 w-1 h-1 bg-red-400 rounded-full transform -translate-y-1/2"></div>
          <div class="absolute right-0 top-1/2 w-1 h-1 bg-green-400 rounded-full transform -translate-y-1/2"></div>
        </div>
      </div>
      
      <!-- Analog sticks -->
      <div class="absolute left-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      <div class="absolute right-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
    </div>
  {:else if controllerProps.platform === 'playstation'}
    <!-- PlayStation DualShock Style -->
    <div 
      class="relative w-full h-full bg-gradient-to-br {controllerProps.color} border border-gray-600"
      style="border-radius: 40% 40% 60% 60%; box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- PlayStation symbols -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute top-0 left-1/2 w-1 h-1 bg-pink-400 rounded-sm transform -translate-x-1/2 rotate-45"></div>
          <div class="absolute bottom-0 left-1/2 w-1 h-1 bg-blue-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute left-0 top-1/2 w-1 h-1 bg-green-400 transform -translate-y-1/2"></div>
          <div class="absolute right-0 top-1/2 w-1 h-1 bg-red-400 rounded-full transform -translate-y-1/2"></div>
        </div>
      </div>
      
      <!-- Analog sticks -->
      <div class="absolute left-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      <div class="absolute right-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      
      <!-- Grips -->
      <div class="absolute -left-1 bottom-0 w-2 h-3 bg-gradient-to-br {controllerProps.color} rounded-b-full"></div>
      <div class="absolute -right-1 bottom-0 w-2 h-3 bg-gradient-to-br {controllerProps.color} rounded-b-full"></div>
    </div>
  {:else if controllerProps.platform === 'xbox'}
    <!-- Xbox Controller Style -->
    <div 
      class="relative w-full h-full bg-gradient-to-br {controllerProps.color} border border-gray-600"
      style="border-radius: 60% 60% 30% 30%; box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- ABXY buttons -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute top-0 left-1/2 w-1 h-1 bg-yellow-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute bottom-0 left-1/2 w-1 h-1 bg-green-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute left-0 top-1/2 w-1 h-1 bg-blue-400 rounded-full transform -translate-y-1/2"></div>
          <div class="absolute right-0 top-1/2 w-1 h-1 bg-red-400 rounded-full transform -translate-y-1/2"></div>
        </div>
      </div>
      
      <!-- Analog sticks -->
      <div class="absolute left-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      <div class="absolute right-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      
      <!-- Xbox bumpers -->
      <div class="absolute top-0 left-1/4 w-2 h-1 bg-gray-700 rounded-t-sm"></div>
      <div class="absolute top-0 right-1/4 w-2 h-1 bg-gray-700 rounded-t-sm"></div>
    </div>
  {:else if controllerProps.platform === 'pc'}
    <!-- PC Gamepad Style -->
    <div 
      class="relative w-full h-full bg-gradient-to-br {controllerProps.color} rounded-lg border border-gray-600"
      style="box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- Action buttons -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute top-0 left-1/2 w-1 h-1 bg-cyan-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute bottom-0 left-1/2 w-1 h-1 bg-purple-400 rounded-full transform -translate-x-1/2"></div>
          <div class="absolute left-0 top-1/2 w-1 h-1 bg-orange-400 rounded-full transform -translate-y-1/2"></div>
          <div class="absolute right-0 top-1/2 w-1 h-1 bg-pink-400 rounded-full transform -translate-y-1/2"></div>
        </div>
      </div>
      
      <!-- Analog sticks -->
      <div class="absolute left-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      <div class="absolute right-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
    </div>
  {:else if controllerProps.platform === 'arcade'}
    <!-- Arcade Controller Style -->
    <div 
      class="relative w-full h-8 bg-gradient-to-br {controllerProps.color} rounded border border-gray-600"
      style="box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- Action buttons -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2 flex space-x-0.5">
        <div class="w-1 h-1 bg-red-400 rounded-full"></div>
        <div class="w-1 h-1 bg-blue-400 rounded-full"></div>
      </div>
      
      <!-- Start/Select -->
      <div class="absolute top-1 left-1/2 transform -translate-x-1/2 flex space-x-1">
        <div class="w-1.5 h-0.5 bg-gray-800 rounded-full"></div>
        <div class="w-1.5 h-0.5 bg-gray-800 rounded-full"></div>
      </div>
    </div>
  {:else if controllerProps.platform === 'retro'}
    <!-- Retro NES/SNES Style -->
    <div 
      class="relative w-full h-8 bg-gradient-to-br {controllerProps.color} rounded-sm border border-gray-600"
      style="box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- A/B buttons -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2 flex space-x-1">
        <div class="w-1.5 h-1.5 bg-red-400 rounded-full"></div>
        <div class="w-1.5 h-1.5 bg-red-400 rounded-full"></div>
      </div>
      
      <!-- Start/Select -->
      <div class="absolute top-1 right-6 flex space-x-1">
        <div class="w-1 h-0.5 bg-gray-800 rounded-full"></div>
        <div class="w-1 h-0.5 bg-gray-800 rounded-full"></div>
      </div>
    </div>
  {:else}
    <!-- Modern/Default Controller -->
    <div 
      class="relative w-full h-full bg-gradient-to-br {controllerProps.color} rounded-lg border border-gray-600"
      style="box-shadow: 0 0 {controllerProps.popularity * 0.15}px {controllerProps.popularity * 0.05}px rgba(59, 130, 246, {controllerProps.popularity * 0.004})"
    >
      <!-- D-pad -->
      <div class="absolute left-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute inset-x-0.5 inset-y-0 bg-gray-800 rounded-sm"></div>
          <div class="absolute inset-y-0.5 inset-x-0 bg-gray-800 rounded-sm"></div>
        </div>
      </div>
      
      <!-- Action buttons -->
      <div class="absolute right-2 top-1/2 transform -translate-y-1/2">
        <div class="relative w-2 h-2">
          <div class="absolute top-0 left-1/2 w-1 h-1 bg-white rounded-full transform -translate-x-1/2"></div>
          <div class="absolute bottom-0 left-1/2 w-1 h-1 bg-white rounded-full transform -translate-x-1/2"></div>
          <div class="absolute left-0 top-1/2 w-1 h-1 bg-white rounded-full transform -translate-y-1/2"></div>
          <div class="absolute right-0 top-1/2 w-1 h-1 bg-white rounded-full transform -translate-y-1/2"></div>
        </div>
      </div>
      
      <!-- Analog sticks -->
      <div class="absolute left-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
      <div class="absolute right-3 bottom-1">
        <div class="w-1.5 h-1.5 bg-gray-800 rounded-full"></div>
      </div>
    </div>
  {/if}
</div>
