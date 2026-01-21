<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KHAI POWER - ยืนยันการเช่า</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600&family=Orbitron:wght@500;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Kanit', 'sans-serif'],
                        mono: ['Orbitron', 'monospace'],
                    },
                    colors: {
                        cyber: {
                            dark: '#0f172a',
                            panel: '#1e293b',
                            accent: '#06b6d4', // Cyan
                            glow: '#22d3ee',
                            danger: '#ef4444',
                            purple: '#a855f7',
                            success: '#10b981',
                            warning: '#eab308'
                        }
                    },
                    animation: {
                        'pulse-slow': 'pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #0b1120;
            background-image: 
                radial-gradient(circle at 50% 0%, #1e293b 0%, transparent 70%),
                linear-gradient(rgba(6, 182, 212, 0.05) 1px, transparent 1px),
                linear-gradient(90deg, rgba(6, 182, 212, 0.05) 1px, transparent 1px);
            background-size: 100% 100%, 40px 40px, 40px 40px;
        }
        .neon-text { text-shadow: 0 0 10px rgba(6, 182, 212, 0.7); }
        .neon-border { box-shadow: 0 0 15px rgba(6, 182, 212, 0.2); }
        .glass-panel {
            background: rgba(30, 41, 59, 0.9);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(6, 182, 212, 0.3);
        }
        /* Range Slider Styling */
        input[type=range] {
            -webkit-appearance: none;
            background: transparent;
        }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 20px;
            width: 20px;
            border-radius: 50%;
            background: #22d3ee;
            cursor: pointer;
            margin-top: -8px;
            box-shadow: 0 0 10px #22d3ee;
            position: relative;
            z-index: 20;
        }
        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 4px;
            cursor: pointer;
            background: #334155;
            border-radius: 2px;
        }
        /* Green thumb for target */
        .target-slider::-webkit-slider-thumb {
            background: #10b981;
            box-shadow: 0 0 10px #10b981;
        }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
    </style>
