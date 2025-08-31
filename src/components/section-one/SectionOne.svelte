<script>
  import {} from "./SectionOne.scss";
  let cards = [
    {
      id: 1,
      text: "¿Quiénes somos y qué ofrecemos?",
      respuesta:
        " Somos Zentry.sh, una startup fundada por cinco jovenes entusiastas del desarrollo de software y la innovacion. Nos dedicamos al desarrollo y consultaria de software, usando como enfoque principal la experiencia del usuario y la vanguardia tecnologica.",
      color: "#040200",
    },
    {
      id: 2,
      text: "Misión y Visión",
      respuesta:
        "Nuestra misión es apoyar a las empresas de cualquier sector a resolver sus necesidades digitales de manera automatizada, eficiente y autónoma. Tenemos una vision diferente a la mayoria de las soluciones del mercado, en Zentry.sh entendemos que la mayoria de los grandes emprezarios no tienen por que ser expertos en tecnologia.",
      color: "#040200",
    },
    {
      id: 3,
      text: "Lema Zentry",
      respuesta:
        "Nos distinguimos en escuchar y entender a fondo tus necesidades, ofreciéndote soluciones personalizadas, de esta manera, evitamos gastos innecesarios y aseguramos tus resultados óptimos.",
      color: "#040200",
    },
  ];

  let currentIndex = 0;

  function next() {
    currentIndex = (currentIndex + 1) % cards.length;
  }

  function prev() {
    currentIndex = (currentIndex - 1 + cards.length) % cards.length;
  }

  // Posiciones relativas: -1 = prev, 0 = active, 1 = next
  $: positions = cards.map((_, i) => {
    let offset = i - currentIndex;
    if (offset < -1) offset += cards.length;
    if (offset > 1) offset -= cards.length;
    return offset;
  });
</script>

<div>
  <div class="slider">
    {#each cards as card, i}
      {@const x = positions[i] * 320}
      <div
        class="card"
        style="
        left: 50%;
        transform: translateX(calc({x}px - 50%)) scale({positions[i] === 0
          ? 1.1
          : 0.7});
        opacity: {positions[i] === 0 ? 1 : 1};
        z-index: {10 - Math.abs(positions[i])};
        background: {positions[i] === 0 ? card.color : '#191C27'};
        <!-- border: {positions[i] === 0 ? '6px solid #5B2C6F' : 'none'}; -->
        
      "
      >
        <h1>{card.text}</h1>
        <p>{card.respuesta}</p>
      </div>
    {/each}
  </div>

  <div class="controls">
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <button on:click={prev}>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="currentColor"
        class="icon icon-tabler icons-tabler-filled icon-tabler-arrow-big-left"
        ><path stroke="none" d="M0 0h24v24H0z" fill="none" /><path
          d="M9.586 4l-6.586 6.586a2 2 0 0 0 0 2.828l6.586 6.586a2 2 0 0 0 2.18 .434l.145 -.068a2 2 0 0 0 1.089 -1.78v-2.586h7a2 2 0 0 0 2 -2v-4l-.005 -.15a2 2 0 0 0 -1.995 -1.85l-7 -.001v-2.585a2 2 0 0 0 -3.414 -1.414z"
        /></svg
      >
    </button>
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <button on:click={next}>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="currentColor"
        class="icon icon-tabler icons-tabler-filled icon-tabler-arrow-big-right"
      >
        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
        <path
          d="M14.414 4l6.586 6.586a2 2 0 0 1 0 2.828l-6.586 6.586a2 2 0 0 1-2.18 .434l-.145 -.068a2 2 0 0 1-1.089 -1.78v-2.586h-7a2 2 0 0 1-2 -2v-4l.005 -.15a2 2 0 0 1 1.995 -1.85l7 -.001v-2.585a2 2 0 0 1 3.414 -1.414z"
        />
      </svg>
    </button>
  </div>
</div>
