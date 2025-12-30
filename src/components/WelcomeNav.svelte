<script>
  import { onMount } from "svelte";
  let zEl;
  let restEl;
  let navEl;

  onMount(async () => {
    const { gsap } = await import("gsap");

    const tl = gsap.timeline({
      onComplete: () => {
        gsap.to(navEl, {
          opacity: 0,
          duration: 0.8,
          ease: "power2.inOut",
          onComplete: () => {
            navEl.style.display = "none";
          },
        });
      },
    });

    // 1. Initial state
    gsap.set(zEl, { opacity: 0, scale: 3 });
    gsap.set(restEl, { opacity: 0, letterSpacing: "1em" });

    // 2. Animate { Z }
    tl.to(zEl, {
      scale: 1,
      opacity: 1,
      duration: 1,
      ease: "power3.out",
    })
      // 3. Animate rest of text
      .to(
        restEl,
        {
          letterSpacing: "0.05em",
          opacity: 1,
          duration: 1.2,
          ease: "power3.inOut",
        },
        "-=0.5",
      )
      // 4. Hold for a moment
      .to({}, { duration: 0.5 });
  });
</script>

<div class="welcome-loader" bind:this={navEl}>
  <div class="content">
    <h1>
      <span bind:this={zEl} class="logo-z">{"{ Z }"}</span>
    </h1>
    <h2>
      <span bind:this={restEl} class="brand-text"> Zentry.sh </span>
    </h2>
  </div>
</div>

<style>
  .welcome-loader {
    font-family: "CMU Typewriter Text", monospace;
    position: fixed;
    inset: 0;
    background-color: #000000;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
    color: white;
  }

  .content {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  h1 {
    font-size: clamp(4rem, 10vw, 8rem);
    margin: 0;
    line-height: 1;
    font-weight: 700;
  }

  h2 {
    font-size: clamp(1.5rem, 4vw, 3rem);
    margin: 1rem 0 0;
    font-weight: 400;
    white-space: nowrap;
  }

  .logo-z {
    display: inline-block;
    background: linear-gradient(135deg, #8b5cf6, #3b82f6);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .brand-text {
    display: inline-block;
    color: #94a3b8;
  }
</style>
