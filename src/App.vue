<script setup>
import { computed, ref } from 'vue'
import metrics from './data/metrics.json'

const selectedPattern = ref(metrics.patterns[0])
const selectedRange = ref('12 weeks')
const showPatientInfo = ref(false)
const showHistory = ref(false)
const isDark = ref(false)
const notificationSent = ref(false)
const latestMonth = computed(() => metrics.monthly[metrics.monthly.length - 1])

function selectPattern(pattern) {
  selectedPattern.value = pattern
}

function progressTone(value) {
  if (value < 40) return 'progress-red'
  if (value < 70) return 'progress-yellow'
  return 'progress-green'
}

async function shareStatus() {
  const shareData = {
    title: `${metrics.patient.name}'s daily status`,
    text: `${metrics.patient.name}'s independence status is ${metrics.kpis.independenceScore}% today.`,
  }

  if (navigator.share) {
    await navigator.share(shareData)
  } else {
    await navigator.clipboard.writeText(`${shareData.title}: ${shareData.text}`)
  }
}

function notifyCareCircle() {
  notificationSent.value = true
}
</script>

<template>
  <main class="app-shell" :class="{ 'light-mode': !isDark }">
    <header class="topbar">
      <div class="brand-lockup">
        <div class="brand-mark">m</div>
        <div><p class="eyebrow">Care circle</p><h1>My dashboard</h1></div>
      </div>
      <div class="patient-context"><span class="status-dot"></span><div><strong>{{ metrics.patient.name }}</strong></div></div>
      <div class="top-actions"><button class="history-button" type="button" @click="showHistory = !showHistory"><span class="button-icon">↗</span> Patient history</button><button class="theme-toggle" type="button" :aria-label="isDark ? 'Switch to light mode' : 'Switch to dark mode'" :title="isDark ? 'Switch to light mode' : 'Switch to dark mode'" @click="isDark = !isDark"><span>{{ isDark ? '☀' : '☾' }}</span></button><button class="profile-button" type="button" aria-label="Open patient information" @click="showPatientInfo = !showPatientInfo">E</button></div>
    </header>

    <section v-if="showHistory || showPatientInfo" class="utility-panel"><div v-if="showHistory"><span class="panel-icon">↗</span><strong>Patient history</strong><span>12 months of observations are ready to review.</span></div><div v-if="showPatientInfo"><span class="panel-icon">i</span><strong>{{ metrics.patient.name }}</strong><span>Family contact: Mia Vale · (555) 014-2280</span></div></section>

    <div class="content">
      <section class="welcome-row"><div><p class="eyebrow blue-label">Dec 8, 2025 – Check-In</p><h2>Good morning, Rosa <span class="wave">✦</span></h2><p class="muted">Here is how Elena's morning is moving along.</p></div><button class="share-button" type="button" @click="shareStatus"><span aria-hidden="true">↗</span> Share</button></section>

      <section class="kpi-grid">
        <article class="card patient-card"><div class="card-heading"><span class="icon-circle blue">◎</span><span>Patient Name</span><span class="more">···</span></div><strong class="metric-large">{{ metrics.patient.name }}</strong><p class="muted">Age 76</p></article>
        <article class="card diagnosis-card"><div class="card-heading"><span class="icon-circle blue">✦</span><span>Diagnosis</span><span class="more">···</span></div><strong class="metric-large">{{ metrics.patient.diagnosis }}</strong><p class="muted">Diagnosed {{ metrics.patient.diagnosisDate }}</p></article>
        <article class="card observation-card"><div class="card-heading"><span class="icon-circle yellow">◌</span><span>Recent observations</span><span class="more">···</span></div><strong class="metric-large">{{ metrics.kpis.observations }} <small>this week</small></strong><button class="text-button" type="button" @click="selectPattern(metrics.patterns[0])">{{ metrics.kpis.observationsNeedingAction }} need your attention <span>→</span></button></article>
        <article class="card independence-card" :class="progressTone(metrics.kpis.independenceScore)"><div class="card-heading"><span class="icon-circle green">↗</span><span>Independence status</span><span class="more">···</span></div><div class="score-row"><strong class="metric-large">{{ metrics.kpis.independenceScore }}%</strong><span class="trend-up">↗ {{ metrics.kpis.independenceChange }}%</span></div><div class="mini-progress"><span :style="{ width: `${metrics.kpis.independenceScore}%` }"></span></div><p class="muted">Compared to last month</p></article>
      </section>

      <section class="section-heading"><div><p class="eyebrow blue-label">Pattern watch</p><h2>What we are noticing</h2></div><button class="round-action" type="button" aria-label="View all patterns">→</button></section>
      <section class="patterns-layout"><div class="pattern-list"><button v-for="pattern in metrics.patterns" :key="pattern.id" type="button" class="pattern-item" :class="{ active: selectedPattern.id === pattern.id }" @click="selectPattern(pattern)"><span class="pattern-status" :class="pattern.severity"></span><span class="pattern-copy"><strong>{{ pattern.title }}</strong><small>{{ pattern.description }}</small></span><span class="pattern-count">{{ pattern.count }}<small>logs</small></span><span class="arrow">→</span></button></div><article class="analysis-card"><div class="analysis-top"><span class="sparkle">✦</span><span>Smart analysis</span><span class="analysis-tag">{{ selectedPattern.severity }}</span></div><h3>{{ selectedPattern.title }}</h3><p>{{ selectedPattern.analysis }}</p><button class="outline-button" type="button">Add an observation <span>+</span></button></article></section>

      <section class="section-heading chart-heading"><div><p class="eyebrow blue-label">Weekly analysis</p><h2>Elena's independence over time</h2><p class="muted">A higher score means more daily activities completed with less support.</p></div><div class="segmented-control"><button v-for="range in ['12 weeks', '6 months', '12 months']" :key="range" type="button" :class="{ selected: selectedRange === range }" @click="selectedRange = range">{{ range }}</button></div></section>
      <article class="card chart-card"><div class="trend-insight"><span class="insight-icon">✦</span><div><strong>Elena is building steady momentum</strong><p>Her independence has risen 14 points across the last 12 weeks, with the strongest gains appearing in morning routines and orientation after breakfast.</p></div></div><div class="chart-wrap"><div class="y-axis"><span>80</span><span>70</span><span>60</span><span>50</span><span>40</span></div><svg viewBox="0 0 760 220" preserveAspectRatio="none" role="img" aria-label="Line chart showing independence rising from 58 to 72 percent"><defs><linearGradient id="chart-fill" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#4f83c9" stop-opacity=".18"/><stop offset="1" stop-color="#4f83c9" stop-opacity="0"/></linearGradient></defs><path d="M0 132 L69 114 L138 120 L207 92 L276 96 L345 78 L414 66 L483 72 L552 54 L621 60 L690 48 L760 42 L760 220 L0 220Z" fill="url(#chart-fill)" /><path d="M0 132 L69 114 L138 120 L207 92 L276 96 L345 78 L414 66 L483 72 L552 54 L621 60 L690 48 L760 42" fill="none" stroke="#3f71b5" stroke-width="3" vector-effect="non-scaling-stroke" /><circle cx="760" cy="42" r="5" fill="#ffffff" stroke="#3f71b5" stroke-width="3" vector-effect="non-scaling-stroke" /></svg><div class="x-axis"><span v-for="label in metrics.weeklyAnalysis.labels" :key="label">{{ label }}</span></div></div><div class="chart-legend"><span><i class="legend-line"></i> Independence score</span><button type="button" class="full-chart">Open full view ↗</button></div></article>

      <section class="lower-grid"><article class="card task-card"><div class="section-heading compact"><div><p class="eyebrow blue-label">Time analysis</p><h2>Daily activities</h2></div><button class="round-action" type="button" aria-label="View activity details">→</button></div><div v-for="task in metrics.taskAnalysis" :key="task.label" class="task-row"><div class="task-title"><strong>{{ task.label }}</strong><span>{{ task.duration }} <small>/ {{ task.baseline }}</small></span></div><div class="task-track"><span :style="{ width: `${task.progress}%` }"></span></div><p>{{ task.note }}</p></div><div class="meaning-note"><span class="meaning-icon">i</span><div><strong>What does this mean?</strong><p>Elena is managing familiar morning activities with growing independence. She may need more time and gentle reminders during medication and evening transitions.</p></div></div></article><article class="card today-card"><div class="section-heading compact"><div><p class="eyebrow blue-label">Today</p><h2>Daily log</h2></div><span class="day-status">4 of 5 done</span></div><div v-for="event in metrics.dailyEvents" :key="event.label" class="event-row"><span class="event-dot" :class="event.status"></span><div><strong>{{ event.label }}</strong><p>{{ event.detail }}</p></div><time>{{ event.time }}</time></div><div class="notify-request"><div><strong>Need help with something today?</strong><p>Send the medication reminder request to Elena's doctor and family.</p></div><button class="notify-button" type="button" @click="notifyCareCircle"><span aria-hidden="true">↗</span>{{ notificationSent ? 'Sent' : 'Notify care circle' }}</button></div></article></section>

      <section class="section-heading monthly-heading"><div><p class="eyebrow blue-label">A year in view</p><h2>Highs, lows, and next steps</h2><p class="muted">Use these moments to guide the next family conversation.</p></div></section>
      <article class="card monthly-card"><div class="month-bars"><div v-for="month in metrics.monthly" :key="month.month" class="month-bar" :class="progressTone(month.score)"><span class="bar-value">{{ month.score }}</span><div class="bar-track"><span :style="{ height: `${month.score}%` }"></span></div><small>{{ month.month }}</small></div></div><div class="monthly-notes"><div><span class="note-label positive">↑ A good pattern</span><strong>{{ latestMonth.high }}</strong><p>Share this win with the family and repeat the conditions that made it possible.</p></div><div><span class="note-label caution">↓ Worth noting</span><strong>{{ latestMonth.low }}</strong><p>Bring this observation to the next appointment so the team can spot a trend.</p></div></div></article>
      <footer><span>Last updated {{ metrics.patient.lastUpdated }}</span><span>Private care notes · Fictional demo data</span></footer>
    </div>
  </main>
</template>
