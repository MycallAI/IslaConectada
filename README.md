<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IslaConectada | 5G Infrastructure Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Lato:wght@300;400;700;900&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        indigo: {
                            950: '#1e1b4b',
                        }
                    },
                    animation: {
                        'blob': 'blob 7s infinite',
                        'pulse-slow': 'pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                    },
                    keyframes: {
                        blob: {
                            '0%': { transform: 'translate(0px, 0px) scale(1)' },
                            '33%': { transform: 'translate(30px, -50px) scale(1.1)' },
                            '66%': { transform: 'translate(-20px, 20px) scale(0.9)' },
                            '100%': { transform: 'translate(0px, 0px) scale(1)' },
                        }
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Lato', sans-serif;
            background-color: #f5f5f4;
            overflow-x: hidden;
        }
        
        h1, h2, h3 {
            font-family: 'Playfair Display', serif;
        }

        /* Modern Background Mesh */
        .bg-mesh {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: -1;
            background: radial-gradient(circle at 15% 50%, rgba(224, 231, 255, 0.8), transparent 25%), 
                        radial-gradient(circle at 85% 30%, rgba(245, 245, 244, 1), transparent 25%);
        }

        /* Glassmorphism Card Style */
        .glass-card {
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.5);
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.05);
        }

        /* Scroll Reveal Classes */
        .reveal {
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s ease-out;
        }
        .reveal.active {
            opacity: 1;
            transform: translateY(0);
        }

        /* Chart Containers */
        .chart-container {
            position: relative;
            width: 100%;
            height: 250px;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f5f5f4; }
        ::-webkit-scrollbar-thumb { background: #a8a29e; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #78716c; }

        .tech-btn {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .tech-btn:hover {
            transform: translateX(5px);
            background: rgba(255,255,255,0.9);
        }
        .tech-btn.active-tab {
            background: linear-gradient(90deg, #e0e7ff 0%, rgba(255,255,255,0) 100%);
            border-left: 4px solid #312e81;
            padding-left: 1.5rem; 
        }

        .gradient-text {
            background: linear-gradient(135deg, #312e81 0%, #4338ca 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
    </style>
</head>
<body class="text-stone-800 antialiased selection:bg-indigo-100 selection:text-indigo-900">

    <div class="bg-mesh"></div>
    <div class="fixed top-20 left-10 w-72 h-72 bg-indigo-200 rounded-full mix-blend-multiply filter blur-3xl opacity-30 animate-blob -z-10"></div>
    <div class="fixed bottom-20 right-10 w-72 h-72 bg-stone-300 rounded-full mix-blend-multiply filter blur-3xl opacity-30 animate-blob animation-delay-2000 -z-10"></div>

    <header class="glass-card sticky top-4 mx-4 sm:mx-8 rounded-2xl z-50 transition-all duration-300">
        <div class="max-w-7xl mx-auto px-6 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3 cursor-pointer" onclick="window.scrollTo(0,0)">
                <div class="w-8 h-8 bg-indigo-900 rounded-lg flex items-center justify-center text-white font-bold">I</div>
                <span class="text-xl font-bold tracking-tight text-indigo-950">Isla<span class="font-light">Conectada</span></span>
            </div>
            
            <nav class="hidden md:flex space-x-1">
                <button onclick="scrollToSection('overview')" class="px-4 py-2 text-sm font-medium text-stone-600 hover:text-indigo-900 rounded-lg hover:bg-white/50 transition-all">Overview</button>
                <button onclick="scrollToSection('technology')" class="px-4 py-2 text-sm font-medium text-stone-600 hover:text-indigo-900 rounded-lg hover:bg-white/50 transition-all">Technology</button>
                <button onclick="scrollToSection('impact')" class="px-4 py-2 text-sm font-medium text-stone-600 hover:text-indigo-900 rounded-lg hover:bg-white/50 transition-all">Impact</button>
            </nav>

            <div class="flex items-center gap-3">
                <span class="relative flex h-3 w-3">
                  <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
                  <span class="relative inline-flex rounded-full h-3 w-3 bg-green-500"></span>
                </span>
                <span class="text-xs font-bold text-indigo-900 uppercase tracking-widest">Phase 2 Live</span>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12 space-y-16">

        <section id="overview" class="glass-card p-8 md:p-12 rounded-3xl shadow-lg reveal active relative overflow-hidden">
            <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-indigo-900 via-indigo-500 to-transparent"></div>
            
            <div class="grid lg:grid-cols-2 gap-12 items-center relative z-10">
                <div>
                    <h1 class="text-5xl md:text-6xl text-stone-900 mb-6 leading-tight">
                        Connecting Puerto Rico to the <span class="gradient-text">Future</span>.
                    </h1>
                    <p class="text-xl text-stone-600 mb-8 leading-relaxed">
                        IslaConectada is a paradigm shift designed to catalyze innovation. We are deploying ultra-wideband infrastructure to bridge the digital divide and enable the next generation of Smart City applications.
                    </p>
                    
                    <div class="flex flex-wrap gap-6">
                        <div class="flex flex-col border-l-4 border-indigo-900 pl-4">
                            <span class="text-sm text-stone-500 uppercase tracking-wider">Target Speed</span>
                            <span class="text-3xl font-bold text-stone-800 flex items-baseline">
                                <span class="counter" data-target="10">0</span>x
                                <span class="text-sm font-normal text-stone-500 ml-2">Faster than LTE</span>
                            </span>
                        </div>
                        <div class="flex flex-col border-l-4 border-stone-300 pl-4">
                            <span class="text-sm text-stone-500 uppercase tracking-wider">Latency</span>
                            <span class="text-3xl font-bold text-stone-800 flex items-baseline">
                                <<span class="counter" data-target="5">0</span>ms
                                <span class="text-sm font-normal text-stone-500 ml-2">Edge Response</span>
                            </span>
                        </div>
                    </div>
                </div>

                <div class="bg-indigo-950/5 rounded-2xl p-6 border border-indigo-100/50">
                    <h3 class="text-indigo-900 font-bold mb-4 flex items-center gap-2">
                        <span class="text-xl">⚡</span> Project Mandate
                    </h3>
                    <div class="space-y-4">
                        <div class="bg-white p-4 rounded-xl shadow-sm flex items-start gap-4 transition-transform hover:-translate-y-1 duration-300">
                            <div class="bg-indigo-100 p-2 rounded-lg text-indigo-700">🏗️</div>
                            <div>
                                <h4 class="font-bold text-stone-800">Macro Tower Retrofitting</h4>
                                <p class="text-sm text-stone-500 mt-1">Upgrading existing physical assets with Massive MIMO arrays.</p>
                            </div>
                        </div>
                        <div class="bg-white p-4 rounded-xl shadow-sm flex items-start gap-4 transition-transform hover:-translate-y-1 duration-300 delay-75">
                            <div class="bg-indigo-100 p-2 rounded-lg text-indigo-700">📡</div>
                            <div>
                                <h4 class="font-bold text-stone-800">Small Cell Densification</h4>
                                <p class="text-sm text-stone-500 mt-1">Increasing capacity in urban centers via street-level nodes.</p>
                            </div>
                        </div>
                        <div class="bg-white p-4 rounded-xl shadow-sm flex items-start gap-4 transition-transform hover:-translate-y-1 duration-300 delay-150">
                            <div class="bg-indigo-100 p-2 rounded-lg text-indigo-700">🛡️</div>
                            <div>
                                <h4 class="font-bold text-stone-800">Resilient Backbone</h4>
                                <p class="text-sm text-stone-500 mt-1">Hurricane-hardened fiber backhaul for consistent uptime.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="technology" class="space-y-8 reveal">
            <div class="max-w-3xl">
                <span class="text-indigo-600 font-bold tracking-widest uppercase text-xs">The Architecture</span>
                <h2 class="text-4xl text-stone-900 mt-2 mb-4">Technological Pillars</h2>
                <p class="text-stone-600 text-lg">
                    Interact with the modules below to explore the core components driving the IslaConectada ecosystem.
                </p>
            </div>

            <div class="grid md:grid-cols-12 gap-8">
                <div class="md:col-span-4 space-y-4">
                    <button onclick="updateTechView('5gnr')" id="btn-5gnr" class="tech-btn w-full text-left p-5 rounded-xl bg-white/60 border border-white shadow-sm active-tab group">
                        <div class="flex items-center justify-between">
                            <span class="font-bold text-stone-800 group-hover:text-indigo-900">5G NR Standard</span>
                            <span class="opacity-0 group-hover:opacity-100 text-indigo-600 transition-opacity">→</span>
                        </div>
                        <div class="text-xs text-stone-500 mt-1">Unified Wireless Interface</div>
                    </button>

                    <button onclick="updateTechView('edge')" id="btn-edge" class="tech-btn w-full text-left p-5 rounded-xl bg-white/60 border border-white shadow-sm group">
                        <div class="flex items-center justify-between">
                            <span class="font-bold text-stone-800 group-hover:text-indigo-900">Edge Computing</span>
                            <span class="opacity-0 group-hover:opacity-100 text-indigo-600 transition-opacity">→</span>
                        </div>
                        <div class="text-xs text-stone-500 mt-1">Zero-Latency Processing</div>
                    </button>

                    <button onclick="updateTechView('slicing')" id="btn-slicing" class="tech-btn w-full text-left p-5 rounded-xl bg-white/60 border border-white shadow-sm group">
                        <div class="flex items-center justify-between">
                            <span class="font-bold text-stone-800 group-hover:text-indigo-900">Network Slicing</span>
                            <span class="opacity-0 group-hover:opacity-100 text-indigo-600 transition-opacity">→</span>
                        </div>
                        <div class="text-xs text-stone-500 mt-1">Virtual Partitioning</div>
                    </button>

                    <button onclick="updateTechView('cells')" id="btn-cells" class="tech-btn w-full text-left p-5 rounded-xl bg-white/60 border border-white shadow-sm group">
                        <div class="flex items-center justify-between">
                            <span class="font-bold text-stone-800 group-hover:text-indigo-900">Densification</span>
                            <span class="opacity-0 group-hover:opacity-100 text-indigo-600 transition-opacity">→</span>
                        </div>
                        <div class="text-xs text-stone-500 mt-1">Spatial Capacity Reuse</div>
                    </button>
                </div>

                <div class="md:col-span-8 glass-card rounded-3xl p-8 relative flex flex-col justify-center min-h-[350px]">
                    <div id="tech-content" class="transition-all duration-500 ease-out transform translate-y-0 opacity-100">
                        </div>
                    <div class="absolute right-4 bottom-4 text-9xl opacity-5 pointer-events-none text-indigo-900" id="bg-icon">
                        📶
                    </div>
                </div>
            </div>
        </section>

        <section id="impact" class="space-y-8 reveal">
            <div class="max-w-3xl">
                <span class="text-indigo-600 font-bold tracking-widest uppercase text-xs">Validated Metrics</span>
                <h2 class="text-4xl text-stone-900 mt-2 mb-4">Societal & Economic Impact</h2>
            </div>

            <div class="grid lg:grid-cols-2 gap-8">
                
                <div class="glass-card p-8 rounded-3xl flex flex-col">
                    <div class="mb-6 flex justify-between items-end">
                        <div>
                            <h3 class="text-xl font-bold text-stone-800">Broadband Evolution</h3>
                            <p class="text-sm text-stone-500 mt-1">Average download speeds (Mbps)</p>
                        </div>
                        <div class="text-right">
                             <span class="text-xs font-bold bg-green-100 text-green-800 px-2 py-1 rounded">+900%</span>
                        </div>
                    </div>
                    <div class="flex-grow flex items-center justify-center relative">
                        <div class="chart-container">
                            <canvas id="speedChart"></canvas>
                        </div>
                    </div>
                </div>

                <div class="glass-card p-8 rounded-3xl flex flex-col">
                    <div class="mb-6">
                        <h3 class="text-xl font-bold text-stone-800">Smart City Pilots</h3>
                        <p class="text-sm text-stone-500 mt-1">Current efficiency gains by sector</p>
                    </div>

                    <div class="flex bg-stone-100 p-1 rounded-xl mb-6">
                        <button onclick="updatePilot('traffic')" id="tab-traffic" class="flex-1 py-2 text-xs font-bold rounded-lg transition-all text-white bg-indigo-900 shadow-sm">Traffic</button>
                        <button onclick="updatePilot('environment')" id="tab-environment" class="flex-1 py-2 text-xs font-bold rounded-lg text-stone-500 hover:text-stone-700 transition-all">Eco</button>
                        <button onclick="updatePilot('energy')" id="tab-energy" class="flex-1 py-2 text-xs font-bold rounded-lg text-stone-500 hover:text-stone-700 transition-all">Energy</button>
                    </div>

                    <div id="pilot-content" class="mb-4">
                        </div>

                    <div class="flex-grow flex items-center justify-center">
                        <div class="chart-container" style="height: 200px;">
                            <canvas id="pilotChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section class="mt-12 border-t border-stone-200 pt-12 pb-6 reveal">
            <div class="grid grid-cols-2 md:grid-cols-4 gap-6 text-center opacity-70 grayscale hover:grayscale-0 transition-all duration-500">
                <div class="flex items-center justify-center gap-2">
                    <span class="text-2xl">📡</span> <span class="font-bold text-stone-400">TechCo Global</span>
                </div>
                <div class="flex items-center justify-center gap-2">
                    <span class="text-2xl">🏛️</span> <span class="font-bold text-stone-400">Gov Planning</span>
                </div>
                <div class="flex items-center justify-center gap-2">
                    <span class="text-2xl">💡</span> <span class="font-bold text-stone-400">EcoEnergy</span>
                </div>
                <div class="flex items-center justify-center gap-2">
                    <span class="text-2xl">🎓</span> <span class="font-bold text-stone-400">Uni Research</span>
                </div>
            </div>
            <div class="text-center mt-12 text-stone-400 text-sm">
                <p>&copy; 2024 Mycalla Inc. | IslaConectada Initiative</p>
            </div>
        </section>

    </main>

    <script>
        // --- Data Definitions ---
        const techData = {
            '5gnr': {
                title: "5G New Radio (NR)",
                icon: "📶",
                subtitle: "The Global Wireless Standard",
                desc: "5G NR is the global standard for a unified 5G wireless air interface. It delivers significantly accelerated data transmission rates—projected to exceed gigabit-per-second thresholds—using advanced modulation coding schemes."
            },
            'edge': {
                title: "Edge Computing",
                icon: "⚡",
                subtitle: "Processing at the Source",
                desc: "By moving computation away from centralized clouds to the network edge, we minimize latency to near-zero levels. This is critical for mission-critical applications like autonomous navigation and industrial automation."
            },
            'slicing': {
                title: "Network Slicing",
                icon: "🍰",
                subtitle: "Virtual Partitioning",
                desc: "Allows us to create multiple isolated, virtualized networks on shared infrastructure. One slice supports emergency services with guaranteed uptime, while another handles high-bandwidth consumer streaming."
            },
            'cells': {
                title: "Small Cell Densification",
                icon: "🏙️",
                subtitle: "Capacity & Spatial Reuse",
                desc: "Low-profile nodes deployed in dense urban zones. By reducing physical distance between transmitter and receiver, we sustain high speeds during peak congestion and enable aggressive frequency reuse."
            }
        };

        const pilotData = {
            'traffic': {
                title: "Intelligent Traffic Flow",
                desc: "Sensor-based adaptive signal controls designed to reduce urban congestion.",
                val: 35,
                color: '#4338ca' // Indigo
            },
            'environment': {
                title: "Eco Monitoring",
                desc: "IoT networks for real-time tracking of air quality and water levels.",
                val: 60,
                color: '#059669' // Emerald
            },
            'energy': {
                title: "Smart Grid Energy",
                desc: "Responsive street illumination to optimize municipal energy consumption.",
                val: 45,
                color: '#d97706' // Amber
            }
        };

        // --- Core Functions ---

        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        // Tech View Logic
        function updateTechView(key) {
            const data = techData[key];
            const contentDiv = document.getElementById('tech-content');
            const bgIcon = document.getElementById('bg-icon');
            
            // Fade Out
            contentDiv.style.opacity = '0';
            contentDiv.style.transform = 'translateY(10px)';

            setTimeout(() => {
                // Update Content
                contentDiv.innerHTML = `
                    <div class="flex items-start justify-between mb-4">
                        <div>
                            <span class="text-xs font-bold text-indigo-500 uppercase tracking-widest">${data.subtitle}</span>
                            <h3 class="text-3xl font-bold text-indigo-900 mt-1">${data.title}</h3>
                        </div>
                        <span class="text-4xl shadow-sm bg-white p-3 rounded-full">${data.icon}</span>
                    </div>
                    <p class="text-stone-600 text-lg leading-relaxed border-t border-stone-200 pt-4">
                        ${data.desc}
                    </p>
                    <div class="mt-6">
                        <button class="text-sm font-bold text-indigo-700 hover:text-indigo-900 flex items-center gap-1 group">
                            Technical Documentation 
                            <span class="transition-transform group-hover:translate-x-1">→</span>
                        </button>
                    </div>
                `;
                bgIcon.innerHTML = data.icon;
                
                // Fade In
                contentDiv.style.opacity = '1';
                contentDiv.style.transform = 'translateY(0)';
            }, 200);

            // Update Buttons Styling
            document.querySelectorAll('.tech-btn').forEach(btn => {
                btn.classList.remove('active-tab');
                btn.classList.add('border-white');
                btn.querySelector('span.text-indigo-600').classList.remove('opacity-100');
                btn.querySelector('span.text-indigo-600').classList.add('opacity-0');
            });
            const activeBtn = document.getElementById(`btn-${key}`);
            activeBtn.classList.add('active-tab');
            activeBtn.classList.remove('border-white');
            activeBtn.querySelector('span.text-indigo-600').classList.add('opacity-100');
            activeBtn.querySelector('span.text-indigo-600').classList.remove('opacity-0');
        }

        // Pilot View Logic
        let pilotChartInstance = null;

        function updatePilot(key) {
            const data = pilotData[key];
            
            // Text Update
            document.getElementById('pilot-content').innerHTML = `
                <div class="flex justify-between items-end mb-2">
                    <h4 class="font-bold text-stone-800 text-lg">${data.title}</h4>
                    <span class="text-2xl font-bold" style="color:${data.color}">${data.val}%</span>
                </div>
                <p class="text-sm text-stone-500 h-10">${data.desc}</p>
            `;

            // Tab Styling
            const tabs = ['traffic', 'environment', 'energy'];
            tabs.forEach(t => {
                const el = document.getElementById(`tab-${t}`);
                if(t === key) {
                    el.className = "flex-1 py-2 text-xs font-bold rounded-lg transition-all text-white shadow-sm transform scale-105";
                    el.style.backgroundColor = data.color;
                } else {
                    el.className = "flex-1 py-2 text-xs font-bold rounded-lg text-stone-500 hover:text-stone-700 transition-all hover:bg-stone-200";
                    el.style.backgroundColor = 'transparent';
                }
            });

            // Chart Update
            if(pilotChartInstance) {
                pilotChartInstance.data.datasets[0].data = [data.val, 100 - data.val];
                pilotChartInstance.data.datasets[0].backgroundColor = [data.color, '#e5e7eb'];
                pilotChartInstance.update();
            }
        }

        // --- Chart Initialization ---

        function initCharts() {
            // Gradient Setup for Speed Chart
            const ctxSpeed = document.getElementById('speedChart').getContext('2d');
            const gradientSpeed = ctxSpeed.createLinearGradient(0, 0, 0, 400);
            gradientSpeed.addColorStop(0, '#312e81'); // indigo-900
            gradientSpeed.addColorStop(1, '#6366f1'); // indigo-500

            new Chart(ctxSpeed, {
                type: 'bar',
                data: {
                    labels: ['4G LTE', 'IslaConectada 5G'],
                    datasets: [{
                        label: 'Mbps',
                        data: [50, 500],
                        backgroundColor: ['#d6d3d1', gradientSpeed],
                        borderRadius: 10,
                        barThickness: 50
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false },
                        tooltip: { backgroundColor: '#1e1b4b', padding: 12, cornerRadius: 8 }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            grid: { display: true, color: '#f5f5f4', drawBorder: false },
                            ticks: { font: { family: 'Lato' } }
                        },
                        x: {
                            grid: { display: false, drawBorder: false },
                            ticks: { font: { family: 'Lato', weight: '700' } }
                        }
                    },
                    animation: {
                        duration: 2000,
                        easing: 'easeOutQuart'
                    }
                }
            });

            // Pilot Doughnut Chart
            const ctxPilot = document.getElementById('pilotChart').getContext('2d');
            pilotChartInstance = new Chart(ctxPilot, {
                type: 'doughnut',
                data: {
                    labels: ['Gain', 'Potential'],
                    datasets: [{
                        data: [35, 65],
                        backgroundColor: ['#4338ca', '#e5e7eb'],
                        borderWidth: 0,
                        hoverOffset: 4
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '75%',
                    plugins: {
                        legend: { display: false },
                        tooltip: { enabled: false }
                    },
                    animation: { animateScale: true, animateRotate: true }
                }
            });
        }

        // --- Scroll & Count Animations ---

        function animateValue(obj, start, end, duration) {
            let startTimestamp = null;
            const step = (timestamp) => {
                if (!startTimestamp) startTimestamp = timestamp;
                const progress = Math.min((timestamp - startTimestamp) / duration, 1);
                obj.innerHTML = Math.floor(progress * (end - start) + start);
                if (progress < 1) {
                    window.requestAnimationFrame(step);
                }
            };
            window.requestAnimationFrame(step);
        }

        const observerOptions = { threshold: 0.1 };
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('active');
                    
                    // Trigger counters if inside this section
                    const counters = entry.target.querySelectorAll('.counter');
                    counters.forEach(counter => {
                        const target = +counter.getAttribute('data-target');
                        animateValue(counter, 0, target, 2000);
                        counter.classList.remove('counter'); // Ensure it runs only once
                    });
                }
            });
        }, observerOptions);

        document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

        // --- Init ---
        document.addEventListener('DOMContentLoaded', () => {
            initCharts();
            updateTechView('5gnr');
            updatePilot('traffic');
        });

    </script>
</body>
</html>
