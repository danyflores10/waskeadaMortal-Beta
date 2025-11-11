<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import trailerVideo from './assets/video/trailer.mp4'

// Estado de la navegación
const isScrolled = ref(false)
const isMobileMenuOpen = ref(false)
const isVideoPlaying = ref(false)
const videoElement = ref(null)

// Manejar el scroll para el navbar
const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

// Toggle mobile menu
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

// Scroll suave a secciones
const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth', block: 'start' })
    isMobileMenuOpen.value = false
  }
}

// Controlar video
const playVideo = () => {
  if (videoElement.value) {
    videoElement.value.play()
    isVideoPlaying.value = true
  }
}

const pauseVideo = () => {
  if (videoElement.value) {
    videoElement.value.pause()
    isVideoPlaying.value = false
  }
}

const toggleVideo = () => {
  if (isVideoPlaying.value) {
    pauseVideo()
  } else {
    playVideo()
  }
}
</script>

<template>
  <div class="rockstar-container">
    <!-- Navegación Superior Estilo Rockstar -->
    <nav class="navbar" :class="{ 'navbar-scrolled': isScrolled }">
      <div class="navbar-content">
        <div class="navbar-logo">
          <span class="logo-main">WUASKEADA</span>
          <span class="logo-sub">MORTAL</span>
        </div>
        
        <div class="navbar-links" :class="{ 'mobile-open': isMobileMenuOpen }">
          <a @click="scrollToSection('home')" class="nav-link">INICIO</a>
          <a @click="scrollToSection('trailer')" class="nav-link">TRÁILER</a>
          <a @click="scrollToSection('gameplay')" class="nav-link">GAMEPLAY</a>
          <a @click="scrollToSection('screenshots')" class="nav-link">CAPTURAS</a>
          <a @click="scrollToSection('news')" class="nav-link">NOTICIAS</a>
        </div>

        <div class="navbar-actions">
          <button class="btn-primary">COMPRAR AHORA</button>
        </div>

        <!-- Mobile Menu Button -->
        <button class="mobile-menu-btn" @click="toggleMobileMenu">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>
    </nav>

    <!-- Hero Section Fullscreen -->
    <section id="home" class="hero-section">
      <div class="hero-background">
        <!-- Aquí iría un video o imagen de fondo -->
        <div class="hero-overlay"></div>
      </div>
      
      <div class="hero-content">
        <div class="hero-logo-container">
          <h1 class="hero-title">
            <span class="title-main">WUASKEADA</span>
            <span class="title-sub">MORTAL</span>
          </h1>
          <p class="hero-tagline">LA BATALLA MÁS BRUTAL DE LA HISTORIA</p>
        </div>
        
        <div class="hero-actions">
          <button class="btn-hero-primary">
            <span>COMPRAR AHORA</span>
          </button>
          <button class="btn-hero-secondary" @click="scrollToSection('trailer')">
            <span class="play-icon">▶</span>
            <span>VER TRÁILER</span>
          </button>
        </div>

        <div class="hero-info">
          <div class="info-item">
            <span class="info-label">Disponible en</span>
            <span class="info-value">PC • PS5 • XBOX SERIES X|S</span>
          </div>
        </div>
      </div>

      <div class="scroll-indicator">
        <div class="scroll-arrow">↓</div>
      </div>
    </section>

    <!-- Sección de Tráiler -->
    <section id="trailer" class="trailer-section">
      <div class="section-container">
        <div class="trailer-header">
          <h2 class="section-title">TRÁILER OFICIAL</h2>
          <p class="section-subtitle">Experimenta la intensidad del combate</p>
        </div>

        <div class="trailer-video-container">
          <div class="video-wrapper">
            <video 
              ref="videoElement"
              :src="trailerVideo"
              class="trailer-video"
              @click="toggleVideo"
              controls
              preload="metadata"
            ></video>
            <div 
              v-if="!isVideoPlaying" 
              class="video-overlay" 
              @click="playVideo"
            >
              <div class="play-button">
                <svg width="80" height="80" viewBox="0 0 80 80">
                  <circle cx="40" cy="40" r="38" stroke="white" stroke-width="2" fill="rgba(255,255,255,0.1)"/>
                  <polygon points="32,25 32,55 55,40" fill="white"/>
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Sección Gameplay -->
    <section id="gameplay" class="gameplay-section">
      <div class="section-container">
        <div class="gameplay-grid">
          <div class="gameplay-content">
            <span class="section-label">CARACTERÍSTICAS</span>
            <h2 class="section-title-large">COMBATE BRUTAL</h2>
            <p class="section-description">
              Sistema de combate revolucionario con mecánicas fluidas y combos devastadores. 
              Cada golpe cuenta, cada movimiento puede cambiar el curso de la batalla.
            </p>
            <ul class="feature-list">
              <li>Más de 50 movimientos únicos por personaje</li>
              <li>Sistema de combos en tiempo real</li>
              <li>Físicas realistas y daño destructible</li>
              <li>Modo historia cinemático de 20+ horas</li>
            </ul>
            <button class="btn-secondary">MÁS INFORMACIÓN</button>
          </div>
          
          <div class="gameplay-image">
            <div class="image-placeholder">
              <span class="placeholder-text">GAMEPLAY</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Sección de Capturas -->
    <section id="screenshots" class="screenshots-section">
      <div class="section-container">
        <div class="screenshots-header">
          <h2 class="section-title">CAPTURAS DE PANTALLA</h2>
          <p class="section-subtitle">Cada frame es una obra de arte</p>
        </div>

        <div class="screenshots-grid">
          <div class="screenshot-item" v-for="i in 6" :key="i">
            <div class="screenshot-placeholder">
              <span class="screenshot-number">{{ String(i).padStart(2, '0') }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Sección de Noticias -->
    <section id="news" class="news-section">
      <div class="section-container">
        <div class="news-header">
          <h2 class="section-title">ÚLTIMAS NOTICIAS</h2>
          <a href="#" class="view-all-link">VER TODAS →</a>
        </div>

        <div class="news-grid">
          <article class="news-card news-card-featured">
            <div class="news-image">
              <div class="news-image-placeholder">NOVEDAD</div>
              <span class="news-badge">DESTACADO</span>
            </div>
            <div class="news-content">
              <span class="news-date">15 NOVIEMBRE 2025</span>
              <h3 class="news-title">Nuevo modo Battle Royale disponible</h3>
              <p class="news-excerpt">
                Sumérgete en la acción con hasta 100 jugadores en el nuevo modo de supervivencia.
              </p>
              <a href="#" class="news-link">LEER MÁS →</a>
            </div>
          </article>

          <article class="news-card">
            <div class="news-image">
              <div class="news-image-placeholder">ACTUALIZACIÓN</div>
            </div>
            <div class="news-content">
              <span class="news-date">10 NOVIEMBRE 2025</span>
              <h3 class="news-title">Patch 2.5 - Mejoras de balance</h3>
              <p class="news-excerpt">
                Ajustes importantes en personajes y corrección de bugs.
              </p>
              <a href="#" class="news-link">LEER MÁS →</a>
            </div>
          </article>

          <article class="news-card">
            <div class="news-image">
              <div class="news-image-placeholder">EVENTO</div>
            </div>
            <div class="news-content">
              <span class="news-date">05 NOVIEMBRE 2025</span>
              <h3 class="news-title">Torneo mundial anunciado</h3>
              <p class="news-excerpt">
                Competición global con premios de más de $1M en efectivo.
              </p>
              <a href="#" class="news-link">LEER MÁS →</a>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="footer-container">
        <div class="footer-content">
          <div class="footer-brand">
            <div class="footer-logo">
              <span class="logo-main">WUASKEADA</span>
              <span class="logo-sub">MORTAL</span>
            </div>
            <p class="footer-tagline">El juego más brutal jamás creado</p>
          </div>

          <div class="footer-links-container">
            <div class="footer-links-group">
              <h4 class="footer-links-title">JUEGO</h4>
              <a href="#" class="footer-link">Comprar</a>
              <a href="#" class="footer-link">Características</a>
              <a href="#" class="footer-link">Requisitos del sistema</a>
              <a href="#" class="footer-link">Soporte</a>
            </div>

            <div class="footer-links-group">
              <h4 class="footer-links-title">COMUNIDAD</h4>
              <a href="#" class="footer-link">Discord</a>
              <a href="#" class="footer-link">Twitter</a>
              <a href="#" class="footer-link">Twitch</a>
              <a href="#" class="footer-link">YouTube</a>
            </div>

            <div class="footer-links-group">
              <h4 class="footer-links-title">EMPRESA</h4>
              <a href="#" class="footer-link">Acerca de</a>
              <a href="#" class="footer-link">Empleos</a>
              <a href="#" class="footer-link">Prensa</a>
              <a href="#" class="footer-link">Contacto</a>
            </div>
          </div>
        </div>

        <div class="footer-bottom">
          <div class="footer-legal">
            <p>&copy; 2025 Wuaskeada Mortal. Todos los derechos reservados.</p>
            <div class="footer-legal-links">
              <a href="#">Términos de servicio</a>
              <a href="#">Política de privacidad</a>
              <a href="#">Cookies</a>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.rockstar-container {
  width: 100%;
  background: #000;
  color: #fff;
  overflow-x: hidden;
}

/* Navbar Estilo Rockstar */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  transition: all 0.4s ease;
  background: linear-gradient(180deg, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0) 100%);
  padding: 1.5rem 0;
}

