<script>
  import Hero from './Hero.svelte';
  import Explanation from './Explanation.svelte';
  import JoystickGrid from './JoystickGrid.svelte';
</script>

<main class="min-h-screen bg-gray-900">
  <Hero />
  <Explanation />
  <JoystickGrid />
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: #111827;
    color: #f9fafb;
    overflow-x: hidden;
  }

  :global(*) {
    box-sizing: border-box;
  }

  :global(.glow) {
    filter: drop-shadow(0 0 10px currentColor);
  }

  :global(.scanlines) {
    position: relative;
  }

  :global(.scanlines::before) {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
      transparent 50%, 
      rgba(0, 255, 0, 0.03) 50%
    );
    background-size: 100% 4px;
    pointer-events: none;
    animation: scanlines 0.1s linear infinite;
  }

  @keyframes scanlines {
    0% { transform: translateY(0); }
    100% { transform: translateY(4px); }
  }

  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-20px); }
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  @keyframes glow {
    0%, 100% { filter: drop-shadow(0 0 5px currentColor); }
    50% { filter: drop-shadow(0 0 20px currentColor); }
  }
</style>
