<script>
  import { onMount } from "svelte";
  let zEl;
  let restEl;
  let navEl;

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
    window.addEventListener("scroll", () => {
      if (window.scrollY > 50) {
        // al bajar más de 50px
        gsap.to(restEl, { opacity: 0, duration: 0.5 });
        gsap.to(navEl, {
          height: "6.5rem",
          position: "fixed",
          top: 0,
          left: 0,
          right: 0,
          backgroundColor: "white",
          color: "black",
          boxShadow: "0 4px 12px rgba(0,0,0,0.2)",
        });
      } else {
        gsap.to(restEl, { opacity: 1, duration: 0.5 });
        gsap.to(navEl, {
          height: "auto",
          position: "sticky",
          backgroundColor: "var(--spider-black)",
          color: "whitesmoke",
          boxShadow: "none",
        });
      }
    });
  });
</script>

<div class="welcome-nav" bind:this={navEl}>
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
    background-color: var(--spider-black);
    display: flex;
    flex-direction: column;
    align-items: center;
    color: whitesmoke;
    width: 100%;
    z-index: 100;
  }

  h1 {
    font-size: 5rem;
    margin-top: 0;
    margin-bottom: 0.1em;
    padding-bottom: 0;
  }
  h2 {
    font-size: 2rem;
    letter-spacing: 0.05em;
    margin-top: 0;
  }
</style>