.navbar-scrolled {
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(10px);
  padding: 1rem 0;
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.5);
}

.navbar-content {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 3rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navbar-logo {
  display: flex;
  flex-direction: column;
  line-height: 1;
}

.logo-main {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 2rem;
  letter-spacing: 2px;
  color: #fff;
}

.logo-sub {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 0.8rem;
  letter-spacing: 6px;
  color: #999;
}

.navbar-links {
  display: flex;
  gap: 2.5rem;
  align-items: center;
}

.nav-link {
  font-family: 'Montserrat', sans-serif;
  font-weight: 600;
  font-size: 0.85rem;
  letter-spacing: 1px;
  color: #fff;
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  padding: 0.5rem 0;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: #fff;
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: #ccc;
}

.nav-link:hover::after {
  width: 100%;
}

.navbar-actions {
  display: flex;
  gap: 1rem;
}

.btn-primary {
  background: #fff;
  color: #000;
  border: none;
  padding: 0.8rem 2rem;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 0.85rem;
  letter-spacing: 1px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary:hover {
  background: #f0f0f0;
  transform: scale(1.05);
}

.mobile-menu-btn {
  display: none;
  flex-direction: column;
  gap: 4px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
}

.mobile-menu-btn span {
  width: 25px;
  height: 2px;
  background: #fff;
  transition: all 0.3s ease;
}

/* Hero Section */
.hero-section {
  position: relative;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.hero-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1a1a1a 0%, #000 100%);
  background-image: 
    radial-gradient(circle at 20% 50%, rgba(255, 50, 50, 0.1) 0%, transparent 50%),
    radial-gradient(circle at 80% 80%, rgba(255, 150, 50, 0.1) 0%, transparent 50%);
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
}

.hero-content {
  position: relative;
  z-index: 10;
  text-align: center;
  max-width: 1200px;
  padding: 2rem;
}

.hero-logo-container {
  margin-bottom: 3rem;
}

.hero-title {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1.5rem;
}

.title-main {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(4rem, 12vw, 10rem);
  letter-spacing: 10px;
  line-height: 0.9;
  color: #fff;
  text-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
}

.title-sub {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(2rem, 6vw, 5rem);
  letter-spacing: 15px;
  color: #999;
  margin-top: -0.5rem;
}

.hero-tagline {
  font-family: 'Montserrat', sans-serif;
  font-size: clamp(0.9rem, 2vw, 1.2rem);
  font-weight: 400;
  letter-spacing: 4px;
  color: #ccc;
  text-transform: uppercase;
}

.hero-actions {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  margin-bottom: 4rem;
  flex-wrap: wrap;
}

.btn-hero-primary,
.btn-hero-secondary {
  padding: 1.2rem 3rem;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 0.95rem;
  letter-spacing: 2px;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.8rem;
}

.btn-hero-primary {
  background: #fff;
  color: #000;
}

.btn-hero-primary:hover {
  background: #f0f0f0;
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(255, 255, 255, 0.3);
}

.btn-hero-secondary {
  background: transparent;
  color: #fff;
  border: 2px solid #fff;
}

.btn-hero-secondary:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-3px);
}

