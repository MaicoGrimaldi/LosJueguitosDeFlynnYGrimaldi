<script>
  import { onMount } from 'svelte';
  import Modal from './Modal.svelte';
  
  let sectionRef = $state();
  let isVisible = $state(false);
  let activeModal = $state(null);
  
  const characteristics = [
    {
      id: 'difficulty',
      title: "Nivel de Dificultad",
      description: "La intensidad del movimiento del stick representa la dificultad del juego",
      icon: "🎯",
      color: "from-red-500 to-orange-500"
    },
    {
      id: 'popularity',
      title: "Puntuación de Popularidad",
      description: "La intensidad del resplandor muestra cuán popular es el juego",
      icon: "⭐",
      color: "from-yellow-500 to-amber-500"
    },
    {
      id: 'genre',
      title: "Género del Juego",
      description: "Los esquemas de color representan diferentes categorías de juegos",
      icon: "🎮",
      color: "from-purple-500 to-pink-500"
    },
    {
      id: 'rating',
      title: "Valoración de Usuarios",
      description: "La respuesta de los botones refleja la valoración de los usuarios",
      icon: "💯",
      color: "from-green-500 to-emerald-500"
    },
    {
      id: 'year',
      title: "Año de Lanzamiento",
      description: "La velocidad de animación indica cuándo se lanzó el juego",
      icon: "📅",
      color: "from-blue-500 to-cyan-500"
    },
    {
      id: 'platform',
      title: "Tipo de Plataforma",
      description: "Las variaciones de estilo de los controladores muestran las plataformas de juego",
      icon: "🖥️",
      color: "from-indigo-500 to-purple-500"
    }
  ];
  
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
    
    if (sectionRef) {
      observer.observe(sectionRef);
    }
    
    return () => observer.disconnect();
  });
  
  function openModal(characteristicId) {
    activeModal = characteristicId;
  }
  
  function closeModal() {
    activeModal = null;
  }
</script>

<section bind:this={sectionRef} class="py-20 px-4 bg-gray-900 relative overflow-hidden">
  <!-- Patrón de fondo -->
  <div class="absolute inset-0 opacity-5">
    <div class="absolute inset-0" style="background-image: radial-gradient(circle at 25% 25%, #8b5cf6 0%, transparent 50%), radial-gradient(circle at 75% 75%, #06b6d4 0%, transparent 50%);"></div>
  </div>
  
  <div class="max-w-7xl mx-auto relative z-10">
    <!-- Encabezado de sección -->
    <div class="text-center mb-16">
      <h2 class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-cyan-400 to-purple-400 bg-clip-text text-transparent mb-6">
        Entendiendo los Datos
      </h2>
      <p class="text-xl text-gray-300 max-w-3xl mx-auto leading-relaxed">
        Cada controlador representa dinámicamente las características del juego mediante elementos visuales. 
        Haz clic en "Saber más" en cada tarjeta para ver explicaciones detalladas y ejemplos visuales.
      </p>
    </div>
    
    <!-- Grid de características -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      {#each characteristics as characteristic, index}
        <div 
          class="bg-gray-800 rounded-xl p-6 border border-gray-700 hover:border-gray-600 transition-all duration-300 transform hover:scale-105 hover:shadow-2xl"
          class:animate-fade-in-up={isVisible}
          style="animation-delay: {index * 100}ms;"
        >
          <!-- Icono y fondo degradado -->
          <div class="relative mb-4">
            <div class="w-16 h-16 bg-gradient-to-br {characteristic.color} rounded-lg flex items-center justify-center text-2xl mb-4 mx-auto shadow-lg">
              {characteristic.icon}
            </div>
            <div class="absolute inset-0 bg-gradient-to-br {characteristic.color} rounded-lg opacity-20 blur-xl"></div>
          </div>
          
          <!-- Contenido -->
          <h3 class="text-xl font-bold text-white mb-3 text-center">
            {characteristic.title}
          </h3>
          <p class="text-gray-300 text-center leading-relaxed mb-4">
            {characteristic.description}
          </p>
          
          <!-- Botón Saber más -->
          <div class="text-center">
            <button 
              onclick={() => openModal(characteristic.id)}
              class="px-4 py-2 bg-gradient-to-r {characteristic.color} text-white rounded-lg font-semibold text-sm transition-all duration-300 transform hover:scale-105 hover:shadow-lg"
            >
              Saber más
            </button>
          </div>
          
          <!-- Elemento decorativo -->
          <div class="mt-4 h-1 bg-gradient-to-r {characteristic.color} rounded-full mx-auto w-12"></div>
        </div>
      {/each}
    </div>
    
    <!-- Llamado a la acción -->
    <div class="text-center mt-16">
      <div class="inline-flex items-center space-x-2 text-cyan-400 font-semibold">
        <span>Desplázate hacia abajo para explorar los controladores interactivos</span>
        <svg class="w-5 h-5 animate-bounce" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path>
        </svg>
      </div>
    </div>
  </div>
</section>

<!-- Componente Modal -->
{#if activeModal}
  <Modal {activeModal} onClose={closeModal} />
{/if}

<style>
  @keyframes fade-in-up {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .animate-fade-in-up {
    animation: fade-in-up 0.6s ease-out forwards;
  }
</style>
