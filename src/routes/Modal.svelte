<script>
  import { onMount } from 'svelte';
  import MiniController from './MiniController.svelte';
  
  let { activeModal, onClose } = $props();
  let modalRef = $state();
  
  const modalContent = {
    difficulty: {
      title: "Visualización del Nivel de Dificultad",
      description: "La intensidad del movimiento del stick analógico representa cuán desafiante es dominar el juego.",
      details: [
        "Baja dificultad (1-3): Movimiento mínimo del stick, animaciones suaves",
        "Dificultad media (4-6): Movimiento moderado del stick, animaciones estables", 
        "Alta dificultad (7-10): Movimiento intenso del stick, animaciones rápidas"
      ],
      examples: [
        { name: "Animal Crossing", difficulty: 2, description: "Movimiento muy suave", type: "difficulty" },
        { name: "The Witcher 3", difficulty: 7, description: "Movimiento intenso", type: "difficulty" },
        { name: "Elden Ring", difficulty: 9, description: "Intensidad máxima", type: "difficulty" }
      ]
    },
    popularity: {
      title: "Visualización del Nivel de Popularidad",
      description: "La intensidad del resplandor alrededor de los controles muestra cuán popular y jugado es el juego.",
      details: [
        "Baja popularidad (0-50%): Efecto de resplandor mínimo o nulo",
        "Popularidad media (51-80%): Resplandor azul moderado alrededor del control",
        "Alta popularidad (81-100%): Resplandor brillante e intenso con mayor radio"
      ],
      examples: [
        { name: "Cyberpunk 2077", popularity: 75, description: "Resplandor moderado", type: "popularity" },
        { name: "Minecraft", popularity: 98, description: "Intensidad máxima de resplandor", type: "popularity" },
        { name: "Among Us", popularity: 85, description: "Efecto de resplandor fuerte", type: "popularity" }
      ]
    },
    genre: {
      title: "Colores por Género de Juego",
      description: "Cada género de juego tiene un esquema de color único aplicado al cuerpo del control.",
      details: [
        "Acción: De rojo a naranja - representa intensidad y energía",
        "Aventura: De verde a esmeralda - simboliza exploración y naturaleza",
        "RPG: De púrpura a violeta - indica magia y fantasía",
        "Estrategia: De azul a cian - representa lógica y planificación",
        "Deportes: De amarillo a ámbar - colores energéticos y competitivos",
        "Puzzle: De rosa a rosado - tonos creativos y reflexivos",
        "Carreras: De índigo a azul - colores de velocidad y movimiento",
        "Lucha: Rojo intenso - agresivo y competitivo",
        "Simulación: De teal a verde - tonos realistas y naturales"
      ],
      examples: [
        { name: "God of War", genre: "Acción", color: "de rojo a naranja", type: "genre" },
        { name: "The Witcher 3", genre: "RPG", color: "de púrpura a violeta", type: "genre" },
        { name: "FIFA 23", genre: "Deportes", color: "de amarillo a ámbar", type: "genre" },
        { name: "Portal 2", genre: "Puzzle", color: "de rosa a rosado", type: "genre" },
        { name: "League of Legends", genre: "Estrategia", color: "de azul a cian", type: "genre" },
        { name: "Zelda BOTW", genre: "Aventura", color: "de verde a esmeralda", type: "genre" }
      ]
    },
    rating: {
      title: "Visualización de Valoración de Usuarios",
      description: "La velocidad y brillo del pulso de los botones refleja la valoración y reseñas de los usuarios.",
      details: [
        "Valoración baja (1-5): Pulso lento y tenue en los botones",
        "Valoración media (6-8): Pulso de velocidad y brillo moderados",
        "Valoración alta (9-10): Animaciones rápidas y brillantes en los botones"
      ],
      examples: [
        { name: "Cyberpunk 2077", rating: 7.5, description: "Pulso moderado", type: "rating" },
        { name: "The Witcher 3", rating: 9.8, description: "Pulso rápido y brillante", type: "rating" },
        { name: "Breath of the Wild", rating: 9.7, description: "Máxima respuesta", type: "rating" }
      ]
    },
    year: {
      title: "Animación por Año de Lanzamiento",
      description: "La velocidad de animación indica cuándo se lanzó el juego; los juegos más nuevos tienen animaciones más rápidas.",
      details: [
        "Juegos clásicos (1980-1995): Animaciones lentas, estilo retro",
        "Juegos modernos (1996-2010): Velocidad de animación moderada",
        "Juegos contemporáneos (2011-2024): Animaciones rápidas y fluidas"
      ],
      examples: [
        { name: "Pac-Man", year: 1980, description: "Movimiento lento y nostálgico", type: "year" },
        { name: "The Witcher 3", year: 2015, description: "Velocidad de animación moderna", type: "year" },
        { name: "Elden Ring", year: 2022, description: "Movimiento rápido y contemporáneo", type: "year" }
      ]
    },
    platform: {
      title: "Tipos de Control por Plataforma",
      description: "Diferentes formas y estilos de control representan varias plataformas y épocas del gaming.",
      details: [
        "Nintendo: Controles redondeados con disposición distintiva de botones ABXY",
        "PlayStation: Diseño DualShock con botones de símbolos (△, ○, ✕, □)",
        "Xbox: Diseño ergonómico con botones ABXY de colores y forma única",
        "PC: Gamepad moderno con disposición de botones personalizable",
        "Arcade: Controles clásicos rectangulares con diseño simple de botones",
        "Retro: Controles vintage estilo NES/SNES rectangulares"
      ],
      examples: [
        { name: "Super Mario Odyssey", platform: "nintendo", description: "Diseño redondeado de Nintendo", type: "platform" },
        { name: "God of War", platform: "playstation", description: "Estilo DualShock de PlayStation", type: "platform" },
        { name: "Halo 3", platform: "xbox", description: "Forma ergonómica de Xbox", type: "platform" },
        { name: "Counter-Strike", platform: "pc", description: "Diseño de gamepad para PC", type: "platform" },
        { name: "Pac-Man", platform: "arcade", description: "Control clásico de arcade", type: "platform" },
        { name: "Super Mario Bros", platform: "retro", description: "Diseño retro estilo NES", type: "platform" }
      ]
    }
  };
  
  const currentContent = $derived(modalContent[activeModal]);
  
  onMount(() => {
    const handleEscape = (e) => {
      if (e.key === 'Escape') {
        onClose();
      }
    };
    
    document.addEventListener('keydown', handleEscape);
    document.body.style.overflow = 'hidden';
    
    return () => {
      document.removeEventListener('keydown', handleEscape);
      document.body.style.overflow = 'auto';
    };
  });
  
  function handleBackdropClick(e) {
    if (e.target === modalRef) {
      onClose();
    }
  }
