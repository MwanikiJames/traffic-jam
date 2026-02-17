<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traffic Jam - Smart Traffic Monitoring</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        body { font-family: 'Inter', sans-serif; }
        .map-container { height: 100%; width: 100%; }
        .camera-feed { aspect-ratio: 16/9; background: #1a1a1a; }
        .glass-panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        .dark-glass {
            background: rgba(0, 0, 0, 0.85);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        .pulse-dot {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        .video-card:hover .delete-btn { opacity: 1; }
        .hidden { display: none !important; }
        
        /* Fix for Leaflet map container */
        #map {
            min-height: 100%;
            min-width: 100%;
        }
        
        /* Custom scrollbar for webkit */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1f2937; 
        }
        ::-webkit-scrollbar-thumb {
            background: #4b5563; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #6b7280; 
        }
    </style>
</head>
<body class="bg-gray-900 text-gray-100 overflow-hidden">

    <!-- Login Section -->
    <div id="loginSection" class="fixed inset-0 z-50 flex items-center justify-center bg-gradient-to-br from-gray-900 via-blue-900 to-gray-900">
        <div class="glass-panel rounded-2xl p-8 w-full max-w-md mx-4 shadow-2xl">
            <div class="text-center mb-8">
                <div class="w-16 h-16 bg-blue-600 rounded-full flex items-center justify-center mx-auto mb-4">
                    <i data-lucide="traffic-cone" class="w-8 h-8 text-white"></i>
                </div>
                <h1 class="text-3xl font-bold text-gray-900">Traffic Jam</h1>
                <p class="text-gray-600 mt-2">Smart Traffic Monitoring System</p>
            </div>
            
            <form id="loginForm" class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                    <input type="email" id="emailInput" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none text-gray-900" placeholder="driver@example.com" required>
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                    <input type="password" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none text-gray-900" placeholder="••••••••" required>
                </div>
                <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg transition duration-200 flex items-center justify-center gap-2">
                    <i data-lucide="log-in" class="w-5 h-5"></i>
                    Sign In
                </button>
            </form>
            
            <p class="text-center text-sm text-gray-500 mt-6">
                Don't have an account? <button onclick="showSubscription()" class="text-blue-600 font-semibold hover:underline">Subscribe Now</button>
            </p>
        </div>
    </div>

    <!-- Subscription Section -->
    <div id="subscriptionSection" class="fixed inset-0 z-50 hidden overflow-y-auto bg-gray-900">
        <div class="min-h-screen p-4 flex flex-col items-center justify-center relative">
            <!-- Back Button -->
            <button onclick="backToLogin()" class="absolute top-4 left-4 flex items-center gap-2 text-gray-400 hover:text-white transition bg-gray-800 hover:bg-gray-700 px-4 py-2 rounded-lg z-50">
                <i data-lucide="arrow-left" class="w-5 h-5"></i>
                Back to Login
            </button>
            <div class="text-center mb-8">
                <h2 class="text-3xl font-bold text-white mb-2">Choose Your Plan</h2>
                <p class="text-gray-400">Select a subscription to access Traffic Jam features</p>
            </div>
            
            <div class="grid md:grid-cols-3 gap-6 max-w-6xl w-full px-4">
                <!-- Normal Plan -->
                <div class="glass-panel rounded-2xl p-6 flex flex-col transform hover:scale-105 transition duration-300">
                    <div class="text-center mb-6">
                        <div class="w-12 h-12 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-3">
                            <i data-lucide="map" class="w-6 h-6 text-green-600"></i>
                        </div>
                        <h3 class="text-xl font-bold text-gray-900">Normal</h3>
                        <div class="text-3xl font-bold text-gray-900 mt-2">100<span class="text-lg text-gray-600">/month</span></div>
                    </div>
                    <ul class="space-y-3 mb-6 flex-1 text-gray-700">
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-green-500"></i> Real-time traffic updates</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-green-500"></i> Road condition reports</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-green-500"></i> Interactive maps</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-green-500"></i> 1 device access</li>
                    </ul>
                    <button onclick="selectPlan('normal')" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-3 rounded-lg transition">
                        Subscribe Now
                    </button>
                </div>

                <!-- Premium Plan -->
                <div class="glass-panel rounded-2xl p-6 flex flex-col transform hover:scale-105 transition duration-300 ring-2 ring-blue-500 relative">
                    <div class="absolute -top-3 left-1/2 transform -translate-x-1/2 bg-blue-500 text-white px-3 py-1 rounded-full text-xs font-bold">
                        POPULAR
                    </div>
                    <div class="text-center mb-6">
                        <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-3">
                            <i data-lucide="video" class="w-6 h-6 text-blue-600"></i>
                        </div>
                        <h3 class="text-xl font-bold text-gray-900">Premium</h3>
                        <div class="text-3xl font-bold text-gray-900 mt-2">500<span class="text-lg text-gray-600">/month</span></div>
                    </div>
                    <ul class="space-y-3 mb-6 flex-1 text-gray-700">
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-blue-500"></i> All Normal features</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-blue-500"></i> Live CCTV access</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-blue-500"></i> Auto camera switching</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-blue-500"></i> 1 device access</li>
                    </ul>
                    <button onclick="selectPlan('premium')" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg transition">
                        Subscribe Now
                    </button>
                </div>

                <!-- Platinum Plan -->
                <div class="glass-panel rounded-2xl p-6 flex flex-col transform hover:scale-105 transition duration-300 ring-2 ring-purple-500">
                    <div class="text-center mb-6">
                        <div class="w-12 h-12 bg-purple-100 rounded-full flex items-center justify-center mx-auto mb-3">
                            <i data-lucide="crown" class="w-6 h-6 text-purple-600"></i>
                        </div>
                        <h3 class="text-xl font-bold text-gray-900">Platinum</h3>
                        <div class="text-3xl font-bold text-gray-900 mt-2">1000<span class="text-lg text-gray-600">/month</span></div>
                    </div>
                    <ul class="space-y-3 mb-6 flex-1 text-gray-700">
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-purple-500"></i> All Premium features</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-purple-500"></i> Save videos</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-purple-500"></i> Request past videos</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5 text-purple-500"></i> Up to 5 devices</li>
                    </ul>
                    <button onclick="selectPlan('platinum')" class="w-full bg-purple-600 hover:bg-purple-700 text-white font-semibold py-3 rounded-lg transition">
                        Subscribe Now
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Search Interface (Post-Login) -->
    <div id="searchSection" class="fixed inset-0 z-40 hidden bg-gray-900 flex flex-col">
        <!-- Header with Logout -->
        <div class="p-4 flex items-center justify-between border-b border-gray-800 bg-gray-900">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center">
                    <i data-lucide="traffic-cone" class="w-4 h-4 text-white"></i>
                </div>
                <span class="font-bold text-white">Traffic Jam</span>
            </div>
            <button onclick="logoutFromSearch()" class="flex items-center gap-2 text-gray-400 hover:text-white transition bg-gray-800 hover:bg-gray-700 px-4 py-2 rounded-lg">
                <i data-lucide="log-out" class="w-4 h-4"></i>
                Logout
            </button>
        </div>
        
        <div class="flex-1 flex items-center justify-center p-4">
            <div class="w-full max-w-2xl">
                <div class="text-center mb-8">
                <h2 class="text-3xl font-bold text-white mb-2">Where are you heading?</h2>
                <p class="text-gray-400">Search for a road or highway to view traffic conditions</p>
            </div>
            
            <div class="relative mb-6">
                <i data-lucide="search" class="w-6 h-6 text-gray-400 absolute left-4 top-1/2 transform -translate-y-1/2"></i>
                <input type="text" id="roadSearchInput" class="w-full pl-12 pr-4 py-4 rounded-xl bg-gray-800 border border-gray-700 text-white placeholder-gray-400 focus:ring-2 focus:ring-blue-500 outline-none text-lg" placeholder="Search roads (e.g., Mombasa Road, Thika Road)...">
            </div>
            
            <div id="searchResults" class="space-y-2 max-h-96 overflow-y-auto">
                <!-- Search results populated by JS -->
            </div>
            
            <div class="mt-6 flex flex-wrap gap-2 justify-center">
                <button onclick="quickSearch('Mombasa Road')" class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-full text-sm text-gray-300 transition">Mombasa Road</button>
                <button onclick="quickSearch('Thika Road')" class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-full text-sm text-gray-300 transition">Thika Road</button>
                <button onclick="quickSearch('Waiyaki Way')" class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-full text-sm text-gray-300 transition">Waiyaki Way</button>
                <button onclick="quickSearch('Langata Road')" class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-full text-sm text-gray-300 transition">Langata Road</button>
            </div>
        </div>
    </div>

    <!-- Main App Interface -->
    <div id="mainApp" class="hidden h-screen w-full flex flex-col relative" style="display: none;">
        <!-- Header -->
        <header class="bg-gray-800 border-b border-gray-700 px-4 py-3 flex items-center justify-between z-30">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-blue-600 rounded-lg flex items-center justify-center">
                    <i data-lucide="traffic-cone" class="w-5 h-5 text-white"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg">Traffic Jam</h1>
                    <p id="currentRoadDisplay" class="text-xs text-gray-400">No road selected</p>
                </div>
            </div>
            
            <div class="flex items-center gap-3">
                <span id="subscriptionBadge" class="px-3 py-1 rounded-full text-xs font-semibold bg-gray-700 text-gray-300">Normal</span>
                <button onclick="showDriverReports()" class="p-2 hover:bg-gray-700 rounded-lg transition relative" title="Driver Reports">
                    <i data-lucide="alert-triangle" class="w-5 h-5 text-yellow-500"></i>
                    <span id="reportCount" class="absolute -top-1 -right-1 w-5 h-5 bg-red-500 rounded-full text-xs flex items-center justify-center">3</span>
                </button>
                <button onclick="showVideoLibrary()" id="libraryBtn" class="hidden p-2 hover:bg-gray-700 rounded-lg transition" title="Video Library">
                    <i data-lucide="film" class="w-5 h-5 text-purple-400"></i>
                </button>
                <button onclick="logout()" class="p-2 hover:bg-gray-700 rounded-lg transition">
                    <i data-lucide="log-out" class="w-5 h-5 text-gray-400"></i>
                </button>
            </div>
        </header>

        <!-- Map Container -->
        <div class="flex-1 relative w-full h-full" style="z-index: 1;">
            <div id="map" class="absolute inset-0 w-full h-full z-0"></div>
            
            <!-- Side Panel -->
            <div class="absolute top-4 left-4 z-[1000] w-80 max-h-[calc(100vh-6rem)] overflow-y-auto space-y-3">
                <!-- Traffic Status Card -->
                <div class="glass-panel rounded-xl p-4 shadow-lg">
                    <h3 class="font-semibold text-gray-900 mb-3 flex items-center gap-2">
                        <i data-lucide="activity" class="w-4 h-4 text-red-500"></i>
                        Live Traffic Status
                    </h3>
                    <div id="trafficStatus" class="space-y-2">
                        <div class="flex items-center justify-between text-sm">
                            <span class="text-gray-600">Flow Direction</span>
                            <span class="font-medium text-gray-900">Northbound</span>
                        </div>
                        <div class="flex items-center justify-between text-sm">
                            <span class="text-gray-600">Congestion</span>
                            <span class="font-medium text-red-600">Heavy (2.5km)</span>
                        </div>
                        <div class="flex items-center justify-between text-sm">
                            <span class="text-gray-600">Avg Speed</span>
                            <span class="font-medium text-gray-900">15 km/h</span>
                        </div>
                    </div>
                </div>

                <!-- Camera List (Premium/Platinum only) -->
                <div id="cameraListPanel" class="glass-panel rounded-xl p-4 shadow-lg hidden">
                    <h3 class="font-semibold text-gray-900 mb-3 flex items-center gap-2">
                        <i data-lucide="camera" class="w-4 h-4 text-blue-500"></i>
                        Cameras on Route
                    </h3>
                    <div id="cameraList" class="space-y-2 max-h-48 overflow-y-auto text-sm">
                        <!-- Populated by JS -->
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="glass-panel rounded-xl p-4 shadow-lg space-y-2">
                    <button onclick="startCameraMode()" id="cameraModeBtn" class="hidden w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg transition flex items-center justify-center gap-2">
                        <i data-lucide="video" class="w-5 h-5"></i>
                        Start Camera/Live Mode
                    </button>
                    <button onclick="showSearch()" class="w-full bg-gray-800 hover:bg-gray-700 text-white font-semibold py-3 rounded-lg transition flex items-center justify-center gap-2">
                        <i data-lucide="map-pin" class="w-5 h-5"></i>
                        Change Road
                    </button>
                    <button onclick="reportTraffic()" class="w-full bg-yellow-600 hover:bg-yellow-700 text-white font-semibold py-2 rounded-lg transition flex items-center justify-center gap-2 text-sm">
                        <i data-lucide="alert-circle" class="w-4 h-4"></i>
                        Report Traffic Jam
                    </button>
                </div>
            </div>

            <!-- Floating Legend -->
            <div class="absolute bottom-4 right-4 z-[1000] glass-panel rounded-lg p-3 text-xs shadow-lg">
                <div class="flex items-center gap-2 mb-1">
                    <div class="w-3 h-3 rounded-full bg-red-500"></div>
                    <span class="text-gray-700">Heavy Traffic</span>
                </div>
                <div class="flex items-center gap-2 mb-1">
                    <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
                    <span class="text-gray-700">Moderate</span>
                </div>
                <div class="flex items-center gap-2">
                    <div class="w-3 h-3 rounded-full bg-blue-500"></div>
                    <span class="text-gray-700">Camera</span>
                </div>
            </div>
        </div>
    </div>

    <!-- Live Camera Mode Modal -->
    <div id="cameraModeModal" class="fixed inset-0 z-50 hidden bg-black">
        <div class="h-full flex flex-col">
            <!-- Header -->
            <div class="bg-gray-900 px-4 py-3 flex items-center justify-between border-b border-gray-800">
                <div class="flex items-center gap-3">
                    <div class="w-2 h-2 bg-red-500 rounded-full pulse-dot"></div>
                    <span class="font-semibold text-white">LIVE</span>
                    <span id="liveCameraName" class="text-gray-400 text-sm">Cam-001</span>
                </div>
                <button onclick="exitCameraMode()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg font-medium flex items-center gap-2 transition">
                    <i data-lucide="x" class="w-4 h-4"></i>
                    Exit Live Mode
                </button>
            </div>

            <!-- Video Feed -->
            <div class="flex-1 relative bg-gray-900 flex items-center justify-center">
                <img id="liveFeedImage" src="" alt="Live Camera Feed" class="max-w-full max-h-full object-contain">
                
                <!-- Overlay Info -->
                <div class="absolute top-4 left-4 dark-glass rounded-lg p-3 text-white">
                    <div class="text-xs text-gray-400 mb-1">Distance Marker</div>
                    <div id="distanceMarker" class="font-bold text-lg">0m</div>
                </div>

                <div class="absolute top-4 right-4 dark-glass rounded-lg p-3 text-white text-right">
                    <div class="text-xs text-gray-400 mb-1">Next Camera</div>
                    <div id="nextCameraInfo" class="font-bold">100m ahead</div>
                </div>

                <!-- Auto-switch Progress -->
                <div class="absolute bottom-24 left-1/2 transform -translate-x-1/2 w-64">
                    <div class="text-center text-white text-sm mb-2">Auto-switching in <span id="countdown">10</span>s</div>
                    <div class="w-full bg-gray-700 rounded-full h-2">
                        <div id="progressBar" class="bg-blue-500 h-2 rounded-full transition-all duration-1000" style="width: 100%"></div>
                    </div>
                </div>
            </div>

            <!-- Controls -->
            <div class="bg-gray-900 border-t border-gray-800 p-4">
                <div class="max-w-4xl mx-auto flex items-center justify-between gap-4">
                    <button onclick="manualSwitchCamera(-1)" class="flex-1 bg-gray-800 hover:bg-gray-700 text-white py-3 rounded-lg font-medium transition flex items-center justify-center gap-2">
                        <i data-lucide="skip-back" class="w-5 h-5"></i>
                        Previous (-100m)
                    </button>
                    
                    <button onclick="toggleAutoSwitch()" id="autoSwitchBtn" class="flex-1 bg-green-600 hover:bg-green-700 text-white py-3 rounded-lg font-medium transition flex items-center justify-center gap-2">
                        <i data-lucide="pause" class="w-5 h-5"></i>
                        Pause Auto
                    </button>

                    <button onclick="manualSwitchCamera(2)" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-3 rounded-lg font-medium transition flex items-center justify-center gap-2">
                        <i data-lucide="skip-forward" class="w-5 h-5"></i>
                        Jump +200m
                    </button>

                    <button onclick="saveCurrentVideo()" id="saveVideoBtn" class="hidden flex-1 bg-purple-600 hover:bg-purple-700 text-white py-3 rounded-lg font-medium transition flex items-center justify-center gap-2">
                        <i data-lucide="download" class="w-5 h-5"></i>
                        Save Clip
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Video Library Modal -->
    <div id="videoLibraryModal" class="fixed inset-0 z-50 hidden bg-gray-900 overflow-y-auto">
        <div class="min-h-screen p-4">
            <div class="max-w-6xl mx-auto">
                <div class="flex items-center justify-between mb-6 sticky top-0 bg-gray-900 py-4 z-10">
                    <h2 class="text-2xl font-bold text-white flex items-center gap-3">
                        <i data-lucide="film" class="w-8 h-8 text-purple-500"></i>
                        Video Library
                    </h2>
                    <div class="flex gap-3">
                        <button onclick="showRequestVideoModal()" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg font-medium transition flex items-center gap-2">
                            <i data-lucide="clock" class="w-4 h-4"></i>
                            Request Past Video
                        </button>
                        <button onclick="closeVideoLibrary()" class="bg-gray-800 hover:bg-gray-700 text-white px-4 py-2 rounded-lg font-medium transition">
                            Close
                        </button>
                    </div>
                </div>

                <!-- Tabs -->
                <div class="flex gap-4 mb-6 border-b border-gray-700 pb-4">
                    <button onclick="switchLibraryTab('saved')" id="savedTab" class="text-blue-400 font-semibold border-b-2 border-blue-400 pb-1">Saved Videos</button>
                    <button onclick="switchLibraryTab('requested')" id="requestedTab" class="text-gray-400 hover:text-white transition pb-1">Requested Videos</button>
                </div>

                <!-- Saved Videos Grid -->
                <div id="savedVideosGrid" class="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <!-- Populated by JS -->
                </div>

                <!-- Requested Videos Grid -->
                <div id="requestedVideosGrid" class="hidden grid md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <!-- Populated by JS -->
                </div>
            </div>
        </div>
    </div>

    <!-- Request Past Video Modal -->
    <div id="requestVideoModal" class="fixed inset-0 z-50 hidden bg-black bg-opacity-80 flex items-center justify-center p-4">
        <div class="bg-gray-800 rounded-2xl p-6 w-full max-w-lg border border-gray-700">
            <h3 class="text-xl font-bold text-white mb-4 flex items-center gap-2">
                <i data-lucide="archive" class="w-6 h-6 text-blue-500"></i>
                Request Past Video
            </h3>
            <p class="text-gray-400 text-sm mb-6">Videos are 10 minutes long. Requested videos will be available in your library within 24 hours.</p>
            
            <form id="requestVideoForm" class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-gray-300 mb-1">Date</label>
                    <input type="date" id="requestDate" class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:ring-2 focus:ring-blue-500 outline-none" required>
                </div>
                
                <div>
                    <label class="block text-sm font-medium text-gray-300 mb-1">Start Time (10-min clip)</label>
                    <input type="time" id="requestTime" class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:ring-2 focus:ring-blue-500 outline-none" required>
                </div>
                
                <div>
                    <label class="block text-sm font-medium text-gray-300 mb-1">Camera</label>
                    <select id="requestCamera" class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:ring-2 focus:ring-blue-500 outline-none">
                        <!-- Populated by JS based on current road -->
                    </select>
                </div>

                <div class="flex gap-3 pt-4">
                    <button type="button" onclick="closeRequestVideoModal()" class="flex-1 bg-gray-700 hover:bg-gray-600 text-white py-2 rounded-lg transition">Cancel</button>
                    <button type="submit" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2 rounded-lg transition font-medium">Submit Request</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Driver Reports Modal -->
    <div id="driverReportsModal" class="fixed inset-0 z-50 hidden bg-black bg-opacity-80 flex items-center justify-center p-4">
        <div class="bg-gray-800 rounded-2xl p-6 w-full max-w-2xl max-h-[80vh] overflow-hidden flex flex-col border border-gray-700">
            <div class="flex items-center justify-between mb-4">
                <h3 class="text-xl font-bold text-white flex items-center gap-2">
                    <i data-lucide="users" class="w-6 h-6 text-yellow-500"></i>
                    Driver Reports
                </h3>
                <button onclick="closeDriverReports()" class="text-gray-400 hover:text-white">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>
            
            <div id="reportsList" class="overflow-y-auto flex-1 space-y-3 pr-2">
                <!-- Populated by JS -->
            </div>
            
            <button onclick="reportTraffic()" class="mt-4 w-full bg-yellow-600 hover:bg-yellow-700 text-white py-3 rounded-lg font-medium transition flex items-center justify-center gap-2">
                <i data-lucide="plus" class="w-5 h-5"></i>
                Report New Traffic
            </button>
        </div>
    </div>

    <!-- Scripts -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <script>
        // Initialize Lucide icons
        lucide.createIcons();

        // State Management
        const state = {
            user: null,
            subscription: null, // 'normal', 'premium', 'platinum'
            currentRoad: null,
            currentCameraIndex: 0,
            cameras: [],
            autoSwitchInterval: null,
            autoSwitchEnabled: true,
            savedVideos: JSON.parse(localStorage.getItem('savedVideos') || '[]'),
            requestedVideos: JSON.parse(localStorage.getItem('requestedVideos') || '[]'),
            driverReports: [
                { id: 1, road: 'Mombasa Road', location: 'Near GM', severity: 'high', time: '10 mins ago', description: 'Accident causing 2km backup' },
                { id: 2, road: 'Thika Road', location: 'Ruaraka', severity: 'medium', time: '25 mins ago', description: 'Broken down truck on shoulder' },
                { id: 3, road: 'Waiyaki Way', location: 'Westlands', severity: 'low', time: '1 hour ago', description: 'Slow traffic due to road works' }
            ],
            deviceCount: 0,
            map: null,
            markers: []
        };

        // Road Database with Coordinates
        const roads = [
            { name: 'Mombasa Road', coords: [-1.3231, 36.8447], cameras: 15 },
            { name: 'Thika Road', coords: [-1.2667, 36.9167], cameras: 20 },
            { name: 'Waiyaki Way', coords: [-1.2649, 36.7947], cameras: 12 },
            { name: 'Langata Road', coords: [-1.3282, 36.7908], cameras: 10 },
            { name: 'Ngong Road', coords: [-1.3100, 36.7600], cameras: 8 },
            { name: 'Jogoo Road', coords: [-1.2900, 36.8700], cameras: 9 },
            { name: 'Outer Ring Road', coords: [-1.2800, 36.8900], cameras: 14 },
            { name: 'Kenyatta Avenue', coords: [-1.2833, 36.8167], cameras: 6 }
        ];

        // Login Handler
        document.getElementById('loginForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const email = document.getElementById('emailInput').value;
            
            // Check device limit simulation
            const deviceId = localStorage.getItem('deviceId') || Math.random().toString(36).substr(2, 9);
            localStorage.setItem('deviceId', deviceId);
            
            state.user = { email, deviceId };
            
            // Show subscription if new user, else check subscription
            const savedSub = localStorage.getItem('subscription');
            if (savedSub) {
                selectPlan(savedSub, true);
            } else {
                showSubscription();
            }
        });

        function showSubscription() {
            document.getElementById('loginSection').classList.add('hidden');
            document.getElementById('subscriptionSection').classList.remove('hidden');
        }

        function backToLogin() {
            document.getElementById('subscriptionSection').classList.add('hidden');
            document.getElementById('loginSection').classList.remove('hidden');
        }

        function logoutFromSearch() {
            if (confirm('Are you sure you want to logout?')) {
                // Remove device from active lists
                if (state.user && state.user.deviceId) {
                    const normalDevices = JSON.parse(localStorage.getItem('activeDevices') || '[]');
                    const platinumDevices = JSON.parse(localStorage.getItem('activeDevicesPlatinum') || '[]');
                    localStorage.setItem('activeDevices', JSON.stringify(normalDevices.filter(d => d !== state.user.deviceId)));
                    localStorage.setItem('activeDevicesPlatinum', JSON.stringify(platinumDevices.filter(d => d !== state.user.deviceId)));
                }
                
                state.user = null;
                state.subscription = null;
                state.currentRoad = null;
                
                document.getElementById('searchSection').classList.add('hidden');
                document.getElementById('loginSection').classList.remove('hidden');
                document.getElementById('emailInput').value = '';
            }
        }

        function selectPlan(plan, skipPayment = false) {
            state.subscription = plan;
            localStorage.setItem('subscription', plan);
            
            // Update badge
            const badge = document.getElementById('subscriptionBadge');
            badge.textContent = plan.charAt(0).toUpperCase() + plan.slice(1);
            badge.className = `px-3 py-1 rounded-full text-xs font-semibold ${
                plan === 'platinum' ? 'bg-purple-600 text-white' :
                plan === 'premium' ? 'bg-blue-600 text-white' :
                'bg-green-600 text-white'
            }`;

            // Show/hide features based on plan
            if (plan === 'premium' || plan === 'platinum') {
                document.getElementById('cameraModeBtn').classList.remove('hidden');
                document.getElementById('cameraListPanel').classList.remove('hidden');
            }
            
            if (plan === 'platinum') {
                document.getElementById('libraryBtn').classList.remove('hidden');
                document.getElementById('saveVideoBtn').classList.remove('hidden');
            }

            // Device limit check simulation
            if (plan !== 'platinum') {
                // Simulate checking if already logged in elsewhere
                const activeDevices = JSON.parse(localStorage.getItem('activeDevices') || '[]');
                if (activeDevices.length >= 1 && !activeDevices.includes(state.user.deviceId)) {
                    alert('This account is already in use on another device. Please upgrade to Platinum for multi-device access.');
                    return;
                }
                if (!activeDevices.includes(state.user.deviceId)) {
                    activeDevices.push(state.user.deviceId);
                    localStorage.setItem('activeDevices', JSON.stringify(activeDevices));
                }
            } else {
                // Platinum allows 5 devices
                const activeDevices = JSON.parse(localStorage.getItem('activeDevicesPlatinum') || '[]');
                if (activeDevices.length >= 5 && !activeDevices.includes(state.user.deviceId)) {
                    alert('Maximum 5 devices reached for Platinum account.');
                    return;
                }
                if (!activeDevices.includes(state.user.deviceId)) {
                    activeDevices.push(state.user.deviceId);
                    localStorage.setItem('activeDevicesPlatinum', JSON.stringify(activeDevices));
                }
            }

            if (!skipPayment) {
                // Simulate payment processing
                const btn = event.target;
                btn.textContent = 'Processing...';
                setTimeout(() => {
                    proceedToSearch();
                }, 1500);
            } else {
                proceedToSearch();
            }
        }

        function proceedToSearch() {
            document.getElementById('subscriptionSection').classList.add('hidden');
            document.getElementById('searchSection').classList.remove('hidden');
            document.getElementById('searchSection').classList.add('flex');
            lucide.createIcons();
        }

        // Search Functionality
        const searchInput = document.getElementById('roadSearchInput');
        const searchResults = document.getElementById('searchResults');

        searchInput.addEventListener('input', (e) => {
            const query = e.target.value.toLowerCase();
            if (query.length < 2) {
                searchResults.innerHTML = '';
                return;
            }

            const matches = roads.filter(road => 
                road.name.toLowerCase().includes(query)
            );

            searchResults.innerHTML = matches.map(road => `
                <button onclick="selectRoad('${road.name}')" class="w-full text-left p-4 bg-gray-800 hover:bg-gray-700 rounded-lg transition flex items-center justify-between group">
                    <div>
                        <div class="font-semibold text-white group-hover:text-blue-400 transition">${road.name}</div>
                        <div class="text-sm text-gray-400">${road.cameras} cameras • ${Math.floor(Math.random() * 5 + 1)}km stretch</div>
                    </div>
                    <i data-lucide="chevron-right" class="w-5 h-5 text-gray-500"></i>
                </button>
            `).join('');
            
            lucide.createIcons();
        });

        function quickSearch(roadName) {
            searchInput.value = roadName;
            selectRoad(roadName);
        }

        function showSearch() {
            document.getElementById('mainApp').classList.add('hidden');
            document.getElementById('mainApp').style.display = 'none';
            document.getElementById('searchSection').classList.remove('hidden');
            document.getElementById('searchSection').classList.add('flex');
            searchInput.value = '';
            searchInput.focus();
            lucide.createIcons();
        }

        function selectRoad(roadName) {
            state.currentRoad = roads.find(r => r.name === roadName);
            state.currentCameraIndex = 0;
            
            // Generate cameras for this road
            state.cameras = Array.from({ length: state.currentRoad.cameras }, (_, i) => ({
                id: `${roadName.replace(/\s+/g, '-').toLowerCase()}-cam-${String(i + 1).padStart(3, '0')}`,
                name: `${roadName} Cam-${i + 1}`,
                distance: i * 100,
                lat: state.currentRoad.coords[0] + (i * 0.001),
                lng: state.currentRoad.coords[1] + (i * 0.001)
            }));

            document.getElementById('searchSection').classList.add('hidden');
            document.getElementById('mainApp').classList.remove('hidden');
            document.getElementById('currentRoadDisplay').textContent = roadName;

            // Small delay to ensure DOM is ready before map init
            setTimeout(() => {
                initMap();
                updateCameraList();
            }, 100);
        }

        // Leaflet Map Initialization
        function initMap() {
            if (state.map) {
                state.map.remove();
                state.map = null;
            }

            // Ensure map container is visible and has dimensions
            const mapContainer = document.getElementById('map');
            mapContainer.style.height = '100%';
            mapContainer.style.width = '100%';

            // Small delay to ensure DOM is ready
            setTimeout(() => {
                state.map = L.map('map', {
                    center: state.currentRoad.coords,
                    zoom: 14,
                    zoomControl: true
                });

                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                    attribution: '© OpenStreetMap contributors',
                    maxZoom: 19
                }).addTo(state.map);

                // Add traffic jam markers (simulated)
                const trafficPoints = [
                    { lat: state.currentRoad.coords[0] + 0.002, lng: state.currentRoad.coords[1] + 0.002, severity: 'high' },
                    { lat: state.currentRoad.coords[0] - 0.001, lng: state.currentRoad.coords[1] - 0.001, severity: 'medium' }
                ];

                trafficPoints.forEach(point => {
                    const color = point.severity === 'high' ? 'red' : 'yellow';
                    L.circleMarker([point.lat, point.lng], {
                        radius: 8,
                        fillColor: color,
                        color: '#fff',
                        weight: 2,
                        opacity: 1,
                        fillOpacity: 0.8
                    }).addTo(state.map).bindPopup(`Traffic Jam: ${point.severity.toUpperCase()}`);
                });

                // Add camera markers
                state.markers = [];
                state.cameras.forEach((cam, idx) => {
                    const marker = L.marker([cam.lat, cam.lng], {
                        icon: L.divIcon({
                            className: 'custom-div-icon',
                            html: `<div style="background-color: #3b82f6; width: 12px; height: 12px; border-radius: 50%; border: 2px solid white; box-shadow: 0 0 4px rgba(0,0,0,0.5);"></div>`,
                            iconSize: [12, 12],
                            iconAnchor: [6, 6]
                        })
                    }).addTo(state.map);
                    
                    marker.bindPopup(`<b>${cam.name}</b><br>Distance: ${cam.distance}m`);
                    state.markers.push(marker);
                });

                // Force map to recalculate size
                state.map.invalidateSize();
            }, 100);
        }

        function updateCameraList() {
            const list = document.getElementById('cameraList');
            list.innerHTML = state.cameras.map((cam, idx) => `
                <div class="flex items-center justify-between p-2 hover:bg-gray-100 rounded cursor-pointer ${idx === state.currentCameraIndex ? 'bg-blue-50 border border-blue-200' : ''}" onclick="jumpToCamera(${idx})">
                    <div class="flex items-center gap-2">
                        <i data-lucide="camera" class="w-4 h-4 text-blue-500"></i>
                        <span class="font-medium ${idx === state.currentCameraIndex ? 'text-blue-700' : 'text-gray-700'}">${cam.name}</span>
                    </div>
                    <span class="text-xs text-gray-500">${cam.distance}m</span>
                </div>
            `).join('');
            lucide.createIcons();
        }

        // Camera Mode Functions
        function startCameraMode() {
            if (state.subscription === 'normal') {
                alert('Upgrade to Premium or Platinum to access live cameras.');
                return;
            }
            
            document.getElementById('cameraModeModal').classList.remove('hidden');
            state.currentCameraIndex = 0;
            loadCameraFeed();
            startAutoSwitch();
        }

        function exitCameraMode() {
            document.getElementById('cameraModeModal').classList.add('hidden');
            stopAutoSwitch();
            // Ensure map is properly refreshed when returning
            if (state.map) {
                setTimeout(() => {
                    state.map.invalidateSize();
                }, 100);
            }
        }

        function loadCameraFeed() {
            const cam = state.cameras[state.currentCameraIndex];
            document.getElementById('liveCameraName').textContent = cam.name;
            document.getElementById('distanceMarker').textContent = `${cam.distance}m`;
            
            // Calculate next camera
            const nextCam = state.cameras[state.currentCameraIndex + 1];
            if (nextCam) {
                document.getElementById('nextCameraInfo').textContent = `${nextCam.distance - cam.distance}m ahead`;
            } else {
                document.getElementById('nextCameraInfo').textContent = 'End of route';
            }

            // Simulate camera feed with static photos
            const seed = Math.floor(Math.random() * 1000);
            document.getElementById('liveFeedImage').src = `https://static.photos/traffic/1024x576/${seed}`;
            
            // Update map marker highlight
            if (state.markers[state.currentCameraIndex]) {
                state.map.setView([cam.lat, cam.lng], 16);
            }
        }

        function startAutoSwitch() {
            state.autoSwitchEnabled = true;
            let countdown = 10;
            
            state.autoSwitchInterval = setInterval(() => {
                if (!state.autoSwitchEnabled) return;
                
                countdown--;
                document.getElementById('countdown').textContent = countdown;
                document.getElementById('progressBar').style.width = `${(countdown / 10) * 100}%`;
                
                if (countdown <= 0) {
                    if (state.currentCameraIndex < state.cameras.length - 1) {
                        state.currentCameraIndex++;
                        loadCameraFeed();
                        countdown = 10;
                    } else {
                        stopAutoSwitch();
                        document.getElementById('autoSwitchBtn').innerHTML = '<i data-lucide="check" class="w-5 h-5"></i> End of Route';
                        lucide.createIcons();
                    }
                }
            }, 1000);
        }

        function stopAutoSwitch() {
            clearInterval(state.autoSwitchInterval);
        }

        function toggleAutoSwitch() {
            state.autoSwitchEnabled = !state.autoSwitchEnabled;
            const btn = document.getElementById('autoSwitchBtn');
            if (state.autoSwitchEnabled) {
                btn.innerHTML = '<i data-lucide="pause" class="w-5 h-5"></i> Pause Auto';
                btn.classList.remove('bg-yellow-600', 'hover:bg-yellow-700');
                btn.classList.add('bg-green-600', 'hover:bg-green-700');
            } else {
                btn.innerHTML = '<i data-lucide="play" class="w-5 h-5"></i> Resume Auto';
                btn.classList.remove('bg-green-600', 'hover:bg-green-700');
                btn.classList.add('bg-yellow-600', 'hover:bg-yellow-700');
            }
            lucide.createIcons();
        }

        function manualSwitchCamera(steps) {
            const newIndex = state.currentCameraIndex + steps;
            if (newIndex >= 0 && newIndex < state.cameras.length) {
                state.currentCameraIndex = newIndex;
                loadCameraFeed();
                // Reset countdown
                document.getElementById('countdown').textContent = '10';
                document.getElementById('progressBar').style.width = '100%';
            }
        }

        function jumpToCamera(index) {
            state.currentCameraIndex = index;
            if (!document.getElementById('cameraModeModal').classList.contains('hidden')) {
                loadCameraFeed();
            }
        }

        function saveCurrentVideo() {
            if (state.subscription !== 'platinum') return;
            
            const cam = state.cameras[state.currentCameraIndex];
            const video = {
                id: Date.now(),
                camera: cam.name,
                cameraId: cam.id,
                date: new Date().toISOString().split('T')[0],
                time: new Date().toLocaleTimeString(),
                thumbnail: document.getElementById('liveFeedImage').src,
                savedAt: new Date().toISOString()
            };
            
            state.savedVideos.unshift(video);
            localStorage.setItem('savedVideos', JSON.stringify(state.savedVideos));
            
            // Visual feedback
            const btn = document.getElementById('saveVideoBtn');
            const originalHTML = btn.innerHTML;
            btn.innerHTML = '<i data-lucide="check" class="w-5 h-5"></i> Saved!';
            btn.classList.remove('bg-purple-600', 'hover:bg-purple-700');
            btn.classList.add('bg-green-600');
            lucide.createIcons();
            
            setTimeout(() => {
                btn.innerHTML = originalHTML;
                btn.classList.remove('bg-green-600');
                btn.classList.add('bg-purple-600', 'hover:bg-purple-700');
                lucide.createIcons();
            }, 2000);
        }

        // Video Library Functions
        function showVideoLibrary() {
            if (state.subscription !== 'platinum') return;
            document.getElementById('videoLibraryModal').classList.remove('hidden');
            renderSavedVideos();
            renderRequestedVideos();
        }

        function closeVideoLibrary() {
            document.getElementById('videoLibraryModal').classList.add('hidden');
            // Ensure map interactions work again
            if (state.map) {
                state.map.invalidateSize();
            }
        }

        function switchLibraryTab(tab) {
            const savedGrid = document.getElementById('savedVideosGrid');
            const requestedGrid = document.getElementById('requestedVideosGrid');
            const savedTab = document.getElementById('savedTab');
            const requestedTab = document.getElementById('requestedTab');
            
            if (tab === 'saved') {
                savedGrid.classList.remove('hidden');
                requestedGrid.classList.add('hidden');
                savedTab.className = 'text-blue-400 font-semibold border-b-2 border-blue-400 pb-1';
                requestedTab.className = 'text-gray-400 hover:text-white transition pb-1';
            } else {
                savedGrid.classList.add('hidden');
                requestedGrid.classList.remove('hidden');
                savedTab.className = 'text-gray-400 hover:text-white transition pb-1';
                requestedTab.className = 'text-blue-400 font-semibold border-b-2 border-blue-400 pb-1';
            }
        }

        function renderSavedVideos() {
            const grid = document.getElementById('savedVideosGrid');
            if (state.savedVideos.length === 0) {
                grid.innerHTML = '<div class="col-span-3 text-center text-gray-500 py-12">No saved videos yet</div>';
                return;
            }
            
            grid.innerHTML = state.savedVideos.map(video => `
                <div class="video-card bg-gray-800 rounded-lg overflow-hidden border border-gray-700 relative group">
                    <div class="relative aspect-video">
                        <img src="${video.thumbnail}" class="w-full h-full object-cover" alt="${video.camera}">
                        <div class="absolute inset-0 bg-black bg-opacity-40 opacity-0 group-hover:opacity-100 transition flex items-center justify-center">
                            <button class="bg-white text-gray-900 rounded-full p-3 hover:bg-gray-100 transition">
                                <i data-lucide="play" class="w-6 h-6 fill-current"></i>
                            </button>
                        </div>
                    </div>
                    <div class="p-4">
                        <div class="font-semibold text-white mb-1">${video.camera}</div>
                        <div class="text-sm text-gray-400 mb-3">${video.date} • ${video.time}</div>
                        <button onclick="deleteVideo(${video.id})" class="delete-btn opacity-100 bg-red-600 hover:bg-red-700 text-white px-3 py-1 rounded text-sm transition flex items-center gap-1">
                            <i data-lucide="trash-2" class="w-4 h-4"></i>
                            Delete
                        </button>
                    </div>
                </div>
            `).join('');
            lucide.createIcons();
        }

        function deleteVideo(id) {
            if (confirm('Are you sure you want to delete this video?')) {
                state.savedVideos = state.savedVideos.filter(v => v.id !== id);
                localStorage.setItem('savedVideos', JSON.stringify(state.savedVideos));
                renderSavedVideos();
            }
        }

        function renderRequestedVideos() {
            const grid = document.getElementById('requestedVideosGrid');
            if (state.requestedVideos.length === 0) {
                grid.innerHTML = '<div class="col-span-3 text-center text-gray-500 py-12">No requested videos yet</div>';
                return;
            }
            
            grid.innerHTML = state.requestedVideos.map(video => `
                <div class="bg-gray-800 rounded-lg overflow-hidden border border-gray-700 p-4">
                    <div class="flex items-start justify-between mb-3">
                        <div>
                            <div class="font-semibold text-white">${video.camera}</div>
                            <div class="text-sm text-gray-400">${video.date} • ${video.startTime}</div>
                        </div>
                        <span class="px-2 py-1 rounded text-xs ${video.status === 'ready' ? 'bg-green-600 text-white' : 'bg-yellow-600 text-white'}">
                            ${video.status === 'ready' ? 'Ready' : 'Processing'}
                        </span>
                    </div>
                    ${video.status === 'ready' ? `
                        <button class="w-full bg-blue-600 hover:bg-blue-700 text-white py-2 rounded transition text-sm flex items-center justify-center gap-2">
                            <i data-lucide="play" class="w-4 h-4"></i>
                            Play Video
                        </button>
                    ` : `
                        <div class="text-sm text-gray-500 text-center py-2">Available in ~24 hours</div>
                    `}
                </div>
            `).join('');
            lucide.createIcons();
        }

        // Request Past Video Functions
        function showRequestVideoModal() {
            const select = document.getElementById('requestCamera');
            select.innerHTML = state.cameras.map(cam => 
                `<option value="${cam.id}">${cam.name}</option>`
            ).join('');
            
            // Set default date to today
            document.getElementById('requestDate').valueAsDate = new Date();
            
            document.getElementById('requestVideoModal').classList.remove('hidden');
        }

        function closeRequestVideoModal() {
            document.getElementById('requestVideoModal').classList.add('hidden');
            // Return focus to main app
            document.getElementById('mainApp').focus();
        }

        document.getElementById('requestVideoForm').addEventListener('submit', (e) => {
            e.preventDefault();
            
            const date = document.getElementById('requestDate').value;
            const time = document.getElementById('requestTime').value;
            const cameraId = document.getElementById('requestCamera').value;
            const camera = state.cameras.find(c => c.id === cameraId);
            
            const request = {
                id: Date.now(),
                camera: camera.name,
                cameraId: cameraId,
                date: date,
                startTime: time,
                status: 'processing',
                requestedAt: new Date().toISOString()
            };
            
            state.requestedVideos.unshift(request);
            localStorage.setItem('requestedVideos', JSON.stringify(state.requestedVideos));
            
            // Simulate 24-hour processing (for demo, we'll make one instantly ready after 5 seconds)
            setTimeout(() => {
                request.status = 'ready';
                localStorage.setItem('requestedVideos', JSON.stringify(state.requestedVideos));
                if (!document.getElementById('videoLibraryModal').classList.contains('hidden')) {
                    renderRequestedVideos();
                }
            }, 5000);
            
            closeRequestVideoModal();
            alert('Video request submitted! It will be available in your library within 24 hours.');
        });

        // Driver Reports Functions
        function showDriverReports() {
            const list = document.getElementById('reportsList');
            list.innerHTML = state.driverReports.map(report => `
                <div class="bg-gray-700 rounded-lg p-4 border-l-4 ${report.severity === 'high' ? 'border-red-500' : report.severity === 'medium' ? 'border-yellow-500' : 'border-green-500'}">
                    <div class="flex items-start justify-between mb-2">
                        <div>
                            <div class="font-semibold text-white">${report.road}</div>
                            <div class="text-sm text-gray-400">${report.location}</div>
                        </div>
                        <span class="text-xs text-gray-500">${report.time}</span>
                    </div>
                    <div class="text-sm text-gray-300">${report.description}</div>
                    <div class="mt-2 flex items-center gap-2">
                        <span class="text-xs px-2 py-1 rounded bg-gray-600 text-gray-300 uppercase">${report.severity}</span>
                    </div>
                </div>
            `).join('');
            
            document.getElementById('driverReportsModal').classList.remove('hidden');
        }

        function closeDriverReports() {
            document.getElementById('driverReportsModal').classList.add('hidden');
            // Ensure map interactions work again
            if (state.map) {
                state.map.invalidateSize();
            }
        }

        function reportTraffic() {
            const description = prompt('Describe the traffic situation:');
            if (description) {
                const newReport = {
                    id: Date.now(),
                    road: state.currentRoad ? state.currentRoad.name : 'Unknown Road',
                    location: 'Current Location',
                    severity: 'medium',
                    time: 'Just now',
                    description: description
                };
                state.driverReports.unshift(newReport);
                document.getElementById('reportCount').textContent = state.driverReports.length;
                alert('Traffic reported successfully! Thank you for helping other drivers.');
                if (!document.getElementById('driverReportsModal').classList.contains('hidden')) {
                    showDriverReports();
                }
            }
        }

        function logout() {
            if (confirm('Are you sure you want to logout?')) {
                // Remove device from active list
                if (state.user && state.user.deviceId) {
                    const normalDevices = JSON.parse(localStorage.getItem('activeDevices') || '[]');
                    const platinumDevices = JSON.parse(localStorage.getItem('activeDevicesPlatinum') || '[]');
                    localStorage.setItem('activeDevices', JSON.stringify(normalDevices.filter(d => d !== state.user.deviceId)));
                    localStorage.setItem('activeDevicesPlatinum', JSON.stringify(platinumDevices.filter(d => d !== state.user.deviceId)));
                }
                
                // Reset state
                state.user = null;
                state.subscription = null;
                state.currentRoad = null;
                state.cameras = [];
                state.markers = [];
                
                // Hide all sections
                document.getElementById('mainApp').classList.add('hidden');
                document.getElementById('mainApp').style.display = 'none';
                document.getElementById('searchSection').classList.add('hidden');
                document.getElementById('searchSection').classList.remove('flex');
                document.getElementById('subscriptionSection').classList.add('hidden');
                
                // Show login
                document.getElementById('loginSection').classList.remove('hidden');
                document.getElementById('emailInput').value = '';
                
                // Clear map
                if (state.map) {
                    state.map.remove();
                    state.map = null;
                }
                
                // Reset UI elements
                document.getElementById('cameraModeBtn').classList.add('hidden');
                document.getElementById('cameraListPanel').classList.add('hidden');
                document.getElementById('libraryBtn').classList.add('hidden');
                document.getElementById('saveVideoBtn').classList.add('hidden');
            }
        }

        // Initialize
        window.addEventListener('DOMContentLoaded', () => {
            // Check if already logged in
            const savedEmail = localStorage.getItem('userEmail');
            if (savedEmail) {
                document.getElementById('emailInput').value = savedEmail;
            }
            
            // Ensure all icons are created
            lucide.createIcons();
            
            // Handle window resize for map
            window.addEventListener('resize', () => {
                if (state.map) {
                    state.map.invalidateSize();
                }
            });
        });
    </script>
<script src="https://deepsite.hf.co/deepsite-badge.js"></script>
</body>
</html>