.play-icon {
  font-size: 0.8rem;
}

.hero-info {
  display: flex;
  justify-content: center;
  gap: 3rem;
}

.info-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.info-label {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 2px;
  color: #999;
  text-transform: uppercase;
}

.info-value {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 1px;
  color: #fff;
}

.scroll-indicator {
  position: absolute;
  bottom: 3rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
}

.scroll-arrow {
  font-size: 2rem;
  color: #fff;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}

/* Secciones */
.section-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 8rem 3rem;
}

.section-title {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3rem, 6vw, 5rem);
  letter-spacing: 5px;
  margin-bottom: 1rem;
  color: #fff;
}

.section-subtitle {
  font-family: 'Montserrat', sans-serif;
  font-size: 1.1rem;
  font-weight: 400;
  color: #999;
  letter-spacing: 1px;
}

/* Trailer Section */
.trailer-section {
  background: #0a0a0a;
}

.trailer-header {
  text-align: center;
  margin-bottom: 4rem;
}

.trailer-video-container {
  max-width: 1400px;
  margin: 0 auto;
}

.video-wrapper {
  position: relative;
  padding-bottom: 56.25%; /* 16:9 */
  overflow: hidden;
  background: #000;
  border-radius: 0;
}

.trailer-video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  cursor: pointer;
}

