<script>
  import { onMount, onDestroy } from "svelte";

  export let target = 4; // número final (prop opcional)
  let count = 0;
  let rootEl; // elemento observado
  let tween = null;
  let gsap;

  // objeto que animamos (GSAP no puede animar directamente "count" porque es reactivo de Svelte)
  const obj = { val: 0 };

  function startTween() {
    // asegurarnos de resetear antes de iniciar
    resetTween();

    // crear tween y guardarlo para poder matarlo después
    tween = gsap.to(obj, {
      val: target,
      duration: 2.2,
      ease: "power1.out",
      onUpdate() {
        // floor para mostrar enteros; podés usar Math.round si preferís
        count = Math.floor(obj.val);
      },
      onComplete() {
        tween = null;
      },
    });
  }

  function resetTween() {
    if (tween) {
      tween.kill();
      tween = null;
    }
    obj.val = 0;
    count = 0;
  }

  onMount(async () => {
    // import dinámico de GSAP solo en cliente
    const mod = await import("gsap");
    gsap = mod.gsap ?? mod.default ?? mod;

    // IntersectionObserver: cuando entra en viewport -> start; cuando sale -> reset
    const io = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            // entra: iniciar animación (puede volver a iniciarse si ya se reseteó)
            startTween();
          } else {
            // sale: resetear para que la próxima vez empiece desde 0
            resetTween();
          }
        });
      },
      {
        root: null,
        rootMargin: "0px",
        threshold: 0.35, // cambia a lo que quieras: 0.5 = 50% visible
      }
    );

    io.observe(rootEl);

    // opcional: reiniciar también cuando la pestaña vuelve a ser visible (back/forward)
    const onVis = () => {
      if (!document.hidden) {
        // si el elemento está visible ahora, iniciar; si no, no hacemos nada
        const rect = rootEl.getBoundingClientRect();
        const inView = rect.top < window.innerHeight && rect.bottom > 0;
        if (inView) startTween();
      } else {
        // si oculto: reset para evitar animación en background
        resetTween();
      }
    };
    document.addEventListener("visibilitychange", onVis, { passive: true });

    onDestroy(() => {
      io.disconnect();
      document.removeEventListener("visibilitychange", onVis);
      resetTween();
    });
  });

  onDestroy(() => {
    // por si algo quedó
    if (tween) {
      tween.kill();
      tween = null;
    }
  });
</script>

<section class="section-two" bind:this={rootEl} aria-live="polite">
  <div class="wrap">
    <h2 style="color: aliceblue;">
      Hemos ayudado a más de
      <span class="count" aria-hidden="true">{count}</span>
      <span class="sr-only">{count} personas</span>
      <span>personas</span>
    </h2>
  </div>
</section>

<style>
  .section-two {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 3rem 1rem;
  }
  .wrap {
    text-align: center;
  }
  h2 {
    font-size: clamp(1.25rem, 3.5vw, 2rem);
    color: #0f172a;
    margin: 0;
  }
  .count {
    color: #6c2dc7;
    font-weight: 800;
    font-size: clamp(1.6rem, 5vw, 3rem);
    margin: 0 0.4rem;
    display: inline-block;
    min-width: 3ch;
  }
  .sr-only {
    position: absolute;
    left: -9999px;
    width: 1px;
    height: 1px;
    overflow: hidden;
  }
</style>
