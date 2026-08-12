import './style.css'

import foto1 from './assets/photos/foto1.jpg'
import foto2 from './assets/photos/foto2.jpg'
import foto3 from './assets/photos/foto3.jpg'
import foto4 from './assets/photos/foto4.jpg'
import foto5 from './assets/photos/foto5.jpg'

import cancion1 from './assets/music/cancion1.mp3'
import cancion2 from './assets/music/cancion2.mp3'
import cancion3 from './assets/music/cancion3.mp3'

// =========================
// DATOS DE LA GALERÍA
// =========================

const fotos = [
  foto1,
  foto2,
  foto3,
  foto4,
  foto5
]

// =========================
// DATOS DE LA MÚSICA
// =========================

const canciones = [
  {
    archivo: cancion1,
    nombre: 'CANCION UNO'
  },

  {
    archivo: cancion2,
    nombre: 'CANCION DOS'
  },

  {
    archivo: cancion3,
    nombre: 'CANCION TRES'
  }
]

let cancionActual = 0
// Foto que estamos viendo actualmente
let fotoActual = 0


// =========================
// INTERFAZ
// =========================

document.querySelector('#app').innerHTML = `
  <main class="app">

    <header class="header">
      <div class="logo">
        <span class="logo-wine">🍷</span>
        <h1>Anthony</h1>
        <span class="logo-wine">🍷</span>
      </div>

      <p class="subtitle">😈 vamoo a chupa culiaaauuu 😈</p>
      <p class="subtitle">a los gorriados no les gusta la mona</p>
    </header>


    <section class="gallery">

      <button
        class="gallery-button gallery-button-left"
        id="previous-photo"
        aria-label="Foto anterior">
        ‹
      </button>


      <div class="photo-container">

        <div class="photo">
          <img
            id="current-photo"
            src="${fotos[fotoActual]}"
            alt="Fotografía de VINASO"
          >
        </div>

        <div class="photo-info">
          <span class="photo-number" id="photo-number">
            01 / ${fotos.length}
          </span>
        </div>

      </div>


      <button
        class="gallery-button gallery-button-right"
        id="next-photo"
        aria-label="Foto siguiente">
        ›
      </button>


      <div class="photo-dots" id="photo-dots">

        ${fotos.map((_, index) => `
          <span
            class="dot ${index === 0 ? 'active' : ''}">
          </span>
        `).join('')}

      </div>

    </section>


    <section class="controls">

      <a
          class="control-button wine-button"
          aria-label="Vino" 
          href="https://wa.me/5493549415460?text=anthonyy%20vamos%20a%20chupaaa%20un%20vino%F0%9F%8D%B7%20"
          target="_blank" 
          rel="noopener noreferrer"> 💬
      </a>

      <button
        class="control-button play-button"
        id="play-button"
        aria-label="Reproducir">
        ▶
      </button>

      <button
        class="control-button music-button"
        aria-label="Cambiar canción">
        >♪
      </button>

    </section>


<section class="music-player">

  <div class="music-icon">
    ♫
  </div>

  <div class="song-info">
    <span class="song-label">
      REPRODUCIENDO
    </span>

    <span
      class="song-title"
      id="song-title">
      ${canciones[cancionActual].nombre}
    </span>
  </div>

</section>


    <div class="wine-box wine-box-left">
      <span>VINASO</span>
    </div>

    <div class="wine-box wine-box-right">
      <span>VINASO</span>
    </div>


    <nav class="bottom-navigation">

      <button class="nav-button active">
        <span class="nav-icon">▧</span>
        <span>GALERÍA</span>
      </button>

      <button class="nav-button">
        <span class="nav-icon">🍷</span>
        <span>VINASO</span>
      </button>

      <button class="nav-button">
        <span class="nav-icon">♫</span>
        <span>MÚSICA</span>
      </button>

    </nav>

  </main>
`
// =========================
// REPRODUCTOR DE AUDIO
// =========================

const audio = new Audio(
  canciones[cancionActual].archivo
)

// =========================
// ELEMENTOS
// =========================

const currentPhoto =
  document.querySelector('#current-photo')

const photoNumber =
  document.querySelector('#photo-number')

const dots =
  document.querySelectorAll('.dot')

const previousButton =
  document.querySelector('#previous-photo')

const nextButton =
  document.querySelector('#next-photo')

const playButton =
  document.querySelector('#play-button')

const musicButton =
  document.querySelector('.music-button')

const songTitle =
  document.querySelector('#song-title')

playButton.addEventListener('click', () => {

  if (audio.paused) {

    audio.play()

    playButton.textContent = '❚❚'

  } else {

    audio.pause()

    playButton.textContent = '▶'

  }

})

musicButton.addEventListener('click', () => {

  cancionActual++

  if (cancionActual >= canciones.length) {
    cancionActual = 0
  }

  audio.pause()

  audio.src =
    canciones[cancionActual].archivo

  songTitle.textContent =
    canciones[cancionActual].nombre

  audio.play()

  playButton.textContent = '❚❚'

})

audio.addEventListener('ended', () => {

  cancionActual++

  if (cancionActual >= canciones.length) {
    cancionActual = 0
  }

  audio.src =
    canciones[cancionActual].archivo

  songTitle.textContent =
    canciones[cancionActual].nombre

  audio.play()

})

// =========================
// MOSTRAR FOTO
// =========================

function mostrarFoto() {

  currentPhoto.src = fotos[fotoActual]

  photoNumber.textContent =
    `${String(fotoActual + 1).padStart(2, '0')} / ${fotos.length}`


  dots.forEach((dot, index) => {

    dot.classList.toggle(
      'active',
      index === fotoActual
    )

  })
}


// =========================
// FOTO SIGUIENTE
// =========================

nextButton.addEventListener('click', () => {

  fotoActual++

  if (fotoActual >= fotos.length) {
    fotoActual = 0
  }

  mostrarFoto()

})


// =========================
// FOTO ANTERIOR
// =========================

previousButton.addEventListener('click', () => {

  fotoActual--

  if (fotoActual < 0) {
    fotoActual = fotos.length - 1
  }

  mostrarFoto()

})
