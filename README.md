<html lang="nl" class="dark">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>EthicalScan — Operationeel Systeem</title>
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
              success: "#10b981",
            },
          },
        },
      };
    </script>
    <style>
      .cam-placeholder {
        background: linear-gradient(135deg, #0f172a 0%, #0b1220 100%);
        border: 1px dashed rgba(255, 255, 255, 0.04);
      }
    </style>
  </head>

  <body class="bg-gray-900 text-gray-100 font-sans antialiased">
    <!-- Topbar -->
    <header class="bg-gray-800 border-b border-gray-700 px-6 py-4">
      <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-lg bg-primary flex items-center justify-center font-bold text-lg">
            ES
          </div>
          <div>
            <h1 class="text-xl font-semibold">EthicalScan — Operationeel Systeem</h1>
            <p class="text-xs text-gray-400">Veiligheids- en ethiekmonitoringplatform</p>
          </div>
        </div>

        <div class="flex items-center gap-4">
          <div class="text-sm text-gray-400" id="dateNow"></div>
          <div id="systemStatus"
            class="flex items-center gap-2 px-3 py-1 rounded-full text-sm bg-green-500/20 text-green-300">
            <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
            Systeem: Actief
          </div>
        </div>
      </div>
    </header>

    <main class="max-w-7xl mx-auto px-6 py-6 space-y-6">
      <!-- Intro + role switch -->
      <section class="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <div>
          <h2 class="text-2xl font-bold">Incident Monitoring & Ethiek</h2>
          <p class="text-sm text-gray-400 mt-1">
            Realtime incidentanalyse, menselijke verificatie en ethische waarborgen.
          </p>
        </div>

        <div class="flex gap-2">
          <button onclick="switchPerspective('operator')" id="btnOp"
            class="px-4 py-2 rounded-lg bg-primary text-black font-medium">🔒 Operator</button>
          <button onclick="switchPerspective('stakeholder')" id="btnSt"
            class="px-4 py-2 rounded-lg bg-gray-700 text-gray-200">👥 Stakeholder</button>
        </div>
      </section>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <!-- Main panel -->
        <section class="lg:col-span-2 space-y-4">
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <div class="flex items-start justify-between gap-4">
              <div>
                <h3 class="text-lg font-semibold">Actief Incident — Camera A12</h3>
                <p class="text-xs text-gray-400">
                  Incident #<span id="incidentId"></span> · Prioriteit:
                  <span id="prioTag" class="font-semibold text-yellow-300">Hoog</span>
                </p>
              </div>

              <div class="flex items-center gap-2">
                <button onclick="escalate()" class="px-3 py-1 rounded-lg bg-danger text-black text-sm font-semibold">
                  Escaleer
                </button>
                <button onclick="verifyManually()"
                  class="px-3 py-1 rounded-lg bg-gray-700 text-gray-200 text-sm">
                  Start menselijke verificatie
                </button>
              </div>
            </div>

            <div class="mt-4 grid grid-cols-1 md:grid-cols-3 gap-4">
              <div class="md:col-span-2 bg-gray-900 rounded-lg p-3 cam-placeholder relative aspect-video">
                <div id="camOverlay"
                  class="absolute inset-0 flex items-center justify-center pointer-events-none">
                  <div class="text-center">
                    <div
                      class="mx-auto w-28 h-28 border-4 rounded-lg border-yellow-500 flex items-center justify-center text-5xl">
                      👤
                    </div>
                    <div class="mt-2 text-sm text-yellow-300" id="cameraTitle"></div>
                    <div class="mt-1 text-xs text-gray-400" id="detectionText"></div>
                  </div>
                </div>
              </div>

              <div class="bg-gray-900 rounded-lg p-3 border border-gray-700">
                <h4 class="font-semibold">Kerninformatie</h4>
                <div class="mt-3 space-y-2 text-sm text-gray-300">
                  <div class="flex justify-between"><span>Locatie</span><span>Voorzijde - Dock</span></div>
                  <div class="flex justify-between"><span>Tijd</span><span id="timeNow">--:--:--</span></div>
                  <div class="flex justify-between"><span>AI Betrouwbaarheid</span>
                    <span id="conf" class="font-semibold text-yellow-300">--%</span></div>
                  <div class="flex justify-between"><span>Actie</span>
                    <span id="actionTag" class="text-sm text-gray-300">Wacht op validatie</span></div>
                </div>

                <div class="mt-3 pt-3 border-t border-gray-700">
                  <h5 class="text-sm font-medium">Ethische Flags</h5>
                  <ul id="ethFlags" class="mt-2 text-xs text-gray-400 space-y-1"></ul>
                </div>
              </div>
            </div>

            <div class="mt-4 bg-gray-800/40 rounded-lg p-3 border border-gray-700 text-sm" id="aiAnalysis">
              <strong>AI Analyse:</strong> Data wordt geladen...
            </div>
          </div>

          <!-- Tijdlijn -->
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h3 class="font-semibold mb-3">Tijdlijn</h3>
            <div id="timeline" class="space-y-2 text-sm text-gray-300"></div>
          </div>
        </section>

        <!-- Sidebar -->
        <aside class="space-y-4">
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h4 class="text-lg font-semibold">Systeem Overzicht</h4>
            <div class="mt-3 text-sm text-gray-300 space-y-3">
              <div class="flex justify-between"><span>Actieve camera's</span><span class="font-semibold">14</span></div>
              <div class="flex justify-between"><span>Open incidenten</span><span class="font-semibold text-yellow-300">3</span></div>
              <div class="flex justify-between"><span>Uptime</span><span class="font-semibold text-green-300">99%</span></div>
            </div>
          </div>

          <!-- Stakeholder acties -->
          <div id="stakeholderPanel" class="bg-gray-800 rounded-lg p-4 border border-gray-700 hidden">
            <h4 class="text-lg font-semibold">Stakeholder Acties</h4>
            <div class="mt-3 text-sm text-gray-300">
              <label class="block text-gray-400 mb-1">Feedback:</label>
              <textarea id="feedback" class="w-full bg-gray-900 border border-gray-700 rounded p-2 text-sm" rows="3"
                placeholder="Voer uw opmerkingen in..."></textarea>

              <label class="block text-gray-400 mt-3 mb-1">Beslissing:</label>
              <select id="decision" class="w-full bg-gray-900 border border-gray-700 rounded p-2 text-sm">
                <option value="">Kies een beslissing...</option>
                <option value="goedgekeurd">✅ Goedkeuren</option>
                <option value="herzien">🔁 Herziening vereist</option>
                <option value="afgesloten">🗂️ Afsluiten incident</option>
              </select>

              <div class="flex justify-end gap-2 mt-3">
                <button onclick="submitStakeholder()" class="px-3 py-1 rounded-lg bg-primary text-black text-sm">
                  Verstuur
                </button>
                <button onclick="exportReport()" class="px-3 py-1 rounded-lg bg-gray-700 text-sm">
                  📄 Exporteer rapport
                </button>
              </div>
            </div>
          </div>
        </aside>
      </div>
    </main>

    <footer class="bg-gray-800 border-t border-gray-700 px-6 py-4 mt-6 text-center text-sm text-gray-400">
      <p>© 2025 EthicalScan — Operationeel Monitoringplatform</p>
    </footer>

    <script>
      const detections = [
        { text: "Persoon geïdentificeerd — lage zekerheid", flags: ["👤 Identificatie gestart", "⚠️ Onvolledig beeld"] },
        { text: "Gezicht gedeeltelijk bedekt — mogelijk masker", flags: ["⚠️ Masker gedetecteerd", "🔒 Privacyzone geactiveerd"] },
        { text: "Verdachte houding — analyse vereist", flags: ["🕵️ Gedragsafwijking", "⚠️ Lage zekerheid (AI)"] },
        { text: "Onherkenbaar gezicht — camera obstructie", flags: ["⚠️ Beeldkwaliteit laag", "🔒 Herkenning gedeactiveerd"] },
        { text: "Geen afwijkingen — routinebeeld", flags: ["✅ Geen ethische flags actief"] }
      ];

      function randomDetection() {
        const d = detections[Math.floor(Math.random() * detections.length)];
        document.getElementById("detectionText").textContent = d.text;
        const flags = document.getElementById("ethFlags");
        flags.innerHTML = d.flags.map(f => `<li>${f}</li>`).join("");
        document.getElementById("aiAnalysis").innerHTML =
          "<strong>AI Analyse:</strong> " + d.text + ". Menselijke verificatie aanbevolen.";
        const conf = 60 + Math.floor(Math.random() * 30);
        document.getElementById("conf").textContent = conf + "%";
        addTimeline("AI-detectie uitgevoerd: " + d.text, "AI");
      }

      function addTimeline(text, actor) {
        const t = new Date().toLocaleTimeString("nl-NL");
        const div = document.createElement("div");
        div.innerHTML = `<div class='flex justify-between'><div>${t} — ${text}</div><div>${actor}</div></div>`;
        document.getElementById("timeline").prepend(div);
      }

      function escalate() {
        document.getElementById("prioTag").textContent = "CRITISCH";
        document.getElementById("prioTag").className = "font-semibold text-red-400";
        addTimeline("Incident geëscaleerd naar kritisch niveau", "Operator");
        notify("Incident geëscaleerd");
      }

      function verifyManually() {
        document.getElementById("actionTag").textContent = "Validatie gestart";
        addTimeline("Menselijke verificatie gestart", "Operator");
        notify("Menselijke verificatie gestart");
      }

      function switchPerspective(p) {
        const st = document.getElementById("stakeholderPanel");
        if (p === "stakeholder") {
          document.getElementById("btnSt").classList.add("bg-primary", "text-black");
          document.getElementById("btnOp").classList.remove("bg-primary", "text-black");
          st.classList.remove("hidden");
          addTimeline("Stakeholder heeft toegang gekregen tot het incident", "Systeem");
        } else {
          document.getElementById("btnOp").classList.add("bg-primary", "text-black");
          document.getElementById("btnSt").classList.remove("bg-primary", "text-black");
          st.classList.add("hidden");
        }
      }

      function submitStakeholder() {
        const feedback = document.getElementById("feedback").value.trim();
        const decision = document.getElementById("decision").value;
        if (!feedback && !decision) return notify("Voer feedback of een beslissing in.");
        if (feedback) addTimeline("Feedback ontvangen: " + feedback, "Stakeholder");
        if (decision) addTimeline("Beslissing genomen: " + decision, "Stakeholder");
        notify("Stakeholderactie opgeslagen");
        document.getElementById("feedback").value = "";
        document.getElementById("decision").value = "";
      }

      function exportReport() {
        addTimeline("Incidentrapport geëxporteerd", "Stakeholder");
        notify("Rapport geëxporteerd (simulatie)");
      }

      function notify(msg) {
        const el = document.createElement("div");
        el.className =
          "fixed right-6 bottom-6 bg-gray-800 border border-gray-700 text-sm text-gray-200 px-4 py-2 rounded shadow";
        el.textContent = msg;
        document.body.appendChild(el);
        setTimeout(() => el.remove(), 3000);
      }

      document.addEventListener("DOMContentLoaded", () => {
        document.getElementById("dateNow").textContent = new Date().toLocaleDateString("nl-NL");
        document.getElementById("timeNow").textContent = new Date().toLocaleTimeString("nl-NL");
        document.getElementById("incidentId").textContent = Date.now().toString().slice(-6);
        document.getElementById("cameraTitle").textContent = "LIVE · Camera A12 — Voordeur";
        randomDetection();
        switchPerspective("operator");
      });
    </script>
  </body>
</html>
