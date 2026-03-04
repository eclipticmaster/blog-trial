---
layout: default
title: Dashboard
permalink: /dashboard/
---

## PhD Dashboard

### Current Stress Level

<div id="stress-container">
  <div id="stress-bar"></div>
</div>

<p id="stress-label"></p>

---

### Stress Over Time

<canvas id="stressChart"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  const current = {{ site.data.current_stress | jsonify }};
  const history = {{ site.data.stress_history | jsonify }};

  const currentStress =
      0.5 * current.emails +
      3 * current.tasks_due_3_days +
      2 * current.meetings_this_week;

  const normalized = Math.min(currentStress, 100);

  const bar = document.getElementById("stress-bar");
  const label = document.getElementById("stress-label");

  bar.style.width = normalized + "%";

  if (normalized < 30) {
    bar.style.backgroundColor = "green";
    label.textContent = "Low stress";
  } else if (normalized < 60) {
    bar.style.backgroundColor = "goldenrod";
    label.textContent = "Moderate stress";
  } else {
    bar.style.backgroundColor = "darkred";
    label.textContent = "High stress";
  }

  const ctx = document.getElementById('stressChart').getContext('2d');

  new Chart(ctx, {
    type: 'line',
    data: {
      labels: history.map(h => h.week),
      datasets: [{
        label: 'Weekly Stress',
        data: history.map(h => h.stress),
        fill: false,
        tension: 0.2
      }]
    }
  });
</script>