.video-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
}

.video-overlay:hover {
  background: rgba(0, 0, 0, 0.2);
}

.video-overlay:hover .play-button {
  transform: scale(1.1);
}

.play-button {
  position: relative;
  z-index: 10;
  transition: transform 0.3s ease;
  cursor: pointer;
}

/* Gameplay Section */
.gameplay-section {
  background: #000;
}

.gameplay-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5rem;
  align-items: center;
}

.section-label {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 700;
  letter-spacing: 3px;
  color: #999;
  text-transform: uppercase;
  display: block;
  margin-bottom: 1rem;
}

.section-title-large {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3rem, 6vw, 6rem);
  letter-spacing: 5px;
  margin-bottom: 2rem;
  line-height: 1;
}

.section-description {
  font-family: 'Montserrat', sans-serif;
  font-size: 1.1rem;
  line-height: 1.8;
  color: #ccc;
  margin-bottom: 2rem;
}

.feature-list {
  list-style: none;
  margin-bottom: 3rem;
}

.feature-list li {
  font-family: 'Montserrat', sans-serif;
  font-size: 1rem;
  color: #fff;
  padding: 1rem 0;
  padding-left: 2rem;
  position: relative;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.feature-list li::before {
  content: '→';
  position: absolute;
  left: 0;
  color: #fff;
  font-weight: bold;
}

.btn-secondary {
  background: transparent;
  color: #fff;
  border: 2px solid #fff;
  padding: 1rem 2.5rem;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 0.85rem;
  letter-spacing: 2px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-secondary:hover {
  background: #fff;
  color: #000;
}

.gameplay-image {
  width: 100%;
  height: 600px;
}

.image-placeholder {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1a1a1a 0%, #0a0a0a 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.placeholder-text {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 4rem;
  letter-spacing: 10px;
  color: rgba(255, 255, 255, 0.05);
}

/* Screenshots Section */
.screenshots-section {
  background: #0a0a0a;
}

.screenshots-header {
  text-align: center;
  margin-bottom: 4rem;
}

.screenshots-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
  gap: 2rem;
}

.screenshot-item {
  aspect-ratio: 16/9;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.screenshot-item:hover {
  transform: scale(1.02);
}

.screenshot-placeholder {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1a1a1a 0%, #0a0a0a 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.screenshot-number {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 5rem;
  letter-spacing: 5px;
  color: rgba(255, 255, 255, 0.05);
}

/* News Section */
.news-section {
  background: #000;
}

.news-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4rem;
}

.view-all-link {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  letter-spacing: 1px;
  color: #fff;
  text-decoration: none;
  transition: all 0.3s ease;
}

.view-all-link:hover {
  color: #ccc;
}

.news-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2rem;
}

