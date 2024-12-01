# Projeto-Integrador-Sistema-de-Alarme
Sistema de alarme de incêndio com notificações via Bluetooth para deficientes visuais e auditivos
<html><head><base href="https://www.exemplo.com">
<meta charset="UTF-8">
<title>Sistema de Alarme Acessível</title>
<style>
:root {
  --primary: #2196F3;
  --danger: #2196F3;
  --success: #4CAF50;
  --warning: #4CAF50;
  --emergency: #ff0000;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  margin: 0;
  padding: 20px;
  background: #f5f5f5;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.app-container {
  max-width: 600px;
  width: 100%;
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.status-panel {
  display: grid;
  grid-template-columns: 1fr;
  gap: 15px;
  margin: 20px 0;
}

.status-card {
  padding: 15px;
  border-radius: 8px;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  min-height: 120px;
  position: relative;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.status-card:hover {
  transform: translateY(-2px);
}

.status-card.fire {
  background: var(--danger);
}

.vibration-pattern {
  margin-top: 20px;
  padding: 20px;
  border: 2px solid var(--primary);
  border-radius: 8px;
}

.alert {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  padding: 15px 30px;
  border-radius: 8px;
  color: white;
  background: var(--danger);
  display: none;
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from {
    transform: translate(-50%, -100%);
  }
  to {
    transform: translate(-50%, 0);
  }
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

@keyframes iconShake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

@keyframes iconGlow {
  0%, 100% { filter: brightness(1); }
  50% { filter: brightness(1.5) drop-shadow(0 0 10px rgba(255,255,255,0.8)); }
}

.status-card svg {
  transition: all 0.3s ease;
}

.status-card.emergency-active svg {
  animation: iconShake 0.5s infinite, iconGlow 1s infinite;
}

.status-card.fire.emergency-active {
  background: var(--emergency);
}

.haptic-test {
  background: var(--primary);
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 25px;
  font-size: 1.1em;
  cursor: pointer;
  margin-top: 20px;
  transition: all 0.3s ease;
}

.haptic-test:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(33,150,243,0.3);
}
</style>
</head>
<body>
<div class="app-container">
  <h1>Sistema de Alarme Acessível</h1>
  
  <div class="status-panel">
    <div class="status-card fire" id="fireAlert">
      <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M12 22c5.523 0 10-4.477 10-10 0-4.478-3.98-9.478-10-12-6.02 2.522-10 7.522-10 12 0 5.523 4.477 10 10 10z"/>
        <path d="M12 18c2.761 0 5-2.239 5-5 0-2.239-1.99-4.739-5-6-3.01 1.261-5 3.761-5 6 0 2.761 2.239 5 5 5z"/>
      </svg>
      <h3>Alerta de Incêndio</h3>
    </div>
  </div>

  <div class="vibration-pattern">
    <h3>Padrões de Vibração:</h3>
    <p>Incêndio: 3 vibrações curtas, pausa, repete</p>
  </div>

  <button class="haptic-test" id="testVibration">Testar Vibração</button>
</div>

<div class="alert" id="emergencyAlert">
  🚨 EMERGÊNCIA DETECTADA!
</div>

<script>
const fireAlert = document.getElementById('fireAlert');
const testButton = document.getElementById('testVibration');
const emergencyAlert = document.getElementById('emergencyAlert');

function vibrate(pattern) {
  if ('vibrate' in navigator) {
    navigator.vibrate(pattern);
  }
}

function simulateFireAlert() {
  const pattern = [200, 100, 200, 100, 200, 500]; // 3 vibrações curtas
  vibrate(pattern);
  fireAlert.classList.add('emergency-active');
  emergencyAlert.style.display = 'block';
  emergencyAlert.textContent = '🚨 ALERTA DE INCÊNDIO DETECTADO!';
  
  setTimeout(() => {
    if (!fireAlert.classList.contains('emergency-active')) return;
    fireAlert.classList.remove('emergency-active');
  }, 5000);
}

fireAlert.addEventListener('click', simulateFireAlert);

testButton.addEventListener('click', () => {
  const testPattern = [200, 100, 200];
  vibrate(testPattern);
});

function startMonitoring() {
  setInterval(() => {
    const fireSensor = Math.random() * 100;
    
    if (fireSensor > 90) {
      simulateFireAlert();
    }
  }, 5000);
}

startMonitoring();

function resetAlerts() {
  fireAlert.classList.remove('emergency-active');
  emergencyAlert.style.display = 'none';
}

document.addEventListener('keypress', (e) => {
  if (e.key === 'r' || e.key === 'R') {
    resetAlerts();
  }
});
</script>
</body></html>
