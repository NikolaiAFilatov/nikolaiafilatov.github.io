<html lang="nl" class="dark">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>EthicalScan — Prototype Workshop 3: Ethisch Experimenteren (PoC)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
      tailwind.config = {
        darkMode: "class",
        theme: {
          extend: {
            colors: {
              primary: "#0ea5e9",
              warn: "#f59e0b",
              danger: "#ef4444",
              success: "#10b981"
            }
          }
        }
      };
    </script>
    <style>
      /* Kleine extra's voor prototype feel */
      .cam-placeholder {
        background: linear-gradient(135deg,#0f172a 0%, #0b1220 100%);
        border: 1px dashed rgba(255,255,255,0.04);
      }
    </style>
  </head>

  <body class="bg-gray-900 text-gray-100 font-sans antialiased">
    <!-- Topbar -->
    <header class="bg-gray-800 border-b border-gray-700 px-6 py-4">
      <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-lg bg-primary flex items-center justify-center font-bold text-lg">ES</div>
          <div>
            <h1 class="text-xl font-semibold">EthicalScan — PoC</h1>
            <p class="text-xs text-gray-400">Workshop 3: Ethisch experimenteren · Ontwerp sprint & stakeholder mockup</p>
          </div>
        </div>

        <div class="flex items-center gap-4">
          <div class="text-sm text-gray-400">11/12/2025 — Demo</div>
          <div id="systemStatus" class="flex items-center gap-2 px-3 py-1 rounded-full text-sm bg-green-500/20 text-green-300">
            <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
            Systeem: Live
          </div>
        </div>
      </div>
    </header>

    <main class="max-w-7xl mx-auto px-6 py-6 space-y-6">
      <!-- Intro + role switch -->
      <section class="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <div>
          <h2 class="text-2xl font-bold">Design Sprint: PoC — Ethiek first</h2>
          <p class="text-sm text-gray-400 mt-1">Prototype om stakeholders te laten zien hoe we incidenten detecteren, beoordelen en ethisch mitigeren. Inclusief: requirements mockup, mitigaties, en SCAN-readiness quickcheck.</p>
        </div>

        <div class="flex gap-2">
          <button onclick="switchPerspective('operator')" id="btnOp" class="px-4 py-2 rounded-lg bg-primary text-black font-medium">🔒 Operator</button>
          <button onclick="switchPerspective('stakeholder')" id="btnSt" class="px-4 py-2 rounded-lg bg-gray-700 text-gray-200">👥 Stakeholder</button>
        </div>
      </section>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <!-- Main panel: camera & incident -->
        <section class="lg:col-span-2 space-y-4">
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <div class="flex items-start justify-between gap-4">
              <div>
                <h3 class="text-lg font-semibold">Actief Incident — Camera A12</h3>
                <p class="text-xs text-gray-400">Incident #2025-11-12-001 · Prioriteit: <span id="prioTag" class="font-semibold text-yellow-300">Hoog</span></p>
              </div>

              <div class="flex items-center gap-2">
                <button onclick="escalate()" class="px-3 py-1 rounded-lg bg-danger text-black text-sm font-semibold">Escaleer</button>
                <button onclick="verifyManually()" class="px-3 py-1 rounded-lg bg-gray-700 text-gray-200 text-sm">Start menselijke verificatie</button>
              </div>
            </div>

            <div class="mt-4 grid grid-cols-1 md:grid-cols-3 gap-4">
              <div class="md:col-span-2 bg-gray-900 rounded-lg p-3 cam-placeholder relative aspect-video">
                <!-- simulated camera feed -->
                <div id="camOverlay" class="absolute inset-0 flex items-center justify-center pointer-events-none">
                  <div class="text-center">
                    <div class="mx-auto w-28 h-28 border-4 rounded-lg border-yellow-500 flex items-center justify-center text-5xl">👤</div>
                    <div class="mt-2 text-sm text-yellow-300">LIVE · Camera A12 — Voordeur</div>
                    <div class="mt-1 text-xs text-gray-400">Bivakmuts gedetecteerd · lage zekerheid</div>
                  </div>
                </div>
              </div>

              <div class="bg-gray-900 rounded-lg p-3 border border-gray-700">
                <h4 class="font-semibold">Kerninformatie</h4>
                <div class="mt-3 space-y-2 text-sm text-gray-300">
                  <div class="flex justify-between"><span>Locatie</span><span>Voorzijde - Dock</span></div>
                  <div class="flex justify-between"><span>Tijd</span><span id="timeNow">22:16:08</span></div>
                  <div class="flex justify-between"><span>AI Betrouwbaarheid</span><span id="conf" class="font-semibold text-yellow-300">68%</span></div>
                  <div class="flex justify-between"><span>Actie</span><span id="actionTag" class="text-sm text-gray-300">Wacht op validatie</span></div>
                </div>

                <div class="mt-3 pt-3 border-t border-gray-700">
                  <h5 class="text-sm font-medium">Ethische Flags</h5>
                  <ul class="mt-2 text-xs text-gray-400 space-y-1">
                    <li>⚠️ Gezicht deels bedekt</li>
                    <li>ℹ️ Mogelijke privacy-scope: Publieke ruimte</li>
                    <li>🔒 Data bewaartermijn: 7 dagen</li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="mt-4 bg-gray-800/40 rounded-lg p-3 border border-gray-700 text-sm">
              <strong>AI Analyse:</strong> Model classificeert gedrag als "verdacht" maar met lage zekerheid. Aanbeveling: menselijke verificatie & minimaliseer data toegang. Gebruik de knoppen rechts voor vervolgacties.
            </div>

            <div class="mt-4 flex gap-3 text-sm">
              <button onclick="tagFalsePositive()" class="px-3 py-2 rounded-lg bg-gray-700">Als vals positief markeren</button>
              <button onclick="requestLiveness()" class="px-3 py-2 rounded-lg bg-primary text-black">Start liveness check</button>
              <button onclick="addAudit('Operator twijfel')" class="px-3 py-2 rounded-lg bg-gray-700">Voeg audit entry toe</button>
            </div>
          </div>

          <!-- Ethics & Requirements mockup -->
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <div class="flex items-center justify-between">
              <h3 class="text-lg font-semibold">Requirements mockup — Ethisch ontwerp</h3>
              <div class="text-xs text-gray-400">Sprint: Design → Prototype → Stakeholder review</div>
            </div>

            <div class="mt-4 grid grid-cols-1 md:grid-cols-3 gap-4 text-sm">
              <div class="bg-gray-900/50 rounded-lg p-3">
                <h4 class="font-medium text-white">Functional</h4>
                <ul class="mt-2 text-gray-300 space-y-1">
                  <li>• Realtime detectie & prioritering</li>
                  <li>• Handmatige validatie verplicht</li>
                  <li>• Audit log & export</li>
                </ul>
              </div>

              <div class="bg-gray-900/50 rounded-lg p-3">
                <h4 class="font-medium text-white">Ethisch</h4>
                <ul class="mt-2 text-gray-300 space-y-1">
                  <li>• Minimale dataretentie (7d)</li>
                  <li>• Transparantie naar stakeholders</li>
                  <li>• Geen profiling buiten scope</li>
                </ul>
              </div>

              <div class="bg-gray-900/50 rounded-lg p-3">
                <h4 class="font-medium text-white">Operationeel</h4>
                <ul class="mt-2 text-gray-300 space-y-1">
                  <li>• SLA voor menselijke verificatie (≤5 min)</li>
                  <li>• Escalatie & fail-safes</li>
                  <li>• Training & logging voor bias</li>
                </ul>
              </div>
            </div>

            <div class="mt-4 flex flex-wrap gap-2">
              <button onclick="downloadMockup()" class="px-3 py-2 rounded-lg bg-primary text-black text-sm">📥 Download mockup (PDF)</button>
              <button onclick="openChecklist()" class="px-3 py-2 rounded-lg bg-gray-700 text-sm">🔎 SCAN Readiness Check</button>
              <button onclick="showStakeholderView()" class="px-3 py-2 rounded-lg bg-gray-700 text-sm">👥 Simuleer stakeholder feedback</button>
            </div>
          </div>

          <!-- Timeline -->
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h3 class="font-semibold">Tijdlijn</h3>
            <div class="mt-3 space-y-3 text-sm text-gray-300">
              <div class="flex justify-between"><div>22:15:33 — Detectie door Camera A12</div><div>AI</div></div>
              <div class="flex justify-between"><div>22:15:45 — Prioriteit: Hoog</div><div>Automatisch</div></div>
              <div class="flex justify-between"><div>22:16:08 — Match onzeker (68%)</div><div>AI</div></div>
              <div class="flex justify-between"><div>22:16:20 — Menselijke validatie gestart</div><div>Operator</div></div>
            </div>
          </div>
        </section>

        <!-- Right column: system & ethics score -->
        <aside class="space-y-4">
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h4 class="text-lg font-semibold">Systeem Overzicht</h4>
            <div class="mt-3 text-sm text-gray-300 space-y-3">
              <div class="flex justify-between"><span>Actieve camera's</span><span class="font-semibold">14</span></div>
              <div class="flex justify-between"><span>Open incidenten</span><span class="font-semibold text-yellow-300">3</span></div>
              <div class="flex justify-between"><span>Uptime</span><span class="font-semibold text-green-300">99%</span></div>
            </div>

            <div class="mt-4">
              <h5 class="text-sm text-gray-400">Ethische score (PoC)</h5>
              <div class="w-full bg-gray-700 rounded-full h-3 mt-2">
                <div id="ethScoreBar" class="h-3 rounded-full" style="width:72%;background:linear-gradient(90deg,#f59e0b,#10b981);"></div>
              </div>
              <div class="text-xs text-gray-400 mt-1">72% — Meeteenheid op basis van: transparantie, menselijke validatie, minimale data-retentie</div>
            </div>
          </div>

          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h4 class="text-lg font-semibold text-green-300">Privacy & Compliance</h4>
            <ul class="mt-3 text-sm text-gray-300 space-y-2">
              <li>✓ AVG: processing document beschikbaar</li>
              <li>✓ Audit logs: 30 dagen (demo)</li>
              <li>✓ Menselijke validatie verplicht</li>
            </ul>
          </div>

          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h4 class="text-lg font-semibold">Stakeholder Quick Wins</h4>
            <ol class="mt-2 text-sm text-gray-300 list-decimal ml-4 space-y-1">
              <li>Transparante rapportage per incident</li>
              <li>Escalaties met SLA</li>
              <li>Periodieke bias-audit</li>
            </ol>
          </div>
        </aside>
      </div>

      <!-- Bottom: stakeholder panel (hidden by default) -->
      <section id="stakeholderPanel" class="hidden bg-gray-800 rounded-lg p-4 border border-gray-700">
        <div class="flex items-start justify-between">
          <div>
            <h3 class="text-lg font-semibold">Stakeholder Review</h3>
            <p class="text-xs text-gray-400 mt-1">Gebruik dit scherm om beslissingen en toelichting te presenteren.</p>
          </div>

          <div class="flex items-center gap-2">
            <button onclick="exportReport()" class="px-3 py-2 rounded-lg bg-primary text-black text-sm">📄 Exporteer report</button>
            <button onclick="closeStakeholder()" class="px-3 py-2 rounded-lg bg-gray-700 text-sm">Sluit</button>
          </div>
        </div>

        <div class="mt-4 grid grid-cols-1 md:grid-cols-2 gap-4">
          <div class="bg-gray-900/50 rounded-lg p-3">
            <h4 class="font-medium">Belangrijkste beslissingen</h4>
            <ul class="mt-2 text-sm text-gray-300 space-y-1">
              <li>• Menselijke verificatie bij < 85% zekerheid</li>
              <li>• Bewaarbeeld 7 dagen, logs 30 dagen</li>
              <li>• Geen externe profiling</li>
            </ul>
          </div>

          <div class="bg-gray-900/50 rounded-lg p-3">
            <h4 class="font-medium">Stakeholder feedback</h4>
            <div class="mt-2 text-sm text-gray-300">
              <textarea id="feedback" class="w-full bg-transparent border border-gray-700 rounded p-2 text-sm" rows="4" placeholder="Voer opmerkingen in..."></textarea>
              <div class="mt-2 flex justify-end">
                <button onclick="submitFeedback()" class="px-3 py-1 rounded-lg bg-primary text-black text-sm">Verstuur</button>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="bg-gray-800 border-t border-gray-700 px-6 py-4 mt-6">
      <div class="max-w-7xl mx-auto text-center text-sm text-gray-400">
        <p>EthicalScan — Prototype voor Workshop 3: Ethisch Experimenteren · Ontwerp sprint</p>
        <p class="text-xs mt-1">Gebruik dit prototype als mockup; alle acties zijn gesimuleerd voor demo-doeleinden.</p>
      </div>
    </footer>

    <script>
      // Simulated state
      let ethScore = 72;
      let conf = 68;

      function switchPerspective(p) {
        document.getElementById('stakeholderPanel')?.classList.add('hidden');
        document.getElementById('btnOp').classList.remove('bg-gray-700','text-gray-200');
        document.getElementById('btnSt').classList.remove('bg-primary','text-black');

        if (p === 'operator') {
          document.getElementById('btnOp').classList.add('bg-primary');
          document.getElementById('btnSt').classList.add('bg-gray-700','text-gray-200');
        } else {
          document.getElementById('btnSt').classList.add('bg-primary','text-black');
          document.getElementById('btnOp').classList.add('bg-gray-700','text-gray-200');
          document.getElementById('stakeholderPanel').classList.remove('hidden');
        }
      }

      function escalate(){
        document.getElementById('prioTag').textContent = 'CRITISCH';
        document.getElementById('prioTag').className = 'font-semibold text-red-400';
        addAudit('Operator escalatie naar kritisch');
        notify('Escaleer: Beveiliging gewaarschuwd');
      }

      function verifyManually(){
        document.getElementById('actionTag').textContent = 'Validatie gestart';
        addAudit('Menselijke validatie gestart');
        notify('Menselijke verificatie: Team B richt zich op locatie');
      }

      function tagFalsePositive(){
        addAudit('Gerapporteerd: mogelijk vals positief');
        notify('Incident gemarkeerd als mogelijk vals positief');
      }

      function requestLiveness(){
        addAudit('Liveness check gestart');
        notify('Liveness check: In uitvoering...');
        // Fake result after 2s
        setTimeout(()=>{
          notify('Liveness check: geslaagd (score 98%)');
          addAudit('Liveness check: geslaagd');
          conf = Math.min(95, conf + 12);
          document.getElementById('conf').textContent = conf + '%';
          updateEthScore(4);
        },2000);
      }

      function addAudit(text){
        console.log('AUDIT:', text);
        // append to timeline (simple)
        const div = document.createElement('div');
        div.className = 'flex justify-between text-xs text-gray-400 mt-2';
        div.innerHTML = '<div>'+new Date().toLocaleTimeString()+' — '+text+'</div><div>Operator</div>';
        document.querySelector('main').appendChild(div);
      }

      function downloadMockup(){
        notify('Download mockup: (gesimuleerd) - gebruik print->PDF in browser');
      }

      function openChecklist(){
        alert('SCAN Readiness Check:\n1) Strategie: Ja\n2) Cultuur: Training vereist\n3) Adopt: Pilot fase\n4) Next: Opschaling planning');
      }

      function showStakeholderView(){
        switchPerspective('stakeholder');
        window.scrollTo({top:document.body.scrollHeight,behavior:'smooth'});
      }

      function exportReport(){
        notify('Report gegenereerd (demo)');
      }

      function submitFeedback(){
        const v = document.getElementById('feedback').value.trim();
        if(!v){ notify('Voer feedback in voordat u verzendt'); return; }
        notify('Feedback verzonden: dank!');
        addAudit('Feedback stakeholder: '+v);
        document.getElementById('feedback').value = '';
      }

      function closeStakeholder(){
        document.getElementById('stakeholderPanel').classList.add('hidden');
        switchPerspective('operator');
      }

      function notify(msg){
        const el = document.createElement('div');
        el.className = 'fixed right-6 bottom-6 bg-gray-800 border border-gray-700 text-sm text-gray-200 px-4 py-2 rounded shadow';
        el.textContent = msg;
        document.body.appendChild(el);
        setTimeout(()=>{ el.remove(); },3000);
      }

      function updateEthScore(delta){
        ethScore = Math.min(100, ethScore + delta);
        const bar = document.getElementById('ethScoreBar');
        bar.style.width = ethScore + '%';
      }

      // init
      document.addEventListener('DOMContentLoaded', ()=>{
        switchPerspective('operator');
        document.getElementById('timeNow').textContent = new Date().toLocaleTimeString();
      });
    </script>
  </body>
</html>