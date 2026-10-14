<script setup>
import { ref, nextTick } from 'vue'
const active = ref('start')
const menuOpen = ref(false)
const selected = ref(1)
const dialog = ref(null)
const detail = ref(1)
const items = [{ id: 'start', label: 'Start' }, { id: 'o-mnie', label: 'O mnie' }, { id: 'umiejetnosci', label: 'Umiejętności' }, { id: 'projekty', label: 'Portfolio' }, { id: 'kontakt', label: 'Kontakt' }]
const projects = [1, 2, 3]
function navigate(id) { active.value = id; menuOpen.value = false; nextTick(() => document.getElementById(id)?.scrollIntoView({ behavior: window.matchMedia('(prefers-reduced-motion: reduce)').matches ? 'instant' : 'smooth', block: 'start' })) }
function move(direction) { selected.value = (selected.value + direction + projects.length) % projects.length }
function showProject(index) { detail.value = index + 1; dialog.value.showModal() }
</script>

<template>
  <div class="portfolio-shell" @keydown.esc="menuOpen = false">
    <header class="topbar"><a href="#start" class="brand" @click.prevent="navigate('start')">Grażyna Długosz<span>Portfolio</span></a><div class="top-actions"><button class="icon-button" aria-label="Przejdź do portfolio" @click="navigate('projekty')">&lt;/&gt;</button><button class="icon-button" aria-label="Przejdź do informacji o mnie" @click="navigate('o-mnie')">GD</button><button class="icon-button" aria-label="Przejdź do kontaktu" @click="navigate('kontakt')"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M2 5h20v14H2z M2 5l10 8L22 5" /></svg></button></div><button class="menu-toggle" :aria-expanded="menuOpen" aria-controls="side-menu" @click="menuOpen = !menuOpen">{{ menuOpen ? 'Zamknij ✕' : 'Menu ☰' }}</button></header>
    <div class="workspace"><aside id="side-menu" class="sidebar" :class="{ open: menuOpen }"><nav aria-label="Menu główne"><a v-for="item in items" :key="item.id" :href="`#${item.id}`" :class="{ active: active === item.id }" :aria-current="active === item.id ? 'location' : undefined" @click.prevent="navigate(item.id)">{{ item.label }}</a></nav><div class="tech-badges" aria-label="Technologie projektu"><span class="vue-mark">V</span><span class="html-mark">HTML</span><span class="css-mark">CSS</span><span class="js-mark">JS</span></div></aside>
    <main>
      <section id="start" class="intro"><h1><span>&lt;code /&gt;</span> Tworzę. Rozwijam. Odkrywam.</h1><p class="identity">Grażyna Długosz <span>/ Portfolio</span></p></section>
      <section v-if="active === 'o-mnie'" id="o-mnie" class="info-panel"><h2>O mnie</h2><p>Grażyna Długosz</p><p>Opis: <span class="muted">Do uzupełnienia</span></p></section>
      <section v-if="active === 'umiejetnosci'" id="umiejetnosci" class="info-panel"><h2>Umiejętności</h2><p>Do uzupełnienia</p><p class="muted">Technologie tej strony: Vue 3, JavaScript, HTML i CSS.</p></section>
      <div class="stats"><section class="stat"><h2>Technologie strony</h2><p>Vue 3 <i>|</i> HTML <i>|</i> CSS <i>|</i> JavaScript</p></section><section class="stat neon"><h2>Ukończone projekty</h2><p class="pending">Do uzupełnienia</p></section><section class="stat"><h2>Doświadczenie</h2><p>Do uzupełnienia</p></section></div>
      <section id="projekty" class="projects"><h2 class="section-title">Moje portfolio</h2><div class="project-grid"><article v-for="(project, index) in projects" :key="project" class="project-card" :class="{ selected: selected === index }"><div class="card-wave" aria-hidden="true"></div><h3>Projekt {{ project }}</h3><p>Do uzupełnienia</p><button class="project-button" @click="showProject(index)">Szczegóły projektu</button><div class="code-preview" aria-hidden="true"><i></i><i></i><i></i><i></i></div></article></div><div class="carousel-controls" aria-label="Wybór projektu"><button aria-label="Poprzedni projekt" @click="move(-1)">‹</button><div><button v-for="(project, index) in projects" :key="project" class="dot" :class="{ chosen: selected === index }" :aria-label="`Wybierz projekt ${project}`" :aria-pressed="selected === index" @click="selected = index"></button></div><button aria-label="Następny projekt" @click="move(1)">›</button></div></section>
    </main></div>
    <footer id="kontakt"><section><h2>Dane kontaktowe</h2><p>E-mail: <span>Do uzupełnienia</span></p><p>Lokalizacja: <span>Do uzupełnienia</span></p></section><section><h2>Pozostańmy w kontakcie</h2><p>LinkedIn: <span>Do uzupełnienia</span></p><p>GitHub: <span>Do uzupełnienia</span></p></section></footer>
    <dialog ref="dialog" class="project-dialog" @click="event => { if (event.target === dialog) dialog.close() }"><h2>Projekt {{ detail }}</h2><dl><dt>Nazwa i opis</dt><dd>Do uzupełnienia</dd><dt>Technologie</dt><dd>Do uzupełnienia</dd><dt>Adres projektu</dt><dd>Do uzupełnienia</dd></dl><form method="dialog"><button class="project-button">Zamknij</button></form></dialog>
  </div>
</template>