.news-card {
  background: #0a0a0a;
  overflow: hidden;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.news-card:hover {
  transform: translateY(-5px);
  border-color: rgba(255, 255, 255, 0.3);
}

.news-card-featured {
  grid-column: span 2;
}

.news-image {
  position: relative;
  aspect-ratio: 16/9;
  overflow: hidden;
}

.news-image-placeholder {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1a1a1a 0%, #0a0a0a 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Bebas Neue', sans-serif;
  font-size: 3rem;
  letter-spacing: 5px;
  color: rgba(255, 255, 255, 0.05);
}

.news-badge {
  position: absolute;
  top: 1rem;
  left: 1rem;
  background: #fff;
  color: #000;
  padding: 0.5rem 1rem;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 1px;
}

.news-content {
  padding: 2rem;
}

.news-date {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 2px;
  color: #999;
  text-transform: uppercase;
  display: block;
  margin-bottom: 1rem;
}

.news-title {
  font-family: 'Montserrat', sans-serif;
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1.3;
  margin-bottom: 1rem;
  color: #fff;
}

.news-excerpt {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.95rem;
  line-height: 1.6;
  color: #ccc;
  margin-bottom: 1.5rem;
}

.news-link {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: #fff;
  text-decoration: none;
  transition: all 0.3s ease;
}

.news-link:hover {
  color: #ccc;
}

/* Footer */
.footer {
  background: #000;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 5rem 3rem 3rem;
}

.footer-content {
  display: grid;
  grid-template-columns: 1.5fr 2fr;
  gap: 5rem;
  margin-bottom: 4rem;
}

.footer-brand {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.footer-logo {
  display: flex;
  flex-direction: column;
  line-height: 1;
}

.footer-tagline {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.95rem;
  color: #666;
}

.footer-links-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 3rem;
}

.footer-links-group {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.footer-links-title {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 700;
  letter-spacing: 2px;
  color: #fff;
  margin-bottom: 0.5rem;
}

.footer-link {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  color: #999;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer-link:hover {
  color: #fff;
}

.footer-bottom {
  padding-top: 2rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-legal {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  color: #666;
}

.footer-legal-links {
  display: flex;
  gap: 2rem;
}

.footer-legal-links a {
  color: #666;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer-legal-links a:hover {
  color: #999;
}

/* Responsive */
@media (max-width: 1200px) {
  .gameplay-grid {
    grid-template-columns: 1fr;
    gap: 3rem;
  }

  .news-card-featured {
    grid-column: span 1;
  }

  .footer-content {
    grid-template-columns: 1fr;
    gap: 3rem;
  }
}

@media (max-width: 768px) {
  .navbar-links {
    position: fixed;
    top: 0;
    right: -100%;
    width: 70%;
    height: 100vh;
    background: rgba(0, 0, 0, 0.98);
    flex-direction: column;
    justify-content: center;
    align-items: center;
    transition: right 0.3s ease;
    gap: 2rem;
  }

  .navbar-links.mobile-open {
    right: 0;
  }

  .navbar-actions {
    display: none;
  }

  .mobile-menu-btn {
    display: flex;
  }

  .screenshots-grid {
    grid-template-columns: 1fr;
  }

  .news-grid {
    grid-template-columns: 1fr;
  }

  .footer-links-container {
    grid-template-columns: 1fr;
    gap: 2rem;
  }

  .footer-legal {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .footer-legal-links {
    flex-direction: column;
    gap: 1rem;
  }
}
</style>