</head>
<body class="text-slate-200 min-h-screen flex flex-col">

    <!-- Header -->
    <header class="border-b border-cyan-900/50 bg-slate-900/80 backdrop-blur-md sticky top-0 z-50">
        <div class="container mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-3 h-3 bg-cyan-400 rounded-full animate-pulse shadow-[0_0_10px_#22d3ee]"></div>
                <h1 class="font-mono text-2xl font-bold tracking-wider text-white">
                    KHAI <span class="text-cyan-400">POWER</span>
                </h1>
            </div>
            <div class="font-mono text-xs text-cyan-500 border border-cyan-800 px-3 py-1 rounded bg-cyan-950/30">
                SYSTEM: ONLINE
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8 flex-grow relative z-10">
        
        <div class="text-center mb-10">
            <h2 class="text-3xl font-bold mb-2 text-transparent bg-clip-text bg-gradient-to-r from-cyan-300 to-blue-500 neon-text">
                เลือกหน่วยพลังงาน
            </h2>
            <p class="text-slate-400 mb-6 font-light">ค้นหารุ่นมือถือของคุณเพื่อเริ่มใช้งาน</p>
            
            <div class="relative max-w-xl mx-auto group">
                <input type="text" id="searchInput" 
                    placeholder="ค้นหารุ่น (เช่น iPhone, Samsung, Vivo, Honor)..." 
                    class="w-full bg-slate-800/50 border border-cyan-800/50 text-cyan-100 pl-12 pr-4 py-3 rounded-lg focus:outline-none focus:border-cyan-500 focus:shadow-[0_0_15px_rgba(6,182,212,0.3)] transition-all font-mono placeholder-slate-600">
                <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none">
                    <i class="fas fa-search text-cyan-600"></i>
                </div>
            </div>
        </div>

        <div id="phoneGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Grid Items -->
        </div>

        <div id="noResults" class="hidden flex flex-col items-center justify-center py-20 opacity-50">
            <i class="fas fa-satellite-dish text-6xl text-slate-700 mb-4"></i>
            <p class="font-mono text-slate-500">DATA NOT FOUND</p>
        </div>

    </main>

    <!-- Booking Modal -->
    <div id="bookingModal" class="fixed inset-0 z-[100] hidden">
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-slate-950/90 backdrop-blur-sm transition-opacity" onclick="closeModal()"></div>
        
        <!-- Modal Content -->
        <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-full max-w-lg px-4">
            <div class="glass-panel bg-slate-900 border-cyan-500/50 rounded-2xl shadow-[0_0_50px_rgba(6,182,212,0.2)] overflow-hidden relative max-h-[90vh] overflow-y-auto">
                
                <!-- Header -->
                <div class="p-6 border-b border-cyan-900/50 relative z-10 bg-gradient-to-r from-slate-900 to-slate-800">
                    <div class="flex justify-between items-start">
                        <div>
                            <span class="text-xs font-mono text-cyan-500 mb-1 block">SMART CALCULATION</span>
                            <h3 id="modalModelName" class="text-2xl font-bold text-white tracking-wide">Model Name</h3>
                            <div class="flex items-center gap-2 mt-1">
                                <span id="modalBattery" class="text-sm text-slate-400 font-mono">Capacity</span>
                                <span class="text-slate-600">|</span>
                                <span id="modalSpeed" class="text-sm text-yellow-400 font-mono">Max 45W</span>
                            </div>
                        </div>
                        <button onclick="closeModal()" class="text-slate-500 hover:text-white transition-colors">
                            <i class="fas fa-times text-xl"></i>
                        </button>
                    </div>
                </div>

                <!-- Body -->
                <div class="p-6 space-y-6 relative z-10">
                    
                    <!-- 1. Battery Percentage Inputs -->
                    <div class="bg-slate-800/50 p-4 rounded-xl border border-slate-700">
                        <label class="block text-sm text-slate-300 mb-4 font-mono">
                            <i class="fas fa-battery-half mr-2 text-cyan-500"></i>BATTERY LEVEL
                        </label>
                        
                        <!-- Start Level -->
                        <div class="mb-4">
                            <div class="flex justify-between text-xs mb-1">
                                <span class="text-cyan-400">Current (มีอยู่)</span>
                                <span class="font-mono text-white font-bold"><span id="startPctDisplay">10</span>%</span>
                            </div>
                            <input type="range" id="startPctInput" min="0" max="95" value="10" class="w-full" oninput="updateCalculation()">
                        </div>

                        <!-- Target Level -->
                        <div>
                            <div class="flex justify-between text-xs mb-1">
                                <span class="text-green-400">Target (เป้าหมาย)</span>
                                <span class="font-mono text-white font-bold"><span id="targetPctDisplay">100</span>%</span>
                            </div>
                            <input type="range" id="targetPctInput" min="5" max="100" value="100" class="w-full target-slider" oninput="updateCalculation()">
                        </div>
                    </div>

                    <!-- 2. Cable Selection -->
                    <div>
                        <label class="block text-sm text-slate-300 mb-3 font-mono">
                            <span><i class="fas fa-plug mr-2 text-yellow-500"></i>CABLE TYPE</span>
                        </label>
                        <div class="flex gap-3">
                            <label class="cursor-pointer flex-1">
                                <input type="radio" name="cable" value="standard" class="peer hidden" checked onchange="updateCalculation()">
                                <div class="bg-slate-800 border border-slate-700 rounded-xl p-3 text-center hover:border-yellow-500 peer-checked:border-yellow-500 peer-checked:bg-yellow-900/20 peer-checked:text-yellow-300 transition-all h-full">
                                    <i class="fas fa-charging-station text-xl mb-1 block"></i>
                                    <span class="text-xs font-bold">สายตู้ (ฟรี)</span>
                                    <div class="text-[9px] text-yellow-400 mt-1">Limit 22.5W</div>
                                </div>
                            </label>
                            <label class="cursor-pointer flex-1">
                                <input type="radio" name="cable" value="own" class="peer hidden" onchange="updateCalculation()">
                                <div class="bg-slate-800 border border-slate-700 rounded-xl p-3 text-center hover:border-cyan-500 peer-checked:border-cyan-500 peer-checked:bg-cyan-900/20 peer-checked:text-cyan-300 transition-all h-full">
                                    <i class="fas fa-bolt text-xl mb-1 block"></i>
                                    <span class="text-xs font-bold">สายตัวเอง</span>
                                    <div class="text-[9px] text-cyan-400 mt-1">Max Speed</div>
                                </div>
                            </label>
                        </div>
                    </div>

                    <!-- 3. Select Activity -->
                    <div>
                        <label class="block text-sm text-slate-300 mb-3 font-mono">
                            <span><i class="fas fa-gamepad mr-2 text-purple-500"></i>CURRENT ACTIVITY</span>
                        </label>
                        <div class="grid grid-cols-3 gap-3">
                            <label class="cursor-pointer">
                                <input type="radio" name="activity" value="game" class="peer hidden" onchange="updateCalculation()">
                                <div class="bg-slate-800 border border-slate-700 rounded-xl p-3 text-center hover:border-purple-500 peer-checked:border-purple-500 peer-checked:bg-purple-900/20 peer-checked:text-purple-300 transition-all h-full relative overflow-hidden group">
                                    <i class="fas fa-gamepad text-xl mb-1 block"></i>
                                    <span class="text-xs font-bold">เล่นเกม</span>
                                    <div class="text-[9px] text-purple-400 mt-1">ช้าลง 40%</div>
                                </div>
                            </label>
                            <label class="cursor-pointer">
                                <input type="radio" name="activity" value="media" class="peer hidden" onchange="updateCalculation()">
                                <div class="bg-slate-800 border border-slate-700 rounded-xl p-3 text-center hover:border-blue-500 peer-checked:border-blue-500 peer-checked:bg-blue-900/20 peer-checked:text-blue-300 transition-all h-full relative overflow-hidden group">
                                    <i class="fas fa-play-circle text-xl mb-1 block"></i>
                                    <span class="text-xs font-bold">ดูหนัง/YT</span>
                                    <div class="text-[9px] text-blue-400 mt-1">ช้าลง 20%</div>
                                </div>
                            </label>
                            <label class="cursor-pointer">
                                <input type="radio" name="activity" value="idle" class="peer hidden" checked onchange="updateCalculation()">
                                <div class="bg-slate-800 border border-slate-700 rounded-xl p-3 text-center hover:border-green-500 peer-checked:border-green-500 peer-checked:bg-green-900/20 peer-checked:text-green-300 transition-all h-full relative overflow-hidden group">
                                    <i class="fas fa-power-off text-xl mb-1 block"></i>
                                    <span class="text-xs font-bold">วางเฉยๆ</span>
                                    <div class="text-[9px] text-green-400 mt-1">ไวสุด</div>
                                </div>
                            </label>
                        </div>
                    </div>
                    
                    <!-- Calculation Result Bar -->
                    <div id="timeResult" class="bg-slate-900 p-4 rounded-lg border border-cyan-900/50 flex justify-between items-center">
                        <div>
                            <div class="text-xs text-slate-500">EST. CHARGING TIME</div>
                            <div class="text-lg text-cyan-300 font-mono font-bold"><i class="fas fa-hourglass-half mr-2"></i><span id="timeDisplay">0</span> Min</div>
                        </div>
                        <div class="text-right">
                            <div class="text-xs text-slate-500">BILLED HOURS</div>
                            <div class="text-lg text-white font-mono font-bold"><span id="billedHours">1</span> HR</div>
                        </div>
                    </div>

                </div>

                <!-- Footer / Total -->
                <div class="p-6 bg-slate-900/80 border-t border-cyan-900/50 flex items-center justify-between">
                    <div>
                        <div class="text-xs text-slate-500 font-mono">TOTAL PRICE</div>
                        <div class="text-3xl font-bold text-white font-mono">฿<span id="totalPrice">--</span></div>
                    </div>
                    <button onclick="alert('ยืนยันการทำรายการ... ระบบกำลังปลดล็อคพาวเวอร์แบงค์')" class="bg-cyan-600 hover:bg-cyan-500 text-white px-6 py-3 rounded-lg font-bold shadow-[0_0_15px_rgba(6,182,212,0.4)] transition-all hover:scale-105 active:scale-95 flex items-center gap-2">
                        CONFIRM <i class="fas fa-chevron-right text-xs"></i>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Configuration
        const hourlyBasePrice = 14; 
        const mahHourlyFactor = 0.0005; 
        let currentPhoneIndex = null;

        // Expanded Database with Charging Speed (Watts)
        // รวมทุกยี่ห้อ (Apple, Samsung, Xiaomi, Vivo, Oppo, Realme, Huawei, Honor, Sony, Moto, Google, Tecno, Infinix, Nothing, etc.)
        const phones = [
            // Apple
            { brand: 'Apple', model: 'iPhone 16 Pro Max', battery: 4685, cable: 'USB-C', watts: 27 },
            { brand: 'Apple', model: 'iPhone 15 Series', battery: 3349, cable: 'USB-C', watts: 20 },
            { brand: 'Apple', model: 'iPhone 14/13 Series', battery: 3279, cable: 'Lightning', watts: 20 },
            
            // Samsung
            { brand: 'Samsung', model: 'Galaxy S24 Ultra', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Samsung', model: 'Galaxy S Series (Gen)', battery: 4000, cable: 'USB-C', watts: 25 },
            { brand: 'Samsung', model: 'Galaxy A Series', battery: 5000, cable: 'USB-C', watts: 25 },
            { brand: 'Samsung', model: 'Galaxy Z Fold/Flip', battery: 4400, cable: 'USB-C', watts: 25 },

            // Google
            { brand: 'Google', model: 'Pixel 9 Pro XL', battery: 5060, cable: 'USB-C', watts: 37 },
            { brand: 'Google', model: 'Pixel 8/7 Series', battery: 4575, cable: 'USB-C', watts: 27 },

            // Xiaomi / Redmi / POCO
            { brand: 'Xiaomi', model: 'Xiaomi 14 Ultra', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Xiaomi', model: 'Redmi Note 13 Pro', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Poco', model: 'POCO F6/X6 Pro', battery: 5000, cable: 'USB-C', watts: 45 },

            // Vivo / iQOO
            { brand: 'Vivo', model: 'Vivo X100 Pro', battery: 5400, cable: 'USB-C', watts: 45 },
            { brand: 'Vivo', model: 'Vivo V30/Y Series', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'iQOO', model: 'iQOO 12', battery: 5000, cable: 'USB-C', watts: 45 },

            // OPPO / OnePlus
            { brand: 'OPPO', model: 'Find X7 Ultra', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'OPPO', model: 'Reno 11 Series', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'OnePlus', model: 'OnePlus 12', battery: 5400, cable: 'USB-C', watts: 45 },

            // Realme
            { brand: 'Realme', model: 'Realme 12 Pro+', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Realme', model: 'Realme GT 5', battery: 5240, cable: 'USB-C', watts: 45 },

            // Huawei / Honor
            { brand: 'Huawei', model: 'Pura 70 Ultra', battery: 5200, cable: 'USB-C', watts: 45 },
            { brand: 'Huawei', model: 'Mate 60 Pro', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Honor', model: 'Magic 6 Pro', battery: 5600, cable: 'USB-C', watts: 45 },
            { brand: 'Honor', model: 'Honor 90/X9b', battery: 5000, cable: 'USB-C', watts: 35 },

            // Sony
            { brand: 'Sony', model: 'Xperia 1 VI', battery: 5000, cable: 'USB-C', watts: 30 },
            { brand: 'Sony', model: 'Xperia 10 V', battery: 5000, cable: 'USB-C', watts: 21 },

            // Motorola
            { brand: 'Motorola', model: 'Edge 50 Pro', battery: 4500, cable: 'USB-C', watts: 45 },
            { brand: 'Motorola', model: 'Razr 40 Ultra', battery: 3800, cable: 'USB-C', watts: 30 },

            // Infinix / Tecno
            { brand: 'Infinix', model: 'GT 20 Pro', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Infinix', model: 'Note 40 Pro', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Tecno', model: 'Camon 30 Premier', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'Tecno', model: 'Pova 6 Pro', battery: 6000, cable: 'USB-C', watts: 45 },

            // Others
            { brand: 'Asus', model: 'ROG Phone 8', battery: 5500, cable: 'USB-C', watts: 45 },
            { brand: 'Nothing', model: 'Phone (2a)', battery: 5000, cable: 'USB-C', watts: 45 },
            { brand: 'ZTE', model: 'Nubia RedMagic 9', battery: 6500, cable: 'USB-C', watts: 45 },
            { brand: 'Tablet', model: 'iPad Air/Pro (USB-C)', battery: 8000, cable: 'USB-C', watts: 30 }
        ];

        function calculateBaseRate(batteryCap) {
            return Math.floor(hourlyBasePrice + (batteryCap * mahHourlyFactor));
        }

        // Render Cards
        function renderCards(filter = '') {
            const grid = document.getElementById('phoneGrid');
            const noRes = document.getElementById('noResults');
            grid.innerHTML = '';

            const filtered = phones.filter(p => 
                p.model.toLowerCase().includes(filter.toLowerCase()) || 
                p.brand.toLowerCase().includes(filter.toLowerCase())
            );

            if (filtered.length === 0) {
                noRes.classList.remove('hidden'); return;
            }
            noRes.classList.add('hidden');

            filtered.forEach((phone, index) => {
                const basePrice = calculateBaseRate(phone.battery);
                const originalIndex = phones.indexOf(phone);
                
                const card = document.createElement('div');
                card.className = 'glass-panel rounded-xl p-6 relative overflow-hidden group hover:border-cyan-400/50 transition-all duration-300 hover:shadow-[0_0_20px_rgba(6,182,212,0.15)]';
                
                card.innerHTML = `
                    <div class="absolute top-0 right-0 w-16 h-16 border-t-2 border-r-2 border-cyan-500/20 rounded-tr-xl group-hover:border-cyan-400/60 transition-colors"></div>
                    <div class="absolute bottom-0 left-0 w-8 h-8 border-b-2 border-l-2 border-cyan-500/20 rounded-bl-xl"></div>

                    <div class="flex justify-between items-start mb-4 relative z-10">
                        <div>
                            <span class="text-[10px] font-mono text-cyan-500 border border-cyan-900 px-1 rounded bg-cyan-950">${phone.brand.toUpperCase()}</span>
                            <h3 class="text-xl font-bold text-white mt-1 tracking-wide">${phone.model}</h3>
                        </div>
                        <div class="text-right">
                            <div class="text-xl font-mono font-bold text-cyan-400 neon-text">Start ${basePrice}฿</div>
                            <div class="text-[10px] text-slate-400 font-mono">PER HOUR</div>
                        </div>
                    </div>

                    <div class="space-y-3 mb-6 relative z-10">
                        <div class="flex items-center justify-between text-sm border-b border-slate-700/50 pb-2">
                            <span class="text-slate-400 font-mono text-xs">BATTERY</span>
                            <span class="text-slate-200 font-mono">${phone.battery.toLocaleString()} <span class="text-xs text-slate-500">mAh</span></span>
                        </div>
                        <div class="flex items-center justify-between text-sm border-b border-slate-700/50 pb-2">
                            <span class="text-slate-400 font-mono text-xs">SPEED</span>
                            <span class="text-yellow-400 text-xs font-mono font-bold">Max ${phone.watts}W</span>
                        </div>
                    </div>

                    <button onclick="openModal(${originalIndex})" class="w-full bg-cyan-900/40 hover:bg-cyan-600 text-cyan-400 hover:text-white border border-cyan-700 hover:border-cyan-400 py-3 rounded font-mono text-sm tracking-wider transition-all duration-300 flex items-center justify-center gap-2 group-hover:neon-border">
                        <i class="fas fa-calculator"></i> คำนวณค่าเช่า
                    </button>
                `;
                grid.appendChild(card);
            });
        }

        // --- Modal & Calculation Logic ---
        function openModal(index) {
            currentPhoneIndex = index;
            const phone = phones[index];

            document.getElementById('modalModelName').innerText = phone.model;
            document.getElementById('modalBattery').innerText = phone.battery.toLocaleString() + ' mAh';
            document.getElementById('modalSpeed').innerText = 'Max ' + phone.watts + 'W';
            
            // Reset Inputs
            document.getElementById('startPctInput').value = 10;
            document.getElementById('targetPctInput').value = 100;
            document.querySelector('input[name="cable"][value="standard"]').checked = true; // Default to Standard
            document.querySelector('input[name="activity"][value="idle"]').checked = true;
            
            updateCalculation();
            document.getElementById('bookingModal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('bookingModal').classList.add('hidden');
        }

        function updateCalculation() {
            if (currentPhoneIndex === null) return;
            
            const phone = phones[currentPhoneIndex];
            const startPct = parseInt(document.getElementById('startPctInput').value);
            let targetPct = parseInt(document.getElementById('targetPctInput').value);
            const activity = document.querySelector('input[name="activity"]:checked').value;
            const cableType = document.querySelector('input[name="cable"]:checked').value;

            // Ensure Target > Start
            if (targetPct <= startPct) {
                targetPct = startPct + 1;
                document.getElementById('targetPctInput').value = targetPct;
            }
            
            // Update UI Displays
            document.getElementById('startPctDisplay').innerText = startPct;
            document.getElementById('targetPctDisplay').innerText = targetPct;

            // --- 1. Calculate Needed Energy ---
            const neededPct = targetPct - startPct;
            const neededWh = ((phone.battery * 3.7) / 1000) * (neededPct / 100);

            // --- 2. Determine Effective Charging Speed ---
            // Base Limit: Phone capability OR PB capability (45W)
            let effectiveWatts = Math.min(phone.watts, 45); 

            // ** Cable Limitation **
            if (cableType === 'standard') {
                // If using kiosk cable, limit to 22.5W
                effectiveWatts = Math.min(effectiveWatts, 22.5);
            }
            // If 'own', no additional limit (up to 45W)

            // Adjust speed based on Activity (using energy while charging)
            let rateModifier = 0; 
            
            if (activity === 'game') {
                effectiveWatts *= 0.6; 
                rateModifier = 10;
            } else if (activity === 'media') {
                effectiveWatts *= 0.8; 
                rateModifier = 5;
            } else {
                effectiveWatts *= 1.0; 
                rateModifier = 0;
            }

            // --- 3. Calculate Time ---
            // Time (Hours) = Energy (Wh) / Power (W)
            // Add 15% inefficiency/overhead
            const timeHours = (neededWh / (effectiveWatts * 0.85)); 
            const timeMinutes = Math.ceil(timeHours * 60);

            // --- 4. Calculate Price (Rounding UP hours) ---
            const baseRatePerHour = calculateBaseRate(phone.battery) + rateModifier;
            const billedHours = Math.ceil(timeMinutes / 60); 
            
            // Minimum billing 1 hour
            const finalBilledHours = billedHours < 1 ? 1 : billedHours;
            const totalPrice = finalBilledHours * baseRatePerHour;

            // Update UI Result
            document.getElementById('timeDisplay').innerText = timeMinutes;
            document.getElementById('billedHours').innerText = finalBilledHours;
            document.getElementById('totalPrice').innerText = totalPrice;
        }

        // Event Listeners
        document.getElementById('searchInput').addEventListener('input', (e) => renderCards(e.target.value));
        
        // Init
        renderCards();

    </script>
</body>
</html>
