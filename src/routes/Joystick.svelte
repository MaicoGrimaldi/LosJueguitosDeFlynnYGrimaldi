<script>
  import { onMount } from 'svelte';
  import SvgJoystick from './SvgJoystick.svelte';
  
  export let game;
  
  let controllerRef;
  let isHovered = false;
  let isPressed = false;
  let leftStickPosition = { x: 0, y: 0 };
  let rightStickPosition = { x: 0, y: 0 };
  let animationFrame = 0;
  
  // Check if this should use SVG controller (PlayStation games)
  const PS = game.platform && game.platform.includes('PlayStation');
  const Xbox = game.platform && game.platform.includes('Xbox');
  const Nintendo = game.platform && game.platform.includes('Nintendo');
  const Arcade = game.platform && game.platform.includes('Arcade');
  const Retro = game.platform && game.platform.includes('Retro');
  const PC = game.platform && game.platform.includes('PC');

  
  // Derived values based on game data
  const difficulty = game.difficulty / 10; // 0-1
  const popularity = game.popularity / 100; // 0-1
  const rating = game.rating / 10; // 0-1
  const releaseYear = game.releaseYear;
  const normalizedYear = (releaseYear - 1980) / (2024 - 1980); // 0-1
  
  // Controller style based on platform and era
  const getControllerStyle = (() => {
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
  })();
  
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

  
  /** @ts-ignore */
  const genreColor = genreColors[game.genre] || 'from-gray-500 to-gray-400';

</script>

<!-- Use SVG controller for PlayStation games -->
{#if PS}
  <SvgJoystick {game} />
{:else if Xbox}
  <SvgJoystick {game} />
{:else if Nintendo}
  <SvgJoystick {game} />
{:else if Arcade}
  <SvgJoystick {game} />
{:else if Retro}
  <SvgJoystick {game} />
{:else if PC}
  <SvgJoystick {game} />

{/if}

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
