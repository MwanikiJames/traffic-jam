<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traffic Jam - Smart Road Monitoring</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    <link rel="stylesheet" href="css/style.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
</head>
<body class="font-['Inter'] bg-gray-50 text-gray-900">

    <!-- Landing Page (Shown when not authenticated) -->
    <div id="landingPage" class="min-h-screen">
        <!-- Navigation -->
        <nav class="bg-white border-b border-gray-200 sticky top-0 z-50">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between h-16 items-center">
                    <div class="flex items-center gap-2">
                        <i data-lucide="traffic-cone" class="w-8 h-8 text-orange-500"></i>
                        <span class="text-xl font-bold text-gray-900">Traffic Jam</span>
                    </div>
                    <div class="flex gap-4">
                        <button onclick="showLoginModal()" class="text-gray-600 hover:text-gray-900 font-medium">Login</button>
                        <button onclick="showSignupModal()" class="bg-orange-500 text-white px-4 py-2 rounded-lg font-medium hover:bg-orange-600 transition">Get Started</button>
                    </div>
                </div>
            </div>
        </nav>

        <!-- Hero Section -->
        <div class="relative bg-gradient-to-br from-orange-50 to-white overflow-hidden">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-20 lg:py-32">
                <div class="text-center max-w-3xl mx-auto">
                    <h1 class="text-5xl font-bold text-gray-900 mb-6 leading-tight">
                        Beat the Traffic with <span class="text-orange-500">Live Intelligence</span>
                    </h1>
                    <p class="text-xl text-gray-600 mb-8">
                        Real-time road monitoring with AI-powered CCTV sensors every 100m. 
                        Choose your view: Basic tracking, Live feeds, or Platinum archival access.
                    </p>
                    <div class="flex justify-center gap-4">
                        <button onclick="scrollToPricing()" class="bg-orange-500 text-white px-8 py-4 rounded-xl font-semibold text-lg hover:bg-orange-600 transition shadow-lg hover:shadow-xl">
                            View Plans
                        </button>
                        <button onclick="showDemo()" class="bg-white text-orange-500 border-2 border-orange-500 px-8 py-4 rounded-xl font-semibold text-lg hover:bg-orange-50 transition">
                            Live Demo
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Pricing Section -->
        <div id="pricing" class="py-20 bg-white">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center mb-16">
                    <h2 class="text-3xl font-bold text-gray-900 mb-4">Choose Your Journey</h2>
                    <p class="text-gray-600">Select the perfect plan for your daily commute</p>
                </div>

                <div class="grid md:grid-cols-3 gap-8 max-w-6xl mx-auto">
                    <!-- Normal Plan -->
                    <div class="border border-gray-200 rounded-2xl p-8 hover:shadow-lg transition bg-white">
                        <div class="text-center mb-8">
                            <h3 class="text-2xl font-bold text-gray-900 mb-2">Normal</h3>
                            <div class="text-4xl font-bold text-orange-500 mb-2">100<span class="text-lg text-gray-500">/month</span></div>
                            <p class="text-gray-500 text-sm">Basic traffic monitoring</p>
                        </div>
                        <ul class="space-y-4 mb-8">
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Road & traffic status</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Jam alerts & reports</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>1 device at a time</span>
                            </li>
                            <li class="flex items-center gap-3 text-gray-400">
                                <i data-lucide="x" class="w-5 h-5"></i>
                                <span>Live CCTV access</span>
                            </li>
                        </ul>
                        <button onclick="selectPlan('normal')" class="w-full py-3 border-2 border-orange-500 text-orange-500 rounded-xl font-semibold hover:bg-orange-50 transition">
                            Select Normal
                        </button>
                    </div>

                    <!-- Premium Plan -->
                    <div class="border-2 border-orange-500 rounded-2xl p-8 shadow-xl bg-white relative transform scale-105">
                        <div class="absolute top-0 right-0 bg-orange-500 text-white px-4 py-1 rounded-bl-xl rounded-tr-xl text-sm font-semibold">
                            Popular
                        </div>
                        <div class="text-center mb-8">
                            <h3 class="text-2xl font-bold text-gray-900 mb-2">Premium</h3>
                            <div class="text-4xl font-bold text-orange-500 mb-2">500<span class="text-lg text-gray-500">/month</span></div>
                            <p class="text-gray-500 text-sm">Live camera access</p>
                        </div>
                        <ul class="space-y-4 mb-8">
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Everything in Normal</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Live CCTV streams</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Auto-switch cameras</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>1 device at a time</span>
                            </li>
                        </ul>
                        <button onclick="selectPlan('premium')" class="w-full py-3 bg-orange-500 text-white rounded-xl font-semibold hover:bg-orange-600 transition shadow-lg">
                            Select Premium
                        </button>
                    </div>

                    <!-- Platinum Plan -->
                    <div class="border border-gray-200 rounded-2xl p-8 hover:shadow-lg transition bg-gradient-to-b from-white to-orange-50">
                        <div class="text-center mb-8">
                            <h3 class="text-2xl font-bold text-gray-900 mb-2">Platinum</h3>
                            <div class="text-4xl font-bold text-orange-500 mb-2">1000<span class="text-lg text-gray-500">/month</span></div>
                            <p class="text-gray-500 text-sm">Ultimate control & archive</p>
                        </div>
                        <ul class="space-y-4 mb-8">
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Everything in Premium</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Save & download videos</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Request past videos (24h)</span>
                            </li>
                            <li class="flex items-center gap-3">
                                <i data-lucide="check" class="w-5 h-5 text-green-500"></i>
                                <span>Up to 5 devices</span>
                            </li>
                        </ul>
                        <button onclick="selectPlan('platinum')" class="w-full py-3 bg-gray-900 text-white rounded-xl font-semibold hover:bg-gray-800 transition">
                            Select Platinum
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Main Application (Hidden initially) -->
    <div id="appContainer" class="hidden min-h-screen bg-gray-100">
        <!-- App Header -->
        <header class="bg-white border-b border-gray-200 sticky top-0 z-40">
            <div class="max-w-full mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between h-16 items-center">
                    <div class="flex items-center gap-4">
                        <div class="flex items-center gap-2">
                            <i data-lucide="traffic-cone" class="w-6 h-6 text-orange-500"></i>
                            <span class="text-lg font-bold">Traffic Jam</span>
                        </div>
                        <!-- Search Bar (Post-Login) -->
                        <div class="hidden md:flex ml-8 relative">
                            <input type="text" id="searchRoad" placeholder="Search road or highway..." 
                                class="w-80 pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent"
                                onkeyup="searchRoads(this.value)">
                            <i data-lucide="search" class="w-5 h-5 text-gray-400 absolute left-3 top-2.5"></i>
                            <div id="searchResults" class="absolute top-full left-0 right-0 bg-white border border-gray-200 rounded-lg mt-1 shadow-lg hidden max-h-60 overflow-y-auto"></div>
                        </div>
                    </div>
                    <div class="flex items-center gap-4">
                        <span id="userTier" class="px-3 py-1 rounded-full text-sm font-medium bg-orange-100 text-orange-700">Normal</span>
                        <button onclick="logout()" class="text-gray-600 hover:text-gray-900">
                            <i data-lucide="log-out" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>
            </div>
        </header>

        <div class="flex h-[calc(100vh-64px)]">
            <!-- Sidebar -->
            <aside class="w-64 bg-white border-r border-gray-200 flex flex-col">
                <nav class="p-4 space-y-2 flex-1">
                    <button onclick="switchView('map')" class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-lg text-left hover:bg-gray-50 transition bg-orange-50 text-orange-700" id="nav-map">
                        <i data-lucide="map" class="w-5 h-5"></i>
                        <span>Traffic Map</span>
                    </button>
                    <button onclick="switchView('reports')" class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-lg text-left hover:bg-gray-50 transition text-gray-700" id="nav-reports">
                        <i data-lucide="alert-triangle" class="w-5 h-5"></i>
                        <span>Driver Reports</span>
                    </button>
                    <button onclick="startCameraMode()" class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-lg text-left hover:bg-gray-50 transition text-gray-700" id="nav-camera">
                        <i data-lucide="video" class="w-5 h-5"></i>
                        <span>Start Camera/Live Mode</span>
                    </button>
                    <button onclick="switchView('library')" class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-lg text-left hover:bg-gray-50 transition text-gray-700 hidden" id="nav-library">
                        <i data-lucide="library" class="w-5 h-5"></i>
                        <span>Video Library</span>
                    </button>
                    <button onclick="switchView('request')" class="nav-item w-full flex items-center gap-3 px-4 py-3 rounded-lg text-left hover:bg-gray-50 transition text-gray-700 hidden" id="nav-request">
                        <i data-lucide="clock" class="w-5 h-5"></i>
                        <span>Request Past Video</span>
                    </button>
                </nav>
                <div class="p-4 border-t border-gray-200">
                    <div class="text-sm text-gray-500 mb-2">Active Devices: <span id="deviceCount">1/1</span></div>
                    <button onclick="showSubscriptionModal()" class="w-full py-2 border border-gray-300 rounded-lg text-sm font-medium hover:bg-gray-50 transition">
                        Manage Subscription
                    </button>
                </div>
            </aside>

            <!-- Main Content -->
            <main class="flex-1 overflow-auto p-6">
                <!-- Map View -->
                <div id="view-map" class="view-section h-full">
                    <div class="bg-white rounded-xl shadow-sm border border-gray-200 h-full flex flex-col">
                        <div class="p-4 border-b border-gray-200 flex justify-between items-center">
                            <h2 class="text-lg font-semibold">Live Traffic Overview</h2>
                            <div class="flex gap-2 text-sm">
                                <span class="flex items-center gap-1"><span class="w-3 h-3 rounded-full bg-red-500"></span> Heavy</span>
                                <span class="flex items-center gap-1"><span class="w-3 h-3 rounded-full bg-yellow-500"></span> Moderate</span>
                                <span class="flex items-center gap-1"><span class="w-3 h-3 rounded-full bg-green-500"></span> Clear</span>
                            </div>
                        </div>
                        <div id="mapContainer" class="flex-1 bg-gray-100 relative z-0"></div>
                    </div>
                </div>

                <!-- Camera Live View -->
                <div id="view-camera" class="view-section hidden h-full">
                    <div class="bg-black rounded-xl overflow-hidden h-full flex flex-col relative">
                        <!-- Camera Header -->
                        <div class="bg-gray-900 text-white p-4 flex justify-between items-center">
                            <div>
                                <h2 class="font-semibold" id="currentCameraName">Loading...</h2>
                                <p class="text-sm text-gray-400" id="cameraDetails">Camera #1 • 100m ahead</p>
                            </div>
                            <div class="flex items-center gap-2">
                                <span class="flex items-center gap-1 text-red-500 animate-pulse">
                                    <span class="w-2 h-2 bg-red-500 rounded-full"></span>
                                    LIVE
                                </span>
                            </div>
                        </div>
                        
                        <!-- Video Feed -->
                        <div class="flex-1 relative bg-gray-800 flex items-center justify-center overflow-hidden">
                            <img id="liveFeed" src="" alt="Live CCTV Feed" class="w-full h-full object-cover">
                            
                            <!-- Auto-switch Indicator -->
                            <div id="autoSwitchIndicator" class="absolute top-4 right-4 bg-black/70 text-white px-3 py-1 rounded-full text-sm hidden">
                                Auto-switching in <span id="countdown">5</span>s
                            </div>

                            <!-- Camera Controls -->
                            <div class="absolute bottom-8 left-1/2 transform -translate-x-1/2 flex gap-4">
                                <button onclick="manualCameraSwitch(-2)" class="bg-white/20 hover:bg-white/30 backdrop-blur text-white px-4 py-2 rounded-lg flex items-center gap-2 transition">
                                    <i data-lucide="skip-back" class="w-4 h-4"></i>
                                    -200m
                                </button>
                                <button onclick="toggleAutoSwitch()" id="autoSwitchBtn" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg flex items-center gap-2 transition">
                                    <i data-lucide="pause" class="w-4 h-4"></i>
                                    Pause Auto
                                </button>
                                <button onclick="manualCameraSwitch(2)" class="bg-white/20 hover:bg-white/30 backdrop-blur text-white px-4 py-2 rounded-lg flex items-center gap-2 transition">
                                    +200m
                                    <i data-lucide="skip-forward" class="w-4 h-4"></i>
                                </button>
                            </div>

                            <!-- Cancel Button (Back to Map) -->
                            <button onclick="exitCameraMode()" class="absolute top-4 left-4 bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg flex items-center gap-2 transition shadow-lg">
                                <i data-lucide="x" class="w-4 h-4"></i>
                                Exit Live Feed
                            </button>

                            <!-- Save Video Button (Platinum Only) -->
                            <button onclick="saveCurrentVideo()" id="saveVideoBtn" class="absolute bottom-8 right-4 bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg flex items-center gap-2 transition hidden shadow-lg">
                                <i data-lucide="download" class="w-4 h-4"></i>
                                Save Video
                            </button>
                        </div>

                        <!-- Camera Navigation Strip -->
                        <div class="bg-gray-900 p-4 flex gap-2 overflow-x-auto" id="cameraStrip">
                            <!-- Thumbnails populated by JS -->
                        </div>
                    </div>
                </div>

                <!-- Video Library (Platinum Only) -->
                <div id="view-library" class="view-section hidden">
                    <div class="max-w-6xl mx-auto">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-2xl font-bold">Video Library</h2>
                            <div class="flex gap-2">
                                <select id="filterVideos" class="border border-gray-300 rounded-lg px-3 py-2" onchange="filterLibrary()">
                                    <option value="all">All Videos</option>
                                    <option value="saved">Saved Live</option>
                                    <option value="requested">Requested Archive</option>
                                </select>
                            </div>
                        </div>
                        <div id="videoGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                            <!-- Videos populated by JS -->
                        </div>
                        <div id="emptyLibrary" class="text-center py-20 text-gray-500 hidden">
                            <i data-lucide="video-off" class="w-16 h-16 mx-auto mb-4 opacity-50"></i>
                            <p>No videos in your library yet.</p>
                        </div>
                    </div>
                </div>

                <!-- Request Video (Platinum Only) -->
                <div id="view-request" class="view-section hidden">
                    <div class="max-w-2xl mx-auto bg-white rounded-xl shadow-sm border border-gray-200 p-8">
                        <h2 class="text-2xl font-bold mb-6">Request Past Video Recording</h2>
                        <form id="videoRequestForm" onsubmit="submitVideoRequest(event)">
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-1">Date</label>
                                    <input type="date" id="reqDate" required class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-orange-500">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-1">Start Time</label>
                                    <input type="time" id="reqTime" required class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-orange-500">
                                    <p class="text-xs text-gray-500 mt-1">Videos are 10 minutes long. End time will be automatically calculated.</p>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-1">Camera Name & Number</label>
                                    <input type="text" id="reqCamera" placeholder="e.g., Mombasa Road Cam-45" required class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-orange-500">
                                </div>
                                <div class="bg-blue-50 border border-blue-200 rounded-lg p-4 text-sm text-blue-800">
                                    <i data-lucide="info" class="w-4 h-4 inline mr-2"></i>
                                    Requested videos will be available in your library within 24 hours.
                                </div>
                                <button type="submit" class="w-full bg-orange-500 text-white py-3 rounded-lg font-semibold hover:bg-orange-600 transition">
                                    Submit Request
                                </button>
                            </div>
                        </form>

                        <!-- Pending Requests -->
                        <div class="mt-8">
                            <h3 class="font-semibold mb-4">Pending Requests</h3>
                            <div id="pendingRequests" class="space-y-2">
                                <!-- Populated by JS -->
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Driver Reports -->
                <div id="view-reports" class="view-section hidden">
                    <div class="max-w-6xl mx-auto">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-2xl font-bold">Driver Reports</h2>
                            <button onclick="showReportModal()" class="bg-orange-500 text-white px-4 py-2 rounded-lg flex items-center gap-2 hover:bg-orange-600 transition">
                                <i data-lucide="plus" class="w-4 h-4"></i>
                                Report Traffic
                            </button>
                        </div>
                        <div id="reportsList" class="grid gap-4">
                            <!-- Reports populated by JS -->
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- Modals -->
    
    <!-- Login Modal -->
    <div id="loginModal" class="fixed inset-0 bg-black/50 hidden items-center justify-center z-50">
        <div class="bg-white rounded-xl p-8 max-w-md w-full mx-4">
            <h2 class="text-2xl font-bold mb-6">Welcome Back</h2>
            <form onsubmit="handleLogin(event)">
                <div class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                        <input type="email" required class="w-full border border-gray-300 rounded-lg px-3 py-2">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                        <input type="password" required class="w-full border border-gray-300 rounded-lg px-3 py-2">
                    </div>
                    <button type="submit" class="w-full bg-orange-500 text-white py-3 rounded-lg font-semibold hover:bg-orange-600 transition">
                        Login
                    </button>
                </div>
            </form>
            <button onclick="closeModal('loginModal')" class="mt-4 w-full text-gray-600 hover:text-gray-900">Cancel</button>
        </div>
    </div>

    <!-- Subscription Modal -->
    <div id="subscriptionModal" class="fixed inset-0 bg-black/50 hidden items-center justify-center z-50 overflow-y-auto">
        <div class="bg-white rounded-xl p-8 max-w-4xl w-full mx-4 my-8">
            <h2 class="text-2xl font-bold mb-6">Choose Your Subscription</h2>
            <div class="grid md:grid-cols-3 gap-6">
                <div class="border rounded-xl p-6 cursor-pointer hover:border-orange-500 transition" onclick="upgradePlan('normal')">
                    <h3 class="font-bold text-lg">Normal</h3>
                    <p class="text-2xl font-bold text-orange-500 my-2">100<span class="text-sm">/mo</span></p>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• Basic traffic data</li>
                        <li>• 1 Device</li>
                    </ul>
                </div>
                <div class="border-2 border-orange-500 rounded-xl p-6 cursor-pointer bg-orange-50" onclick="upgradePlan('premium')">
                    <h3 class="font-bold text-lg">Premium</h3>
                    <p class="text-2xl font-bold text-orange-500 my-2">500<span class="text-sm">/mo</span></p>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• Live CCTV access</li>
                        <li>• Auto-switching</li>
                        <li>• 1 Device</li>
                    </ul>
                </div>
                <div class="border rounded-xl p-6 cursor-pointer hover:border-orange-500 transition" onclick="upgradePlan('platinum')">
                    <h3 class="font-bold text-lg">Platinum</h3>
                    <p class="text-2xl font-bold text-orange-500 my-2">1000<span class="text-sm">/mo</span></p>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• Video save & archive</li>
                        <li>• Request past videos</li>
                        <li>• 5 Devices</li>
                    </ul>
                </div>
            </div>
            <button onclick="closeModal('subscriptionModal')" class="mt-6 w-full py-2 border border-gray-300 rounded-lg">Close</button>
        </div>
    </div>

    <!-- Report Traffic Modal -->
    <div id="reportModal" class="fixed inset-0 bg-black/50 hidden items-center justify-center z-50">
        <div class="bg-white rounded-xl p-8 max-w-md w-full mx-4">
            <h2 class="text-2xl font-bold mb-4">Report Traffic Condition</h2>
            <form onsubmit="submitReport(event)">
                <div class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Road/Location</label>
                        <input type="text" id="reportLocation" required class="w-full border border-gray-300 rounded-lg px-3 py-2">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Condition</label>
                        <select id="reportType" class="w-full border border-gray-300 rounded-lg px-3 py-2">
                            <option value="heavy">Heavy Traffic</option>
                            <option value="moderate">Moderate Traffic</option>
                            <option value="accident">Accident</option>
                            <option value="construction">Road Construction</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Details</label>
                        <textarea id="reportDetails" rows="3" class="w-full border border-gray-300 rounded-lg px-3 py-2"></textarea>
                    </div>
                    <button type="submit" class="w-full bg-orange-500 text-white py-3 rounded-lg font-semibold hover:bg-orange-600 transition">
                        Submit Report
                    </button>
                </div>
            </form>
            <button onclick="closeModal('reportModal')" class="mt-4 w-full text-gray-600">Cancel</button>
        </div>
    </div>

    <script src="js/app.js"></script>
    <script>
        lucide.createIcons();
    </script>
</body>
</html>
