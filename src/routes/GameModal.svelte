<script>
  import { onMount } from 'svelte';
  
  let { game, onClose } = $props();
  let modalRef = $state();
  
  // Game synopses and additional content
  const gameDetails = {
    1: {
      synopsis: "Una aventura épica en mundo abierto donde Link despierta en un reino devastado. Explora libremente Hyrule, resuelve puzzles creativos y enfrenta enemigos usando física realista y herramientas innovadoras.",
      features: ["Mundo abierto masivo", "Física interactiva", "Escalada libre", "Cocina y crafting"],
      funFact: "Puedes completar el juego directamente después del tutorial, saltándote todo el contenido principal.",
      awards: "Juego del Año 2017 - The Game Awards"
    },
    2: {
      synopsis: "Mario viaja por reinos fantásticos con su nueva habilidad de 'captura' usando su gorra Cappy. Controla enemigos, objetos y descubre secretos en cada reino único.",
      features: ["Mecánica de captura única", "Reinos diversos", "Modo cooperativo", "Lunas coleccionables"],
      funFact: "Hay más de 880 lunas para coleccionar en todo el juego.",
      awards: "Mejor Juego de Plataformas 2017"
    },
    3: {
      synopsis: "Kratos y su hijo Atreus emprenden un viaje emocional por los reinos nórdicos. Una historia madura sobre paternidad, venganza y redención con combate visceral.",
      features: ["Combate cuerpo a cuerpo", "Narrativa cinematográfica", "Mitología nórdica", "Relación padre-hijo"],
      funFact: "Todo el juego está filmado en una sola toma continua sin cortes.",
      awards: "Juego del Año 2018 - The Game Awards"
    },
    4: {
      synopsis: "La épica historia de Arthur Morgan y la banda de Dutch van der Linde en el ocaso del Viejo Oeste. Un mundo vivo con detalles increíbles y decisiones morales complejas.",
      features: ["Mundo abierto detallado", "Sistema de honor", "Actividades variadas", "Narrativa ramificada"],
      funFact: "Los caballos tienen personalidades únicas y recuerdan cómo los tratas.",
      awards: "Mejor Narrativa 2018"
    },
    5: {
      synopsis: "Geralt de Rivia busca a su hija adoptiva Ciri en un mundo de fantasía oscura. RPG con decisiones consecuentes, combate táctico y misiones secundarias memorables.",
      features: ["Decisiones impactantes", "Combate con signos", "Mundo gigantesco", "DLCs épicos"],
      funFact: "Tiene más de 100 horas de contenido principal y secundario.",
      awards: "Más de 250 premios Juego del Año"
    },
    6: {
      synopsis: "El juego sandbox definitivo donde tu imaginación es el límite. Construye, explora, sobrevive y crea en mundos infinitos generados proceduralmente.",
      features: ["Creatividad infinita", "Multijugador", "Mods", "Mundos infinitos"],
      funFact: "Es el videojuego más vendido de la historia con más de 300 millones de copias.",
      awards: "Fenómeno Cultural Global"
    },
    7: {
      synopsis: "Battle royale gratuito donde 100 jugadores luchan por ser el último en pie. Construye estructuras, encuentra armas y sobrevive en una isla que se reduce constantemente.",
      features: ["Battle Royale", "Construcción", "Eventos en vivo", "Crossover épicos"],
      funFact: "Ha tenido conciertos virtuales con más de 12 millones de espectadores simultáneos.",
      awards: "Fenómeno de la Cultura Pop"
    },
    8: {
      synopsis: "Juego de deducción social donde tripulantes completan tareas mientras impostores sabotean y eliminan jugadores. La comunicación y el engaño son clave.",
      features: ["Deducción social", "Multijugador online", "Mapas variados", "Personalización"],
      funFact: "Se volvió viral durante la pandemia con streamers famosos jugándolo.",
      awards: "Mejor Juego Móvil 2020"
    },
    9: {
      synopsis: "RPG futurista en Night City, una metrópolis cyberpunk. Juega como V, un mercenario que busca un implante único que es la clave de la inmortalidad.",
      features: ["Mundo cyberpunk", "Personalización profunda", "Múltiples finales", "Combate híbrido"],
      funFact: "Keanu Reeves interpretó a Johnny Silverhand con captura de movimiento completa.",
      awards: "Mejor RPG 2020 (post-actualizaciones)"
    },
    10: {
      synopsis: "Escapa a una isla tropical donde puedes crear tu paraíso personal. Decora, socializa con vecinos animales y disfruta de actividades relajantes.",
      features: ["Personalización total", "Vida social", "Eventos estacionales", "Creatividad"],
      funFact: "Fue el juego perfecto durante la cuarentena, vendiendo 13 millones de copias en 6 semanas.",
      awards: "Mejor Juego Familiar 2020"
    },
    11: {
      synopsis: "Shooter táctico realista que reinventó la franquicia Call of Duty. Campaña intensa y multijugador competitivo con mecánicas modernas.",
      features: ["Realismo táctico", "Multijugador intenso", "Warzone", "Personalización"],
      funFact: "Warzone alcanzó 100 millones de jugadores en su primer año.",
      awards: "Mejor Shooter Multijugador 2019"
    },
    12: {
      synopsis: "La simulación de fútbol más realista del mundo. Juega con los mejores equipos y jugadores en múltiples modos de juego competitivos.",
      features: ["Licencias oficiales", "Ultimate Team", "Carrera", "Física realista"],
      funFact: "Tiene más de 700 equipos licenciados de todo el mundo.",
      awards: "Mejor Juego de Deportes 2022"
    },
    13: {
      synopsis: "El puzzle game más icónico de la historia. Organiza bloques que caen para formar líneas completas en este adictivo juego atemporal.",
      features: ["Gameplay atemporal", "Múltiples modos", "Competitivo", "Fácil de aprender"],
      funFact: "Ha vendido más de 520 millones de copias en todas las plataformas.",
      awards: "Patrimonio Cultural de la Humanidad"
    },
    14: {
      synopsis: "El arcade clásico donde controlas a Pac-Man comiendo puntos mientras evitas fantasmas coloridos. Simple pero adictivo.",
      features: ["Gameplay icónico", "Laberintos", "Power pellets", "Puntuaciones altas"],
      funFact: "Fue inspirado por una pizza a la que le faltaba una rebanada.",
      awards: "Icono Cultural de los Videojuegos"
    },
    15: {
      synopsis: "El fighting game que definió el género. Elige entre luchadores únicos con movimientos especiales y combate en torneos mundiales.",
      features: ["Combos especiales", "Personajes icónicos", "Competitivo", "Movimientos únicos"],
      funFact: "Ryu's Hadoken se ha convertido en el movimiento más famoso de los videojuegos.",
      awards: "Revolucionó los Fighting Games"
    },
    16: {
      synopsis: "La aventura que salvó la industria de los videojuegos. Mario debe rescatar a la Princesa Peach del malvado Bowser en el Reino Champiñón.",
      features: ["Plataformas clásicas", "Power-ups", "Mundos secretos", "Gameplay perfecto"],
      funFact: "Salvó la industria de los videojuegos después del crash de 1983.",
      awards: "Juego Más Influyente de la Historia"
    },
    17: {
      synopsis: "Sandbox criminal en Los Santos donde puedes vivir una vida de crimen o simplemente explorar la ciudad más detallada jamás creada.",
      features: ["Mundo abierto masivo", "Tres protagonistas", "GTA Online", "Libertad total"],
      funFact: "Ha generado más de 6 mil millones de dólares, más que cualquier entretenimiento.",
      awards: "Juego Más Exitoso Comercialmente"
    },
    18: {
      synopsis: "Shooter de héroes donde equipos de 6 jugadores luchan en objetivos usando personajes únicos con habilidades especiales.",
      features: ["Héroes únicos", "Trabajo en equipo", "Competitivo", "Actualizaciones constantes"],
      funFact: "Tiene más de 35 héroes diferentes, cada uno con estilo de juego único.",
      awards: "Mejor Shooter Multijugador 2016"
    },
    19: {
      synopsis: "Puzzle game en primera persona donde usas un arma de portales para resolver acertijos en laboratorios de Aperture Science.",
      features: ["Mecánica de portales", "Puzzles ingeniosos", "Humor negro", "GLaDOS"],
      funFact: "La canción 'Still Alive' se volvió un meme viral de internet.",
      awards: "Mejor Diseño de Juego 2011"
    },
    20: {
      synopsis: "El Master Chief continúa su lucha contra el Covenant en esta épica conclusión de la trilogía original de Halo.",
      features: ["Campaña épica", "Multijugador icónico", "Forge Mode", "Teatro"],
      funFact: "Su beta fue la más jugada en Xbox Live hasta ese momento.",
      awards: "Mejor Shooter de Consola 2007"
    },
    21: {
      synopsis: "MMORPG que definió el género. Explora Azeroth, únete a guilds, completa raids épicos y vive aventuras en un mundo persistente.",
      features: ["Mundo persistente", "Raids épicos", "Guilds", "PvP"],
      funFact: "Ha tenido más de 100 millones de cuentas creadas en su historia.",
      awards: "MMORPG Más Influyente"
    },
    22: {
      synopsis: "Shooter táctico competitivo donde dos equipos luchan en rondas eliminatorias. Precisión, estrategia y trabajo en equipo son esenciales.",
      features: ["Competitivo puro", "Economía de armas", "Mapas icónicos", "Esports"],
      funFact: "Tiene una de las escenas de esports más grandes del mundo.",
      awards: "Mejor Juego de Esports"
    },
    23: {
      synopsis: "MOBA donde dos equipos de 5 campeones luchan para destruir el Nexus enemigo. Estrategia, habilidad y trabajo en equipo definen cada partida.",
      features: ["Más de 160 campeones", "Estrategia profunda", "Esports masivo", "Actualizaciones constantes"],
      funFact: "Su Mundial 2022 tuvo más de 5 millones de espectadores simultáneos.",
      awards: "Esport Más Grande del Mundo"
    },
    24: {
      synopsis: "Fútbol con coches propulsados por cohetes. Combina deportes y conducción en partidas frenéticas donde cualquier cosa puede pasar.",
      features: ["Física única", "Competitivo", "Personalización", "Fácil de jugar"],
      funFact: "Comenzó como un mod y se convirtió en un fenómeno de esports.",
      awards: "Mejor Juego Independiente 2015"
    },
    25: {
      synopsis: "Battle royale colorido donde 60 jugadores compiten en minijuegos eliminatorios hasta que solo uno quede en pie.",
      features: ["Minijuegos variados", "Estilo colorido", "Diversión casual", "Temporadas"],
      funFact: "Alcanzó 100 millones de descargas en solo 2 meses.",
      awards: "Fenómeno Viral 2020"
    },
    26: {
      synopsis: "Shooter táctico 5v5 donde agentes con habilidades únicas plantan o desactivan la Spike en mapas estratégicos.",
      features: ["Agentes únicos", "Táctico puro", "Competitivo", "Anti-cheat robusto"],
      funFact: "Fue desarrollado por el equipo de League of Legends en Riot Games.",
      awards: "Mejor Shooter Táctico 2020"
    },
    27: {
      synopsis: "Battle royale donde equipos de 3 leyendas luchan en Kings Canyon usando habilidades únicas y trabajo en equipo.",
      features: ["Leyendas únicas", "Sistema de respawn", "Comunicación inteligente", "Evolución constante"],
      funFact: "Alcanzó 50 millones de jugadores en su primer mes.",
      awards: "Mejor Battle Royale 2019"
    },
    28: {
      synopsis: "La experiencia de carreras definitiva de Mario con todos los personajes, pistas y modos. Diversión garantizada para toda la familia.",
      features: ["48 pistas", "Multijugador local", "Batalla de globos", "DLC gratuito"],
      funFact: "Es el juego de carreras más vendido de todos los tiempos.",
      awards: "Mejor Juego de Carreras 2017"
    },
    29: {
      synopsis: "RPG de mundo abierto en las Tierras Intermedias. Un mundo oscuro lleno de secretos, jefes épicos y una historia fragmentada que debes descubrir.",
      features: ["Mundo abierto oscuro", "Jefes legendarios", "Construcción de personaje", "Cooperativo"],
      funFact: "Vendió 12 millones de copias en sus primeras 3 semanas.",
      awards: "Juego del Año 2022 - The Game Awards"
    },
    30: {
      synopsis: "RPG de mundo abierto gratuito en el mundo de Teyvat. Explora, colecciona personajes elementales y descubre una historia épica.",
      features: ["Mundo hermoso", "Sistema gacha", "Elementos", "Multijugador cooperativo"],
      funFact: "Ha generado más de 3 mil millones de dólares en móviles.",
      awards: "Mejor Juego Móvil 2020"
    }
  };
  
  const currentDetails = $derived(gameDetails[game.id] || {
    synopsis: "Una experiencia de juego única que ha cautivado a millones de jugadores.",
    features: ["Gameplay innovador", "Gráficos impresionantes", "Historia envolvente"],
    funFact: "Este juego ha dejado una marca indeleble en la industria.",
    awards: "Reconocido por la crítica"
  });
  
  // Genre color mapping
  const genreColors = {
    'Acción': '#ef4444',        // rojo
    'Aventura': '#22c55e',      // verde más brillante, distinto de Simulación
    'RPG': '#7c3aed',           // violeta más oscuro (para alejarlo de Carreras)
    'Estrategia': '#06b6d4',    // cian
    'Deportes': '#f59e0b',      // amarillo
    'Puzzle': '#ec4899',        // rosa
    'Carreras': '#3b82f6',      // azul vibrante, más diferenciado del violeta
    'Lucha': '#dc2626',         // rojo oscuro
    'Simulación': '#0d9488'     // verde azulado más oscuro, diferente de Aventura
  }

  
  const genreColor = $derived(genreColors[game.genre] || 'from-gray-500 to-gray-400');
  
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
  class="fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center z-50 p-4"
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
  <div class="bg-gray-800 rounded-xl max-w-4xl w-full max-h-[90vh] overflow-y-auto border border-gray-600 shadow-2xl">
    <!-- Modal Header -->
    <div class="relative">
      <!-- Game Cover Background -->
      <div class="h-48 bg-gradient-to-br {genreColor} relative overflow-hidden">
        <div class="absolute inset-0 bg-black bg-opacity-40"></div>
        <!-- Game Cover Placeholder -->
        <div class="absolute inset-0 flex items-center justify-center">
          <img 
            src="/placeholder.svg?height=200&width=300&text={encodeURIComponent(game.name)}" 
            alt="Portada de {game.name}"
            class="w-32 h-44 object-cover rounded-lg border-2 border-white shadow-2xl"
          />
        </div>
        <!-- Close button -->
        <button 
          onclick={onClose}
          class="absolute top-4 right-4 text-white hover:text-gray-300 transition-colors duration-200 text-2xl bg-black bg-opacity-50 rounded-full w-10 h-10 flex items-center justify-center"
          aria-label="Cerrar modal"
        >
          ✕
        </button>
      </div>
      
      <!-- Game Title Overlay -->
      <div class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black to-transparent p-6">
        <h2 class="text-3xl font-bold text-white mb-2">{game.name}</h2>
        <div class="flex items-center space-x-4 text-sm">
          <span class="px-3 py-1 bg-gradient-to-r {genreColor} text-white rounded-full font-semibold">
            {game.genre}
          </span>
          <span class="text-gray-300">{game.platform}</span>
          <span class="text-gray-300">{game.releaseYear}</span>
        </div>
      </div>
    </div>
    
    <!-- Modal Content -->
    <div class="p-6">
      <!-- Game Stats -->
      <div class="grid grid-cols-3 gap-4 mb-6">
        <div class="text-center p-4 bg-gray-700 rounded-lg">
          <div class="text-2xl font-bold text-yellow-400">{game.rating}/10</div>
          <div class="text-sm text-gray-300">Valoración</div>
        </div>
        <div class="text-center p-4 bg-gray-700 rounded-lg">
          <div class="text-2xl font-bold text-red-400">{game.difficulty}/10</div>
          <div class="text-sm text-gray-300">Dificultad</div>
        </div>
        <div class="text-center p-4 bg-gray-700 rounded-lg">
          <div class="text-2xl font-bold text-green-400">{game.popularity}%</div>
          <div class="text-sm text-gray-300">Popularidad</div>
        </div>
      </div>
      
      <!-- Synopsis -->
      <div class="mb-6">
        <h3 class="text-xl font-semibold text-white mb-3 flex items-center">
          <span class="mr-2">📖</span>
          Sinopsis
        </h3>
        <p class="text-gray-300 leading-relaxed">
          {currentDetails.synopsis}
        </p>
      </div>
      
      <!-- Key Features -->
      <div class="mb-6">
        <h3 class="text-xl font-semibold text-white mb-3 flex items-center">
          <span class="mr-2">⭐</span>
          Características Principales
        </h3>
        <div class="grid grid-cols-2 gap-3">
          {#each currentDetails.features as feature}
            <div class="flex items-center space-x-2 text-gray-300">
              <div class="w-2 h-2 bg-cyan-400 rounded-full"></div>
              <span>{feature}</span>
            </div>
          {/each}
        </div>
      </div>
      
      <!-- Fun Fact -->
      <div class="mb-6 p-4 bg-gradient-to-r from-purple-900 to-blue-900 rounded-lg border border-purple-500">
        <h3 class="text-lg font-semibold text-white mb-2 flex items-center">
          <span class="mr-2">🎉</span>
          Dato Curioso
        </h3>
        <p class="text-purple-200">
          {currentDetails.funFact}
        </p>
      </div>
      
      <!-- Awards -->
      <div class="mb-6">
        <h3 class="text-xl font-semibold text-white mb-3 flex items-center">
          <span class="mr-2">🏆</span>
          Reconocimientos
        </h3>
        <div class="p-3 bg-yellow-900 bg-opacity-30 rounded-lg border border-yellow-500">
          <p class="text-yellow-200 font-semibold">{currentDetails.awards}</p>
        </div>
      </div>
      
      <!-- Action Buttons -->
      <div class="flex flex-col sm:flex-row gap-3 mt-8">
        <button 
          onclick={onClose}
          class="flex-1 px-6 py-3 bg-gradient-to-r {genreColor} text-white rounded-lg font-semibold transition-all duration-300 transform hover:scale-105 hover:shadow-lg"
        >
          ¡Quiero Jugarlo!
        </button>
        <button 
          onclick={onClose}
          class="flex-1 px-6 py-3 border-2 border-gray-600 text-gray-300 hover:bg-gray-700 rounded-lg font-semibold transition-all duration-300"
        >
          Cerrar
        </button>
      </div>
    </div>
  </div>
</div>
