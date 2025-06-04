<script>
  import { onMount } from 'svelte';
  
  let heroSection = $state();
  let particles = $state([]);
  
  onMount(() => {
    // Crear partículas flotantes
    particles = Array.from({ length: 20 }, (_, i) => ({
      id: i,
      x: Math.random() * 100,
      y: Math.random() * 100,
      size: Math.random() * 4 + 2,
      speed: Math.random() * 2 + 1
    }));
    
    // Animar partículas
    const animateParticles = () => {
      particles = particles.map(particle => ({
        ...particle,
        y: (particle.y + particle.speed * 0.1) % 100
      }));
      requestAnimationFrame(animateParticles);
    };
    animateParticles();
  });

  function scrollToGrid() {
    const el = document.getElementById('joystick-grid-section');
    if (el) {
      el.scrollIntoView({ behavior: 'smooth' });
    }
  }
</script>

<section bind:this={heroSection} class="relative min-h-screen flex items-center justify-center overflow-hidden bg-gradient-to-br from-purple-900 via-blue-900 to-indigo-900" style="font-family: 'zrnic rg', sans-serif;">
  <!-- Partículas animadas de fondo -->
  <div class="absolute inset-0">
    {#each particles as particle (particle.id)}
      <div 
        class="absolute w-1 h-1 bg-cyan-400 rounded-full opacity-60"
        style="left: {particle.x}%; top: {particle.y}%; width: {particle.size}px; height: {particle.size}px;"
      ></div>
    {/each}
  </div>
  
  <!-- Contenido principal -->
  <div class="relative z-10 text-center px-4 max-w-6xl mx-auto">
    <!-- TV retro/Computadora -->
    <div class="mb-12 relative">
      <div class="relative mx-auto w-80 h-60 md:w-96 md:h-72 bg-gray-800 rounded-lg border-4 border-gray-600 shadow-2xl scanlines">
        <!-- Pantalla -->
        <div class="absolute inset-4 bg-black rounded border-2 border-gray-700 overflow-hidden">
          <div class="w-full h-full bg-gradient-to-br from-green-400 to-blue-500 opacity-80 flex items-center justify-center">
            <div class="text-black font-bold text-xl md:text-2xl animate-pulse" style="font-family: 'zrnic rg', sans-serif;">
              CARGANDO DATOS DEL JUEGO...
            </div>
          </div>
          <!-- Efecto de brillo de pantalla -->
          <div class="absolute inset-0 bg-gradient-to-t from-transparent via-white to-transparent opacity-10 animate-pulse"></div>
        </div>
        
        <!-- Base de la TV -->
        <div class="absolute -bottom-8 left-1/2 transform -translate-x-1/2 w-24 h-8 bg-gray-700 rounded-b-lg"></div>
      </div>
      
      <!-- Elementos flotantes de joystick -->
      <div class="absolute -left-10 top-10 w-30 h-30 opacity-60" style="animation: float 3s ease-in-out infinite;">
        <div class="w-full h-full bg-gradient-to-br from-red-500 to-pink-500 rounded-lg transform rotate-12"></div>
      </div>
      <div class="absolute -right-10 bottom-10 w-30 h-30 opacity-60" style="animation: float 4s ease-in-out infinite reverse;">
        <div class="w-full h-full bg-gradient-to-br from-blue-500 to-purple-500 rounded-full"></div>
      </div>
    </div>
    <div class="absolute -left-35 top-80 w-0 h-0 opacity-60" style="animation: float 3s ease-in-out infinite;">
    <div class="w-0 h-0" style="
      width: 0;
      height: 0;
      border-left: 2.5rem solid transparent;
      border-right: 2.5rem solid transparent;
      border-bottom: 4.5rem solid #34d399; /* Tailwind's green-400 */
    "></div>
  </div>
  <div class="absolute right-10 bottom-0 w-20 h-20 opacity-60 rotate-45" style="animation: float 4s ease-in-out infinite reverse;">
    <div class="relative w-full h-full">
      <div class="absolute inset-0 bg-gradient-to-br from-yellow-400 to-yellow-600 w-3 h-full mx-auto rounded"></div>
      <div class="absolute inset-0 bg-gradient-to-br from-yellow-400 to-yellow-600 h-3 w-full my-auto rounded"></div>
    </div>
  </div>

    
    <!-- Texto principal -->
    <div class="space-y-6">
      <h1 class="text-5xl md:text-7xl lg:text-8xl font-extrabold bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 bg-clip-text text-transparent leading-tight drop-shadow-lg tracking-wide animate-gradient-move" style="font-family: 'zrnic rg', sans-serif; letter-spacing: 0.05em;">
        <span class="relative">
          <span class="absolute -inset-1 blur-lg opacity-30 bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 rounded-lg"></span>
          <span class="relative z-10">UNIVERSO GAMER</span>
        </span>
      </h1>
      <style>
        @keyframes gradient-move {
          0% { background-position: 0% 50%; }
          100% { background-position: 100% 50%; }
        }
        .animate-gradient-move {
          background-size: 200% 200%;
          animation: gradient-move 3s linear infinite alternate;
        }
      </style>
      <p class="text-xl md:text-2xl text-gray-300 max-w-3xl mx-auto leading-relaxed" style="font-family: 'zrnic rg', sans-serif;">
        30 joysticks. 30 juegos. Todo se mueve, brilla y cambia con cada dato. Explorá este universo interactivo.

      </p>
      <div class="flex flex-col sm:flex-row gap-4 justify-center items-center mt-8">
        <button onclick={scrollToGrid} class="px-8 py-4 bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 rounded-lg font-semibold text-lg transition-all duration-300 transform hover:scale-105 glow shadow-lg">
          Comenzar a explorar
        </button>
      </div>
    </div>
    
    <!-- Indicador de scroll -->
    <div class="absolute bottom-1 left-60 transform -translate-x-1/2 animate-bounce">
      <div class="w-6 h-10 border-2 border-gray-400 rounded-full flex justify-center">
        <div class="w-1 h-3 bg-gray-400 rounded-full mt-2 animate-pulse"></div>
      </div>
    </div>
  </div>
  
  <!-- Capa de gradiente -->
  <div class="absolute inset-0 bg-gradient-to-b from-transparent via-transparent to-gray-900 opacity-60"></div>
</section>