</script>

<div 
  bind:this={modalRef}
  class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center z-50 p-4"
  onclick={handleBackdropClick}
  onkeydown={(e) => {
        if (e.key === 'Enter' || e.key === ' ') {
            handleBackdropClick(e);
        }
    }}
  role="dialog"
  aria-modal="true"
  tabindex="-1"
>
  <div class="bg-gray-800 rounded-xl max-w-5xl w-full max-h-[90vh] overflow-y-auto border border-gray-600 shadow-2xl">
    <!-- Modal Header -->
    <div class="sticky top-0 bg-gray-800 border-b border-gray-600 p-6 flex justify-between items-center">
      <h2 class="text-2xl font-bold text-white">{currentContent.title}</h2>
      <button 
        onclick={onClose}
        class="text-gray-400 hover:text-white transition-colors duration-200 text-2xl"
        aria-label="Close modal"
      >
        ✕
      </button>
    </div>
    
    <!-- Modal Content -->
    <div class="p-6">
      <!-- Description -->
      <p class="text-gray-300 text-lg mb-6 leading-relaxed">
        {currentContent.description}
      </p>
      
      <!-- Details Section -->
      <div class="mb-8">
        <h3 class="text-xl font-semibold text-white mb-4">¿Cómo funciona?</h3>
        <div class="space-y-3">
          {#each currentContent.details as detail}
            <div class="flex items-start space-x-3">
              <div class="w-2 h-2 bg-cyan-400 rounded-full mt-2 flex-shrink-0"></div>
              <p class="text-gray-300">{detail}</p>
            </div>
          {/each}
        </div>
      </div>
      
      <!-- Visual Examples Section -->
      <div>
        <h3 class="text-xl font-semibold text-white mb-4">Ejemplos visuales</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {#each currentContent.examples as example}
            <div class="bg-gray-700 rounded-lg p-4 border border-gray-600">
              <h4 class="font-semibold text-cyan-400 mb-3 text-center">{example.name}</h4>
              
              <!-- Mini Controller Visual -->
              <div class="flex justify-center mb-3">
                <MiniController {example} />
              </div>
              
              <div class="text-center">
                <p class="text-sm text-gray-300 mb-2">
                  {#if example.difficulty}
                    Dificultad: {example.difficulty}/10
                  {:else if example.popularity}
                    Popularidad: {example.popularity}%
                  {:else if example.rating}
                    Valoración: {example.rating}/10
                  {:else if example.year}
                    Lanzamiento: {example.year}
                  {:else if example.platform}
                    Plataforma: {example.platform}
                  {:else if example.genre}
                    Género: {example.genre}
                  {:else if example.color}
                    Color: {example.color}
                  {/if}
                </p>
                <p class="text-xs text-gray-400">{example.description}</p>
              </div>
            </div>
          {/each}
        </div>
      </div>
      
      <!-- Close Button -->
      <div class="mt-8 text-center">
        <button 
          onclick={onClose}
          class="px-6 py-3 bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 text-white rounded-lg font-semibold transition-all duration-300 transform hover:scale-105"
        >
          ¡Entendido!
        </button>
      </div>
    </div>
  </div>
</div>
