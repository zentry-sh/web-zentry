<script>
  import { onMount } from "svelte";
  let zEl;
  let restEl;

  onMount(async () => {
    const { gsap } = await import("gsap");
    // Animar {Z} de grande a pequeño
    await gsap.fromTo(
      zEl,
      { scale: 3, opacity: 0 },
      { scale: 1, opacity: 1, duration: 1, ease: "power1.out" }
    );
    // Animar el resto del texto juntando las letras (después de la animación de {Z})
    gsap.fromTo(
      restEl,
      { letterSpacing: "2em", opacity: 0 },
      {
        letterSpacing: "0.28em",
        opacity: 1,
        duration: 1.2,
        ease: "power1.inOut",
        delay: 0.2,
      }
    );
  });
</script>

<div class="welcome-nav">
  <h1>
    <span bind:this={zEl} style="display:inline-block;">{"{ Z }"}</span>
  </h1>
  <h2>
    <span bind:this={restEl} style="display:inline-block; opacity:0;">
      Zentry.sh
    </span>
  </h2>
</div>

<style>
  .welcome-nav {
    border: 3px dashed #ff9800;
    background: rgba(20, 20, 20, 0.7);
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    width: 100%;
    height: 100%;
    color: whitesmoke;
  }
  h1,
  h2 {
    font-family: "CMU Typewriter Text", monospace;
  }
  h1 {
    font-size: 5rem;
    margin-bottom: 0.2rem;
  }
  h2 {
    font-size: 2rem;
    letter-spacing: 15px;
    margin: 0%;
  }
</style>
