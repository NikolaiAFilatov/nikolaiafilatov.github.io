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
        flex: 1;
        min-height: 100%;
      }
    </style>
  </head>

  <body class="bg-gray-900 text-gray-100 font-sans antialiased">
    <!-- Topbar -->
    <header class="bg-gray-800 border-b border-gray-700 px-6 py-4">
      <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <div
            class="w-10 h-10 rounded-lg bg-primary flex items-center justify-center font-bold text-lg"
          >
            ES
          </div>
          <div>
            <h1 class="text-xl font-semibold">EthicalScan — Operationeel Systeem</h1>
            <p class="text-xs text-gray-400">
              Veiligheids- en ethiekmonitoringplatform
            </p>
          </div>
        </div>

        <div class="flex items-center gap-4">
          <div class="text-sm text-gray-400" id="dateNow"></div>
          <div
            id="systemStatus"
            class="flex items-center gap-2 px-3 py-1 rounded-full text-sm bg-green-500/20 text-green-300"
          >
            <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
            Systeem: Actief
          </div>
        </div>
      </div>
    </header>

    <main class="max-w-7xl mx-auto px-6 py-6 space-y-6">
      <!-- Intro + role switch -->
      <section
        class="flex flex-col md:flex-row md:items-center md:justify-between gap-4"
      >
        <div>
          <h2 class="text-2xl font-bold">Incident Monitoring & Ethiek</h2>
          <p class="text-sm text-gray-400 mt-1">
            Realtime incidentanalyse, menselijke verificatie en ethische waarborgen.
          </p>
        </div>

        <div class="flex gap-2">
          <button
            onclick="switchPerspective('operator')"
            id="btnOp"
            class="px-4 py-2 rounded-lg bg-primary text-black font-medium"
          >
            🔒 Operator
          </button>
          <button
            onclick="switchPerspective('stakeholder')"
            id="btnSt"
            class="px-4 py-2 rounded-lg bg-gray-700 text-gray-200"
          >
            👥 Stakeholder
          </button>
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

              <div id="operatorControls" class="flex items-center gap-2">
                <button
                  onclick="escalate()"
                  class="px-3 py-1 rounded-lg bg-danger text-black text-sm font-semibold"
                >
                  Escaleer
                </button>
                <button
                  onclick="verifyManually()"
                  class="px-3 py-1 rounded-lg bg-gray-700 text-gray-200 text-sm"
                >
                  Start menselijke verificatie
                </button>
              </div>
            </div>

            <div class="mt-4 grid grid-cols-1 md:grid-cols-3 gap-4">
              <!-- CAMERA -->
              <div class="md:col-span-2 flex items-stretch">
                <div
                  class="w-full aspect-square bg-gray-900 rounded-lg p-3 cam-placeholder relative flex items-center justify-center"
                >
                  <div
                    id="camOverlay"
                    class="absolute inset-0 flex items-center justify-center pointer-events-none"
                  >
                    <div class="text-center">
                      <div
                        class="mx-auto w-40 h-40 border-4 rounded-lg border-yellow-500 flex items-center justify-center text-6xl"
                      >
                        👤
                      </div>
                      <div
                        class="mt-2 text-sm text-yellow-300"
                        id="cameraTitle"
                      ></div>
                      <div
                        class="mt-1 text-xs text-gray-400"
                        id="detectionText"
                      ></div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- INFO -->
              <div class="bg-gray-900 rounded-lg p-3 border border-gray-700">
                <h4 class="font-semibold">Kerninformatie</h4>
                <div class="mt-3 space-y-2 text-sm text-gray-300">
                  <div class="flex justify-between">
                    <span>Locatie</span><span>Voorzijde - Dock</span>
                  </div>
                  <div class="flex justify-between">
                    <span>Tijd</span><span id="timeNow"></span>
                  </div>
                  <div class="flex justify-between">
                    <span>AI Betrouwbaarheid</span
                    ><span id="conf" class="font-semibold text-yellow-300"
                      >68%</span
                    >
                  </div>
                  <div class="flex justify-between">
                    <span>Actie</span
                    ><span id="actionTag" class="text-sm text-gray-300"
                      >Wacht op validatie</span
                    >
                  </div>
                </div>

                <div class="mt-3 pt-3 border-t border-gray-700">
                  <h5 class="text-sm font-medium">Ethische Flags</h5>
                  <ul class="mt-2 text-xs text-gray-400 space-y-1" id="flags">
                    <li>⚠️ Analyse vereist</li>
                    <li>ℹ️ Publieke ruimte</li>
                    <li>🔒 Bewaartermijn: 7 dagen</li>
                  </ul>
                </div>
              </div>
            </div>

            <div
              class="mt-4 bg-gray-800/40 rounded-lg p-3 border border-gray-700 text-sm"
            >
              <strong>AI Analyse:</strong>
              <span id="aiAnalysis"
                >Model detecteert onregelmatig gedrag. Aanbevolen: menselijke
                verificatie.</span
              >
            </div>

            <div id="operatorButtons" class="mt-4 flex gap-3 text-sm">
              <button
                onclick="tagFalsePositive()"
                class="px-3 py-2 rounded-lg bg-gray-700"
              >
                Als vals positief markeren
              </button>
              <button
                onclick="requestLiveness()"
                class="px-3 py-2 rounded-lg bg-primary text-black"
              >
                Start liveness check
              </button>
              <button
                onclick="addAudit('Operator twijfel')"
                class="px-3 py-2 rounded-lg bg-gray-700"
              >
                Voeg audit entry toe
              </button>
            </div>
          </div>

          <!-- Tijdlijn -->
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h3 class="font-semibold">Tijdlijn</h3>
            <div id="timeline" class="mt-3 space-y-2 text-sm text-gray-300">
              <div class="flex justify-between">
                <div>Detectie gestart</div>
                <div>AI</div>
              </div>
            </div>
          </div>
        </section>

        <!-- Right column -->
        <aside id="stakeholderPanel" class="space-y-4 hidden">
          <div class="bg-gray-800 rounded-lg p-4 border border-gray-700">
            <h4 class="text-lg font-semibold">Stakeholder Acties</h4>
            <p class="text-sm text-gray-400 mt-1">
              Geef feedback of keur beslissingen goed.
            </p>
            <div class="mt-3">
              <textarea
                id="feedback"
                rows="4"
                class="w-full bg-gray-900 border border-gray-700 rounded p-2 text-sm"
                placeholder="Voer feedback of besluit in..."
              ></textarea>
              <div class="mt-2 flex flex-wrap gap-2 justify-end">
                <button
                  onclick="stakeholderApprove()"
                  class="px-3 py-1 rounded-lg bg-success text-black text-sm"
                >
                  ✅ Goedkeuren
                </button>
                <button
                  onclick="stakeholderReject()"
                  class="px-3 py-1 rounded-lg bg-danger text-black text-sm"
                >
                  ❌ Afkeuren
                </button>
                <button
                  onclick="stakeholderRequestContext()"
                  class="px-3 py-1 rounded-lg bg-warn text-black text-sm"
                >
                  📄 Meer context vragen
                </button>
                <button
                  onclick="submitFeedback()"
                  class="px-3 py-1 rounded-lg bg-primary text-black text-sm"
                >
                  💬 Verstuur feedback
                </button>
              </div>
            </div>
          </div>
        </aside>
      </div>
    </main>

    <script>
      // --- INIT ---
      const detections = [
        "Bivakmuts gedetecteerd · lage zekerheid",
        "Verdachte houding — analyse vereist",
        "Persoon met masker — gedeeltelijke dekking",
        "Onherkenbaar gezicht — beweging gedetecteerd",
        "Normale situatie — geen actie nodig",
      ];

      const analysisTexts = [
        "Model detecteert onregelmatig gedrag.",
        "Gedragsafwijking waargenomen. Controle aanbevolen.",
        "Gezichtsdekking aanwezig — beperkte betrouwbaarheid.",
        "Activiteit conform patroon, validatie lopend.",
      ];

      let conf = 68;

      document.addEventListener("DOMContentLoaded", () => {
        const now = new Date();
        document.getElementById("timeNow").textContent = now.toLocaleTimeString();
        document.getElementById("dateNow").textContent = now.toLocaleDateString();
        document.getElementById("incidentId").textContent =
          "2025-" + (now.getMonth() + 1) + "-" + now.getDate() + "-001";

        randomizeDetection();
      });

      // --- BASIC UTILS ---
      function randomizeDetection() {
        const det = detections[Math.floor(Math.random() * detections.length)];
        document.getElementById("detectionText").textContent = det;
        document.getElementById("cameraTitle").textContent =
          "LIVE · Camera A12 — Voordeur";
        document.getElementById("aiAnalysis").textContent =
          analysisTexts[Math.floor(Math.random() * analysisTexts.length)];
      }

      function addTimeline(text, actor = "AI") {
        const tl = document.getElementById("timeline");
        const div = document.createElement("div");
        div.className = "flex justify-between";
        div.innerHTML =
          `<div>${new Date().toLocaleTimeString()} — ${text}</div><div>${actor}</div>`;
        tl.appendChild(div);
      }

      function notify(msg) {
        const el = document.createElement("div");
        el.className =
          "fixed right-6 bottom-6 bg-gray-800 border border-gray-700 text-sm text-gray-200 px-4 py-2 rounded shadow";
        el.textContent = msg;
        document.body.appendChild(el);
        setTimeout(() => el.remove(), 3000);
      }

      // --- OPERATOR FUNCTIONS ---
      function escalate() {
        document.getElementById("prioTag").textContent = "CRITISCH";
        document.getElementById("prioTag").className =
          "font-semibold text-red-400";
        addTimeline("Incident geëscaleerd", "Operator");
        notify("Escaleer: Beveiliging gewaarschuwd");
      }

      function verifyManually() {
        document.getElementById("actionTag").textContent = "Validatie gestart";
        addTimeline("Menselijke verificatie gestart", "Operator");
        notify("Menselijke verificatie gestart");
      }

      function tagFalsePositive() {
        addTimeline("Incident gemarkeerd als vals positief", "Operator");
        notify("Gemarkeerd als vals positief");
      }

      function requestLiveness() {
        addTimeline("Liveness check gestart", "Operator");
        notify("Liveness check bezig...");
        setTimeout(() => {
          conf = Math.min(95, conf + 12);
          document.getElementById("conf").textContent = conf + "%";
          addTimeline("Liveness check geslaagd", "AI");
          notify("Liveness check geslaagd (score 98%)");
        }, 2000);
      }

      // --- ROLE SWITCH ---
      function switchPerspective(role) {
        const opControls = document.getElementById("operatorControls");
        const opBtns = document.getElementById("operatorButtons");
        const shPanel = document.getElementById("stakeholderPanel");
        const btnOp = document.getElementById("btnOp");
        const btnSt = document.getElementById("btnSt");

        if (role === "stakeholder") {
          shPanel.classList.remove("hidden");
          opControls.classList.add("hidden");
          opBtns.classList.add("hidden");
          btnSt.classList.add("bg-primary", "text-black");
          btnOp.classList.remove("bg-primary", "text-black");
          addTimeline("Perspectief gewijzigd naar Stakeholder", "Systeem");
          notify("Stakeholdermodus geactiveerd");
        } else {
          shPanel.classList.add("hidden");
          opControls.classList.remove("hidden");
          opBtns.classList.remove("hidden");
          btnOp.classList.add("bg-primary", "text-black");
          btnSt.classList.remove("bg-primary", "text-black");
          addTimeline("Perspectief gewijzigd naar Operator", "Systeem");
          notify("Operatormodus geactiveerd");
        }
      }

      // --- STAKEHOLDER FUNCTIONS ---
      function submitFeedback() {
        const fb = document.getElementById("feedback").value.trim();
        if (!fb) {
          notify("Voer eerst feedback in");
          return;
        }
        addTimeline("Feedback ontvangen: " + fb, "Stakeholder");
        notify("Feedback verzonden");
        document.getElementById("feedback").value = "";
      }

      function stakeholderApprove() {
        addTimeline("Stakeholder keurde actie goed", "Stakeholder");
        notify("Actie goedgekeurd");
      }

      function stakeholderReject() {
        addTimeline("Stakeholder wees actie af", "Stakeholder");
        notify("Actie afgekeurd");
      }

      function stakeholderRequestContext() {
        addTimeline("Stakeholder vroeg om extra context", "Stakeholder");
        notify("Contextverzoek verzonden");
      }

      function addAudit(text) {
        addTimeline("Audit: " + text, "Operator");
      }
    </script>
  </body>
</html>

<!-- ending -->