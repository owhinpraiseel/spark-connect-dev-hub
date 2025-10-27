<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sparks & Connect</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <!-- Lucide icons for clean UI elements -->
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    
    <style>
        body {
            background-color: #f3f4f6;
            font-family: 'Inter', sans-serif;
            color: #1f2937;
            margin: 0;
            padding: 0;
            overflow-x: hidden; 
        }
        /* Style for the main content area to accommodate the fixed sidebar */
        .main-content {
            margin-left: 16rem; 
            width: calc(100% - 16rem); 
            padding: 2.5rem; 
            min-height: 100vh;
        }
        /* Responsive adjustments for mobile/smaller screens */
        @media (max-width: 768px) {
            .sidebar {
                width: 100%;
                height: auto;
                position: relative; /* Sidebar is not fixed on mobile */
            }
            .main-content {
                margin-left: 0;
                width: 100%;
                padding: 1rem;
            }
        }

        .card {
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.05);
        }
        /* Style for active sidebar link */
        .sidebar-active {
            background-color: #1f2937;
            border-left: 4px solid #f653a6; 
        }

        /* Challenge Card Styling (unchanged) */
        .challenge-card {
            transition: all 0.2s ease-in-out;
            border-left: 6px solid;
            border-radius: 0.75rem;
            overflow: hidden;
            cursor: pointer;
        }
        .challenge-card:hover {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.05);
            transform: translateY(-2px);
        }
        .challenge-list {
            max-height: 600px;
            overflow-y: auto;
        }
        .status-badge {
            font-weight: 600;
            padding: 4px 12px;
            border-radius: 9999px;
            font-size: 0.875rem;
        }
        /* Utility classes for border colors based on priority/status */
        .border-l-high { border-left-color: #f653a6; } 
        .hover\:border-l-high-hover:hover { border-left-color: #c71e7d; }
        .border-l-medium { border-left-color: #f59e0b; } 
        .hover\:border-l-medium-hover:hover { border-left-color: #b45309; }
        .border-l-low { border-left-color: #3b82f6; } 
        .hover\:border-l-low-hover:hover { border-left-color: #1d4ed8; }
        .border-l-completed { border-left-color: #10b981; } 
        .hover\:border-l-completed-hover:hover { border-left-color: #059669; }

        /* Custom Range Slider Style for Size Control */
        input[type=range].range-lg::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: #f653a6;
            cursor: pointer;
            margin-top: -7px; /* Adjusting for size */
            box-shadow: 0 0 0 3px rgba(246, 83, 166, 0.3);
        }
        input[type=range].range-lg::-moz-range-thumb {
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: #f653a6;
            cursor: pointer;
        }
        
        /* Styles for Sparks.AI Chat */
        .ai-chat-container {
            height: 60vh;
            display: flex;
            flex-direction: column;
            background-color: #ffffff;
            border-radius: 1rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }
        .ai-chat-messages {
            flex-grow: 1;
            overflow-y: auto;
            padding: 1rem;
            border-bottom: 1px solid #e5e7eb;
        }
        .ai-chat-input {
            padding: 1rem;
        }
        .ai-message {
            margin-bottom: 1rem;
            padding: 0.75rem;
            border-radius: 0.75rem;
            max-width: 90%;
        }
        .ai-message.user {
            margin-left: auto;
            background-color: #d1e5ff; /* Light Blue */
            color: #1f2937;
        }
        .ai-message.sparks {
            margin-right: auto;
            background-color: #f653a6; /* Pink */
            color: white;
        }
        .citation a {
            color: #d1e5ff;
            text-decoration: underline;
            font-weight: 600;
        }
        .citation a:hover {
            color: #f3f4f6;
        }
        
    </style>
</head>
<body>

    <!-- 1. Fixed Sidebar Navigation -->
    <div class="sidebar fixed top-0 left-0 w-64 h-full bg-gray-900 text-white flex flex-col shadow-2xl z-50 md:block hidden">
        
        <!-- Logo/Brand -->
        <div class="p-6 border-b border-gray-800">
            <h2 class="text-2xl font-extrabold tracking-wider text-pink-500">Sparks & Connect</h2>
            <p class="text-xs text-gray-500 mt-1">Dev Hub</p>
        </div>
        
        <!-- Navigation Links -->
        <nav class="flex-grow p-4 space-y-2">
            
            <a href="#" data-page="dashboard" class="nav-link flex items-center space-x-3 p-3 rounded-lg text-sm font-medium text-gray-400 hover:bg-gray-800 hover:text-white transition">
                <i data-lucide="layout-dashboard" class="w-5 h-5"></i>
                <span>Dashboard</span>
            </a>
            
            <a href="#" data-page="challenges" class="nav-link flex items-center space-x-3 p-3 rounded-lg text-sm font-medium text-gray-400 hover:bg-gray-800 hover:text-white transition">
                <i data-lucide="award" class="w-5 h-5"></i>
                <span>Challenges Hub</span>
            </a>
            
            <a href="#" data-page="leaderboard" class="nav-link flex items-center space-x-3 p-3 rounded-lg text-sm font-medium text-gray-400 hover:bg-gray-800 hover:text-white transition">
                <i data-lucide="list-ordered" class="w-5 h-5"></i>
                <span>Leaderboard</span>
            </a>

            <!-- NEW: Social Hub -->
            <a href="#" data-page="social-hub" class="nav-link flex items-center space-x-3 p-3 rounded-lg text-sm font-medium text-gray-400 hover:bg-gray-800 hover:text-white transition">
                <i data-lucide="users-2" class="w-5 h-5"></i>
                <span>Social Hub</span>
            </a>
            
            <!-- Updated Name: Creations -->
            <a href="#" data-page="creations" class="nav-link flex items-center space-x-3 p-3 rounded-lg text-sm font-medium text-gray-400 hover:bg-gray-800 hover:text-white transition">
                <i data-lucide="code" class="w-5 h-5"></i>
                <span>Creations</span>
            </a>
            
            <div class="pt-4 mt-4 border-t border-gray-800">
                <a href="#" onclick="console.log('Navigating to Settings');" class="flex items-center space-x-3 p-3 rounded-lg text-sm font-medium text-gray-400 hover:bg-gray-800 hover:text-white transition">
                    <i data-lucide="settings" class="w-5 h-5"></i>
                    <span>Settings</span>
                </a>
            </div>
        </nav>
    </div>

    <!-- 2. Main Content Area -->
    <div class="main-content">
        
        <!-- User Header -->
        <header class="mb-8 flex items-center justify-end">
            <div onclick="changeUserName()" id="user-profile-widget" class="flex items-center space-x-3 p-2 bg-white rounded-full shadow-md cursor-pointer hover:bg-gray-50 transition">
                <!-- Content injected by updateHeaderWidget() -->
            </div>
        </header>

        <!-- Dynamic Content Container -->
        <div id="dynamic-content">
            <!-- Content will be injected here -->
        </div>

        <footer class="mt-10 text-center text-gray-400 text-sm">
            Sparks & Connect Dev Hub | Built for creativity.
        </footer>
    </div>
    
    <script>
        // Initialize Lucide icons
        lucide.createIcons();
        
        // --- Global State ---
        let currentPage = 'dashboard'; 
        let userName = localStorage.getItem('userName') || 'J. Smith';
        let userInitials = userName.split(' ').map(n => n[0]).join('').toUpperCase() || 'JS';
        let currentFilter = 'active'; 

        // --- Sparks.AI State and Configuration ---
        const apiKey = ""; // Canvas will provide this if needed
        const modelName = "gemini-2.5-flash-preview-09-2025";
        const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/${modelName}:generateContent?key=${apiKey}`;
        
        // Initial welcome message for the AI chat
        const aiChatHistory = [
            { sender: 'sparks', text: "Hello! I'm **Sparks.AI**, your creative development assistant. What can I help you research or build today?" }
        ];

        // --- Persistent User ID Management for Social Hub ---
        let uniqueUserId = localStorage.getItem('uniqueUserId');
        if (!uniqueUserId) {
            uniqueUserId = (Math.random().toString(36).substring(2, 9) + Math.random().toString(36).substring(2, 9)).toUpperCase();
            localStorage.setItem('uniqueUserId', uniqueUserId);
        }

        const mockUsers = [
            { id: 'XYZ123ABC', name: 'Ava L.' },
            { id: 'TUV789WXY', name: 'Ben M.' },
            { id: 'OPQ456RST', name: 'Cali T.' },
            { id: 'GHI012JKL', name: 'David S.' },
        ];
        
        // --- Social Hub Friend List Management ---
        function getFriendsList() {
            try {
                const list = localStorage.getItem('friendsList');
                return list ? JSON.parse(list) : [];
            } catch (e) {
                console.error("Error loading friends list:", e);
                return [];
            }
        }

        function saveFriendsList(list) {
            localStorage.setItem('friendsList', JSON.stringify(list));
        }

        function addFriendById() {
            const friendIdInput = document.getElementById('friendIdInput');
            const friendId = friendIdInput.value.trim().toUpperCase();
            friendIdInput.value = ''; // Clear input

            const messageContainer = document.getElementById('friend-message');
            messageContainer.textContent = '';
            messageContainer.classList.remove('text-green-600', 'text-red-600');

            if (friendId === uniqueUserId) {
                messageContainer.textContent = "You can't add yourself!";
                messageContainer.classList.add('text-red-600');
                return;
            }

            const currentFriends = getFriendsList();
            if (currentFriends.some(f => f.id === friendId)) {
                messageContainer.textContent = "That person is already your friend.";
                messageContainer.classList.add('text-red-600');
                return;
            }

            const foundUser = mockUsers.find(u => u.id === friendId);

            if (foundUser) {
                const newFriend = { id: foundUser.id, name: foundUser.name };
                currentFriends.push(newFriend);
                saveFriendsList(currentFriends);
                messageContainer.textContent = `Successfully added ${foundUser.name} to your friends list!`;
                messageContainer.classList.add('text-green-600');
                
                navigate('social-hub', true); 
            } else {
                messageContainer.textContent = "User ID not found. Please check the ID and try again.";
                messageContainer.classList.add('text-red-600');
            }
        }
        
        // --- User Info Management (Unchanged) ---
        function updateUserInfo() {
            userName = localStorage.getItem('userName') || 'J. Smith';
            userInitials = userName.split(' ').map(n => n[0]).join('').toUpperCase() || 'JS';
        }

        function changeUserName() {
            updateUserInfo(); 
            const messageBox = document.createElement('div');
            messageBox.className = 'fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50';
            messageBox.innerHTML = `
                <div class="bg-white p-6 rounded-xl shadow-2xl w-96 max-w-full">
                    <h4 class="text-xl font-bold mb-4 text-gray-900">Change Display Name</h4>
                    <input type="text" id="newNameInput" value="${userName}" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-pink-500 focus:border-pink-500" placeholder="Enter your name">
                    <div class="flex justify-end space-x-3 mt-5">
                        <button onclick="this.parentNode.parentNode.parentNode.remove()" class="py-2 px-4 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition">
                            Cancel
                        </button>
                        <button id="saveNameBtn" class="py-2 px-4 bg-pink-600 text-white rounded-lg hover:bg-pink-700 transition">
                            Save Name
                        </button>
                    </div>
                </div>
            `;
            document.body.appendChild(messageBox);
            document.getElementById('newNameInput').focus();
            
            document.getElementById('saveNameBtn').addEventListener('click', () => {
                const newName = document.getElementById('newNameInput').value.trim();
                if (newName) {
                    localStorage.setItem('userName', newName);
                    updateUserInfo(); 
                    messageBox.remove();
                    updateHeaderWidget();
                    navigate(currentPage, true); 
                }
            });
        }

        function updateHeaderWidget() {
            const profileWidget = document.getElementById('user-profile-widget');
            if (profileWidget) {
                profileWidget.innerHTML = `
                    <span class="text-sm font-semibold hidden md:block">${userName}</span>
                    <div class="w-10 h-10 bg-blue-500 rounded-full flex items-center justify-center text-white font-bold">
                        ${userInitials}
                    </div>
                `;
            }
        }
        
        // --- Challenge Data Array (Unchanged) ---
        const challengesData = [
            { id: 'ch-001', title: 'Flappy Bird Collision Fix', description: 'Refactor the physics engine to resolve bird-pipe clipping bugs for mobile compatibility.', status: 'In Progress', priority: 'high', points: 650, deadline: '2025-10-27' },
            { id: 'ch-002', title: '8-bit Hero Sprite Sheet', description: 'Design a 32x32 pixel hero sprite with walk, jump, and idle animations using Aseprite.', status: 'In Progress', priority: 'medium', points: 400, deadline: '2025-10-30' },
            { id: 'ch-003', title: 'Metroidvania Level Draft', description: 'Create the map layout and hidden pathways for Sector C of the new world. (Design only)', status: 'Upcoming', priority: 'low', points: 950, deadline: '2025-11-05' },
            { id: 'ch-004', title: 'Implement Global Leaderboard', description: 'Integrate Firestore for real-time ranking updates across all regions.', status: 'In Progress', priority: 'low', points: 1500, deadline: '2025-11-15' },
            { id: 'ch-005', title: 'Refactor Game Loop (Completed)', description: 'Optimize the main game loop for 60 FPS across all platforms.', status: 'Completed', priority: 'low', points: 800, deadline: '2025-10-20' },
            { id: 'ch-006', title: 'Database Optimization', description: 'Index the user scoring table for faster leaderboard queries.', status: 'In Progress', priority: 'high', points: 700, deadline: '2025-11-01' }
        ];

        // --- Leaderboard Data (Updated to include only the current user) ---
        const leaderboardData = [
            { rank: 1, name: userName, score: 8750, progress: 15 },
        ];


        // --- Pixel Art Editor State and Layer Logic (Unchanged) ---
        
        const pixelArtState = {
            canvasSize: 16, 
            pixelSize: 0, 
            currentColor: '#F653A6', 
            history: [],
            historyIndex: -1,
            isDrawing: false,
            canvas: null,
            ctx: null,
            isErasing: false,
            activeLayerId: null, 
            layerCounter: 0, 
        };

        function showConfirmModal(title, message, onConfirm) {
            const existingModal = document.querySelector('.modal-overlay');
            if (existingModal) existingModal.remove();

            const modal = document.createElement('div');
            modal.className = 'fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-[100] modal-overlay';
            modal.innerHTML = `
                <div class="bg-white p-6 rounded-xl shadow-2xl w-80 max-w-full">
                    <h4 class="text-xl font-bold mb-3 text-gray-900">${title}</h4>
                    <p class="text-gray-700 mb-5">${message}</p>
                    <div class="flex justify-end space-x-3">
                        <button onclick="document.querySelector('.modal-overlay').remove()" class="py-2 px-4 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition">
                            Cancel
                        </button>
                        <button id="modalConfirmBtn" class="py-2 px-4 bg-red-600 text-white rounded-lg hover:bg-red-700 transition">
                            Confirm
                        </button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);

            document.getElementById('modalConfirmBtn').addEventListener('click', () => {
                onConfirm();
                modal.remove();
            });
        }
        
        // --- Layer Management Functions ---

        function createBlankGrid(size) {
            return Array(size * size).fill('#FFFFFF');
        }

        function getNextLayerId() {
            pixelArtState.layerCounter++;
            return `layer-${pixelArtState.layerCounter}`;
        }
        
        function deepCopyLayers(layers) {
            if (!layers) return [];
            return layers.map(layer => ({
                ...layer,
                grid: [...layer.grid]
            }));
        }

        function pushHistory(newLayers) {
            pixelArtState.history.length = pixelArtState.historyIndex + 1; 
            pixelArtState.history.push(newLayers);
            pixelArtState.historyIndex++;
            
            const MAX_HISTORY = 50;
            if (pixelArtState.history.length > MAX_HISTORY) {
                pixelArtState.history.shift(); 
                pixelArtState.historyIndex--;
            }
            updateUndoRedoButtons();
        }

        // UPDATED: Set the first layer name to "Background"
        function addLayer(name, isInitial = false) {
            const newLayerId = getNextLayerId();
            
            let layerName;
            if (isInitial) {
                layerName = 'Background'; 
            } else {
                layerName = name || `Layer ${pixelArtState.layerCounter}`;
            }

            const newLayer = {
                id: newLayerId,
                name: layerName, 
                visible: true,
                grid: createBlankGrid(pixelArtState.canvasSize),
            };

            let newHistoryLayers;

            if (pixelArtState.historyIndex >= 0) {
                const currentLayers = deepCopyLayers(pixelArtState.history[pixelArtState.historyIndex]);
                newHistoryLayers = [...currentLayers, newLayer];
            } else {
                newHistoryLayers = [newLayer];
            }
            
            pushHistory(newHistoryLayers);

            if (!isInitial || newHistoryLayers.length === 1) {
                pixelArtState.activeLayerId = newLayerId;
            }
            
            if (document.getElementById('pixelCanvas')) {
                drawGrid(pixelArtState.history[pixelArtState.historyIndex]);
                renderLayersPanel();
            }
        }

        function deleteActiveLayer() {
            const layers = deepCopyLayers(pixelArtState.history[pixelArtState.historyIndex]);
            
            if (layers.length <= 1) {
                showConfirmModal('Cannot Delete Last Layer', 
                                 'You must have at least one layer. The current layer will be cleared instead.', 
                                 () => {
                    const activeLayer = layers.find(l => l.id === pixelArtState.activeLayerId);
                    if (activeLayer) {
                        activeLayer.grid = createBlankGrid(pixelArtState.canvasSize);
                        pushHistory(layers);
                        drawGrid(layers);
                        renderLayersPanel();
                    }
                });
                return;
            }

            const indexToDelete = layers.findIndex(l => l.id === pixelArtState.activeLayerId);
            if (indexToDelete === -1) return; 

            layers.splice(indexToDelete, 1);

            let newActiveLayer = layers[Math.min(indexToDelete, layers.length - 1)];
            pixelArtState.activeLayerId = newActiveLayer.id;

            pushHistory(layers);
            drawGrid(layers);
            renderLayersPanel();
        }

        function setActiveLayer(layerId) {
            pixelArtState.activeLayerId = layerId;
            renderLayersPanel();
        }

        function toggleLayerVisibility(layerId) {
            const layers = deepCopyLayers(pixelArtState.history[pixelArtState.historyIndex]);
            const layer = layers.find(l => l.id === layerId);
            if (layer) {
                layer.visible = !layer.visible;
                pushHistory(layers);
                drawGrid(layers);
                renderLayersPanel();
            }
        }
        
        function renderLayersPanel() {
            const layersPanel = document.getElementById('layersPanel');
            if (!layersPanel) return;

            const layers = pixelArtState.history[pixelArtState.historyIndex] || [];
            
            layers.sort((a, b) => {
                const idA = parseInt(a.id.split('-')[1]);
                const idB = parseInt(b.id.split('-')[1]);
                return idB - idA; // Descending order (newest first)
            });

            layersPanel.innerHTML = layers.map(layer => {
                const isActive = layer.id === pixelArtState.activeLayerId;
                const visibilityIcon = layer.visible ? 'eye' : 'eye-off';
                
                return `
                    <div id="layer-${layer.id}" class="flex items-center justify-between p-3 rounded-lg border cursor-pointer transition ${isActive ? 'bg-pink-100 border-pink-500 shadow-md' : 'bg-gray-50 border-gray-200 hover:bg-gray-100'}"
                         onclick="setActiveLayer('${layer.id}')">
                        
                        <span class="font-medium truncate ${isActive ? 'text-pink-800' : 'text-gray-700'}">${layer.name}</span>
                        
                        <div class="flex space-x-2">
                            <!-- Visibility Toggle -->
                            <button onclick="event.stopPropagation(); toggleLayerVisibility('${layer.id}')" 
                                    class="p-1 rounded-full ${layer.visible ? 'text-green-600 hover:bg-green-100' : 'text-red-500 hover:bg-red-100'}">
                                <i data-lucide="${visibilityIcon}" class="w-4 h-4"></i>
                            </button>
                        </div>
                    </div>
                `;
            }).join('');

            lucide.createIcons();
            
            const deleteBtn = document.getElementById('deleteLayerBtn');
            if(deleteBtn) {
                const layersInHistory = pixelArtState.history[pixelArtState.historyIndex] || [];
                deleteBtn.disabled = layersInHistory.length <= 1;
                deleteBtn.classList.toggle('opacity-50', layersInHistory.length <= 1);
                deleteBtn.classList.toggle('bg-red-500', layersInHistory.length > 1);
                deleteBtn.classList.toggle('bg-gray-400', layersInHistory.length <= 1);
            }
        }

        function initializeCanvas(size) {
            pixelArtState.canvasSize = size;
            const canvas = document.getElementById('pixelCanvas');
            
            if (!canvas) return; 

            pixelArtState.canvas = canvas;
            pixelArtState.ctx = canvas.getContext('2d');
            
            const containerWidth = canvas.parentNode.clientWidth;
            const dimension = Math.min(containerWidth * 0.9, 512); 
            
            canvas.width = dimension;
            canvas.height = dimension;
            
            pixelArtState.pixelSize = dimension / pixelArtState.canvasSize;
            
            pixelArtState.layerCounter = 0;
            
            // Clear history and initialize with the first layer
            pixelArtState.history = [];
            pixelArtState.historyIndex = -1;
            addLayer('Background', true); 
            
            drawGrid(pixelArtState.history[pixelArtState.historyIndex]);
            
            if(document.getElementById('currentGridSize')) {
                document.getElementById('currentGridSize').textContent = `${size}x${size}`;
            }

            renderLayersPanel(); 
        }

        function drawGrid(layers) {
            const { ctx, canvasSize, pixelSize } = pixelArtState;
            if (!ctx || !pixelArtState.canvas) return;
            
            ctx.clearRect(0, 0, pixelArtState.canvas.width, pixelArtState.canvas.height);
            
            ctx.fillStyle = '#FFFFFF';
            ctx.fillRect(0, 0, pixelArtState.canvas.width, pixelArtState.canvas.height);
            
            layers.forEach(layer => {
                if (!layer.visible) return;
                
                const gridState = layer.grid;
                
                for (let i = 0; i < gridState.length; i++) {
                    const color = gridState[i];
                    
                    if (color !== '#FFFFFF') {
                        const x = i % canvasSize;
                        const y = Math.floor(i / canvasSize);
                        
                        ctx.fillStyle = color;
                        ctx.fillRect(x * pixelSize, y * pixelSize, pixelSize, pixelSize);
                    }
                }
            });

            ctx.strokeStyle = '#e5e7eb';
            for(let i = 0; i <= canvasSize; i++) {
                ctx.beginPath();
                ctx.moveTo(i * pixelSize, 0);
                ctx.lineTo(i * pixelSize, pixelArtState.canvas.height);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(0, i * pixelSize);
                ctx.lineTo(pixelArtState.canvas.width, i * pixelSize);
                ctx.stroke();
            }
            
            updateUndoRedoButtons();
        }

        function getGridIndex(e) {
            const rect = pixelArtState.canvas.getBoundingClientRect();
            const clientX = e.touches ? e.touches[0].clientX : e.clientX;
            const clientY = e.touches ? e.touches[0].clientY : e.clientY;

            const x = clientX - rect.left;
            const y = clientY - rect.top;

            const col = Math.floor(x / pixelArtState.pixelSize);
            const row = Math.floor(y / pixelArtState.pixelSize);
            
            if (col < 0 || col >= pixelArtState.canvasSize || row < 0 || row >= pixelArtState.canvasSize) {
                return -1; 
            }

            return row * pixelArtState.canvasSize + col;
        }

        function paintPixel(index) {
            if (index === -1) return;
            
            const currentLayers = deepCopyLayers(pixelArtState.history[pixelArtState.historyIndex]);
            const activeLayer = currentLayers.find(l => l.id === pixelArtState.activeLayerId);

            if (!activeLayer) return;
            
            const newColor = pixelArtState.isErasing ? '#FFFFFF' : pixelArtState.currentColor;

            if (activeLayer.grid[index] !== newColor) {
                activeLayer.grid[index] = newColor;
                
                pushHistory(currentLayers);
                drawGrid(currentLayers);
            }
        }

        function updateUndoRedoButtons() {
            const undoBtn = document.getElementById('undoBtn');
            const redoBtn = document.getElementById('redoBtn');

            if (undoBtn) {
                undoBtn.disabled = pixelArtState.historyIndex <= 0;
                undoBtn.classList.toggle('opacity-50', undoBtn.disabled);
                undoBtn.classList.toggle('hover:bg-gray-100', !undoBtn.disabled);
            }
            if (redoBtn) {
                redoBtn.disabled = pixelArtState.historyIndex >= pixelArtState.history.length - 1;
                redoBtn.classList.toggle('opacity-50', redoBtn.disabled);
                redoBtn.classList.toggle('hover:bg-gray-100', !redoBtn.disabled);
            }
        }
        
        function undo() {
            if (pixelArtState.historyIndex > 0) {
                pixelArtState.historyIndex--;
                const layers = pixelArtState.history[pixelArtState.historyIndex];
                drawGrid(layers);
                if (!layers.some(l => l.id === pixelArtState.activeLayerId)) {
                    pixelArtState.activeLayerId = layers[0].id; 
                }
                renderLayersPanel();
            }
        }

        function redo() {
            if (pixelArtState.historyIndex < pixelArtState.history.length - 1) {
                pixelArtState.historyIndex++;
                const layers = pixelArtState.history[pixelArtState.historyIndex];
                drawGrid(layers);
                if (!layers.some(l => l.id === pixelArtState.activeLayerId)) {
                    pixelArtState.activeLayerId = layers[0].id; 
                }
                renderLayersPanel();
            }
        }

        function handleGridSizeChange(newSize) {
             const size = parseInt(newSize, 10);
             if (size < 8 || size > 64) return; 

             showConfirmModal('Change Canvas Size', 
                `Changing the grid size to ${size}x${size} will reset all layers of your current artwork. Are you sure?`, 
                () => {
                    initializeCanvas(size); 
                    document.getElementById('gridSizeInput').value = size;
                }
             );
        }

        function setupPixelEditorEvents() {
            const canvas = document.getElementById('pixelCanvas');
            const colorPicker = document.getElementById('colorPicker');
            const eraserBtn = document.getElementById('eraserBtn');
            const penBtn = document.getElementById('penBtn');
            const sizeInput = document.getElementById('gridSizeInput');
            
            initializeCanvas(pixelArtState.canvasSize); 
            
            if (canvas) {
                const startDrawing = (e) => {
                    e.preventDefault();
                    pixelArtState.isDrawing = true;
                    paintPixel(getGridIndex(e)); 
                };
                
                const draw = (e) => {
                    e.preventDefault();
                    if (!pixelArtState.isDrawing) return;
                    paintPixel(getGridIndex(e));
                };
                
                const stopDrawing = (e) => {
                    e.preventDefault();
                    pixelArtState.isDrawing = false;
                };

                // Mouse Events
                canvas.addEventListener('mousedown', startDrawing);
                canvas.addEventListener('mousemove', draw);
                document.addEventListener('mouseup', stopDrawing); 

                // Touch Events (for mobile responsiveness)
                canvas.addEventListener('touchstart', startDrawing);
                canvas.addEventListener('touchmove', draw);
                document.addEventListener('touchend', stopDrawing);
                canvas.addEventListener('touchcancel', stopDrawing);
            }

            if (colorPicker) {
                colorPicker.value = pixelArtState.currentColor;
                colorPicker.addEventListener('input', (e) => {
                    pixelArtState.currentColor = e.target.value;
                    pixelArtState.isErasing = false; // Switch back to pen
                    eraserBtn.classList.remove('bg-gray-200');
                    eraserBtn.classList.add('bg-gray-50');
                    penBtn.classList.add('bg-gray-200');
                    penBtn.classList.remove('bg-gray-50');
                });
            }
            
            if (eraserBtn) {
                eraserBtn.addEventListener('click', () => {
                    pixelArtState.isErasing = true;
                    eraserBtn.classList.add('bg-gray-200');
                    eraserBtn.classList.remove('bg-gray-50');
                    penBtn.classList.remove('bg-gray-200');
                    penBtn.classList.add('bg-gray-50');
                });
            }
            
            if (penBtn) {
                penBtn.classList.add('bg-gray-200'); 
                penBtn.addEventListener('click', () => {
                    pixelArtState.isErasing = false;
                    penBtn.classList.add('bg-gray-200');
                    penBtn.classList.remove('bg-gray-50');
                    eraserBtn.classList.remove('bg-gray-200');
                    eraserBtn.classList.add('bg-gray-50');
                });
            }

            if (sizeInput) {
                sizeInput.value = pixelArtState.canvasSize;
                
                sizeInput.addEventListener('change', (e) => handleGridSizeChange(e.target.value));
                sizeInput.addEventListener('input', (e) => {
                    document.getElementById('currentGridSize').textContent = `${e.target.value}x${e.target.value}`;
                });
            }

            updateUndoRedoButtons();
            renderLayersPanel();
        }


        // --- Sparks.AI Core Logic (NEW) ---
        
        // Function to transform Markdown response to HTML, supporting basic elements
        function markdownToHtml(markdown) {
            let html = markdown
                .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>') // Bold
                .replace(/\*(.*?)\*/g, '<em>$1</em>') // Italic (fallback)
                .replace(/`(.*?)`/g, '<code class="bg-gray-200 p-1 rounded text-sm text-pink-700">$1</code>') // Inline code
                .replace(/\n/g, '<br>'); // Newlines

            return html;
        }

        function renderAiChatHistory() {
            const chatBox = document.getElementById('sparksAiChatBox');
            if (!chatBox) return;

            chatBox.innerHTML = aiChatHistory.map(item => {
                const isUser = item.sender === 'user';
                let content = markdownToHtml(item.text);

                // Add sources if available and not the initial welcome message
                if (item.sources && item.sources.length > 0) {
                    const citationsHtml = item.sources.map((src, index) => 
                        `<a href="${src.uri}" target="_blank" rel="noopener noreferrer" class="citation text-xs font-semibold hover:underline">[${index + 1}] ${src.title}</a>`
                    ).join('<br>');
                    content += `<div class="mt-2 text-xs opacity-80 border-t border-white/30 pt-1">Sources:<br>${citationsHtml}</div>`;
                }

                return `
                    <div class="flex ${isUser ? 'justify-end' : 'justify-start'}">
                        <div class="ai-message ${isUser ? 'user' : 'sparks'}">
                            ${content}
                        </div>
                    </div>
                `;
            }).join('');

            // Scroll to the bottom
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        async function callSparksAI(userQuery, retryCount = 0) {
            const maxRetries = 3;
            const inputField = document.getElementById('sparksAiInput');
            const sendButton = document.getElementById('sparksAiSendBtn');
            const chatBox = document.getElementById('sparksAiChatBox');
            const loadingIndicator = document.getElementById('sparksAiLoading');
            
            // Disable input/button and show loading
            if (inputField) inputField.disabled = true;
            if (sendButton) sendButton.disabled = true;
            if (loadingIndicator) loadingIndicator.classList.remove('hidden');

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                tools: [{ "google_search": {} }],
                systemInstruction: {
                    parts: [{ text: "You are Sparks.AI, a highly creative and helpful development assistant. You provide concise, friendly, and grounded answers using the latest information available. Format your responses using Markdown." }]
                },
            };

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    if (response.status === 429 && retryCount < maxRetries) {
                        const delay = Math.pow(2, retryCount) * 1000 + Math.random() * 1000;
                        await new Promise(resolve => setTimeout(resolve, delay));
                        return callSparksAI(userQuery, retryCount + 1);
                    }
                    throw new Error(`API returned status code ${response.status}`);
                }

                const result = await response.json();
                const candidate = result.candidates?.[0];

                if (candidate && candidate.content?.parts?.[0]?.text) {
                    const text = candidate.content.parts[0].text;
                    
                    let sources = [];
                    const groundingMetadata = candidate.groundingMetadata;
                    if (groundingMetadata && groundingMetadata.groundingAttributions) {
                        sources = groundingMetadata.groundingAttributions
                            .map(attribution => ({
                                uri: attribution.web?.uri,
                                title: attribution.web?.title,
                            }))
                            .filter(source => source.uri && source.title); 
                    }
                    
                    aiChatHistory.push({ sender: 'sparks', text: text, sources: sources });
                } else {
                    aiChatHistory.push({ sender: 'sparks', text: "Sorry, I couldn't generate a response for that query right now. Please try again." });
                }

            } catch (error) {
                console.error("Sparks.AI Error:", error);
                aiChatHistory.push({ sender: 'sparks', text: "An error occurred while connecting to the AI. Please check the console for details." });
            } finally {
                // Re-enable input/button and hide loading
                if (inputField) {
                    inputField.disabled = false;
                    inputField.value = ''; // Clear input field
                }
                if (sendButton) sendButton.disabled = false;
                if (loadingIndicator) loadingIndicator.classList.add('hidden');
                
                renderAiChatHistory();
            }
        }

        function sendAiMessage(event) {
            // Check if Enter key was pressed without Shift
            if (event && event.type === 'keydown' && event.key === 'Enter' && !event.shiftKey) {
                event.preventDefault(); 
            } else if (event && event.type === 'keydown' && event.key !== 'Enter') {
                return; // Only process on Enter keydown
            }
            
            const inputField = document.getElementById('sparksAiInput');
            if (!inputField || inputField.disabled) return;

            const userQuery = inputField.value.trim();
            if (!userQuery) return;

            aiChatHistory.push({ sender: 'user', text: userQuery });
            renderAiChatHistory();
            
            // Clear input and send the query
            inputField.value = ''; 
            callSparksAI(userQuery);
        }
        
        function setupAiListeners() {
            // Attach event listener for the send button
            const sendButton = document.getElementById('sparksAiSendBtn');
            if (sendButton) {
                sendButton.onclick = () => sendAiMessage({ type: 'click' });
            }

            // Attach event listener for the input field (Enter key)
            const inputField = document.getElementById('sparksAiInput');
            if (inputField) {
                inputField.addEventListener('keydown', sendAiMessage);
            }
            
            // Initial render of chat history
            renderAiChatHistory();
        }

        // --- Core Navigation and Routing ---

        function navigate(pageName, forceRender = false) {
            const contentContainer = document.getElementById('dynamic-content');
            
            if (!forceRender) {
                currentPage = pageName;
            }

            document.querySelectorAll('.nav-link').forEach(link => {
                link.classList.remove('sidebar-active', 'text-gray-100', 'font-semibold');
                link.classList.add('text-gray-400', 'font-medium');

                const linkPage = link.getAttribute('data-page');
                if (linkPage === currentPage || (linkPage === 'creations' && currentPage === 'pixel-art-editor')) {
                    link.classList.add('sidebar-active', 'text-gray-100', 'font-semibold');
                    link.classList.remove('text-gray-400', 'font-medium');
                }
            });

            // 2. Render Page Content
            if (currentPage === 'dashboard') {
                contentContainer.innerHTML = renderDashboardContent();
                setTimeout(() => setupAiListeners(), 0); // Setup AI listeners after rendering
            } else if (currentPage === 'challenges') {
                contentContainer.innerHTML = renderChallengesHubContent();
                renderChallenges(currentFilter);
            } else if (currentPage === 'leaderboard') {
                contentContainer.innerHTML = renderLeaderboardContent();
            } else if (currentPage === 'creations') {
                contentContainer.innerHTML = renderCreationsContent();
            } else if (currentPage === 'pixel-art-editor') {
                contentContainer.innerHTML = renderPixelArtEditor();
                setTimeout(() => setupPixelEditorEvents(), 0); 
            } else if (currentPage === 'social-hub') {
                contentContainer.innerHTML = renderSocialHubContent();
            }
            
            // 3. Re-initialize Lucide icons for the new content
            lucide.createIcons();
            // 4. Update the header widget
            updateHeaderWidget();
        }

        // --- Dashboard Content HTML (UPDATED) ---
        function renderDashboardContent() {
            const totalScore = challengesData.filter(c => c.status === 'Completed').reduce((sum, challenge) => sum + challenge.points, 0);
            const completedCount = challengesData.filter(c => c.status === 'Completed').length;
            const activeProjects = challengesData.filter(c => c.status === 'In Progress').length;
            
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            const nextActiveDeadlineDate = challengesData
                .filter(c => c.status === 'In Progress' || c.status === 'Upcoming')
                .map(c => new Date(c.deadline))
                .filter(d => d >= today)
                .sort((a, b) => a - b)[0];
            const nextDeadlineText = nextActiveDeadlineDate ? nextActiveDeadlineDate.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }) : 'None';

            const greetingName = userName.split(' ')[0] || 'Developer'; 

            return `
                <div class="text-left">
                    <h1 id="dashboard-greeting" class="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">
                        Welcome Back, ${greetingName}!
                    </h1>
                    <p class="text-gray-500 mt-2">A snapshot of your coding progress and next steps.</p>
                </div>

                <h2 class="text-2xl font-bold text-gray-800 mb-5 mt-10">Your Performance Metrics</h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-10">
                    
                    <div class="card p-6 bg-white rounded-xl shadow-md border border-gray-100">
                        <i data-lucide="trophy" class="w-6 h-6 text-pink-500 mb-3"></i>
                        <p class="text-lg font-semibold text-gray-500">Total Score</p>
                        <span class="text-4xl font-bold text-pink-600 mt-1 block">${totalScore.toLocaleString()}</span>
                    </div>

                    <div class="card p-6 bg-white rounded-xl shadow-md border border-gray-100">
                        <i data-lucide="check-square" class="w-6 h-6 text-green-500 mb-3"></i>
                        <p class="text-lg font-semibold text-gray-500">Completed</p>
                        <span class="text-4xl font-bold text-gray-800 mt-1 block">${completedCount}</span>
                    </div>

                    <div class="card p-6 bg-white rounded-xl shadow-md border border-gray-100">
                        <i data-lucide="layers" class="w-6 h-6 text-blue-500 mb-3"></i>
                        <p class="text-lg font-semibold text-gray-500">Active Projects</p>
                        <span class="text-4xl font-bold text-blue-600 mt-1 block">${activeProjects}</span>
                    </div>

                    <div class="card p-6 bg-white rounded-xl shadow-md border border-gray-100">
                        <i data-lucide="clock" class="w-6 h-6 text-red-500 mb-3"></i>
                        <p class="text-lg font-semibold text-gray-500">Next Deadline</p>
                        <span class="text-2xl font-bold text-red-500 mt-1 block">${nextDeadlineText}</span>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Activity Feed (2/3 width) -->
                    <div class="lg:col-span-2 bg-white p-8 rounded-xl shadow-lg border border-gray-100">
                        <h2 class="text-2xl font-bold text-gray-800 border-b pb-4 mb-5">Recent Activity Feed</h2>
                        
                        <div class="space-y-5">
                            <!-- Activity Item 1 -->
                            <div class="flex items-start space-x-4 border-b pb-4">
                                <div class="p-3 bg-pink-100 rounded-full text-pink-600">
                                    <i data-lucide="code-square" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <p class="font-semibold text-gray-800">Challenge Started: <span class="text-pink-600">Flappy Bird Collision Fix</span></p>
                                    <p class="text-sm text-gray-500">You began working on the physics refactoring 1 hour ago.</p>
                                </div>
                                <span class="ml-auto text-xs text-gray-400 mt-1 hidden sm:block">Just now</span>
                            </div>

                            <!-- Activity Item 2 -->
                            <div class="flex items-start space-x-4 border-b pb-4">
                                <div class="p-3 bg-blue-100 rounded-full text-blue-600">
                                    <i data-lucide="database" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <p class="font-semibold text-gray-800">Progress Update: <span class="text-blue-600">Implement Global Leaderboard</span></p>
                                    <p class="text-sm text-gray-500">Firestore integration is 75% complete.</p>
                                </div>
                                <span class="ml-auto text-xs text-gray-400 mt-1 hidden sm:block">3 hours ago</span>
                            </div>

                            <!-- Activity Item 3 -->
                            <div class="flex items-start space-x-4">
                                <div class="p-3 bg-green-100 rounded-full text-green-600">
                                    <i data-lucide="check-circle" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <p class="font-semibold text-gray-800">Challenge Complete: <span class="text-green-600">Refactor Game Loop</span></p>
                                    <p class="text-sm text-gray-500">800 points added to your score.</p>
                                </div>
                                <span class="ml-auto text-xs text-gray-400 mt-1 hidden sm:block">Yesterday</span>
                            </div>
                        </div>

                        <button onclick="navigate('challenges')" class="mt-6 w-full py-2 text-blue-600 font-semibold border border-blue-200 rounded-lg hover:bg-blue-50 transition">
                            View All Active Challenges
                        </button>
                    </div>

                    <!-- Sparks.AI Chat (1/3 width) - NEW -->
                    <div class="lg:col-span-1 p-0 rounded-xl shadow-lg border border-gray-100 overflow-hidden">
                        <div class="ai-chat-container">
                            <div class="p-4 bg-gray-900 rounded-t-xl flex items-center space-x-3">
                                <i data-lucide="zap" class="w-6 h-6 text-pink-500"></i>
                                <h2 class="text-xl font-bold text-white">Sparks.AI</h2>
                                <span class="text-xs text-gray-400 ml-auto bg-gray-700 px-2 py-1 rounded-full">Grounded</span>
                            </div>

                            <!-- Message Area -->
                            <div id="sparksAiChatBox" class="ai-chat-messages space-y-3">
                                <!-- Messages injected by renderAiChatHistory() -->
                            </div>
                            
                            <!-- Loading Indicator -->
                            <div id="sparksAiLoading" class="hidden p-2 text-center text-sm text-pink-600 font-semibold bg-pink-50">
                                <div class="animate-spin inline-block w-4 h-4 border-2 border-pink-500 border-r-transparent rounded-full mr-2"></div>
                                Sparks.AI is thinking...
                            </div>

                            <!-- Input Area -->
                            <div class="ai-chat-input flex space-x-3 bg-gray-50 border-t border-gray-200">
                                <textarea id="sparksAiInput" placeholder="Ask Sparks.AI anything..." rows="1" 
                                    class="flex-grow p-2.5 border border-gray-300 rounded-lg focus:ring-pink-500 focus:border-pink-500 resize-none overflow-hidden" 
                                    oninput="this.style.height = 'auto'; this.style.height = (this.scrollHeight) + 'px';"></textarea>
                                <button id="sparksAiSendBtn" class="p-3 bg-pink-600 text-white rounded-lg hover:bg-pink-700 transition self-end">
                                    <i data-lucide="send" class="w-5 h-5"></i>
                                </button>
                            </div>
                        </div>
                    </div>

                </div>
            `;
        }

        // --- Challenges Hub Content HTML (Unchanged) ---
        function renderChallengesHubContent() {
             const completedCount = challengesData.filter(c => c.status === 'Completed').length;
             const activeCount = challengesData.filter(c => c.status === 'In Progress').length;
             const totalScore = challengesData.filter(c => c.status === 'Completed').reduce((sum, challenge) => sum + challenge.points, 0);
            
            return `
                <div class="text-left">
                    <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">
                        Challenges Hub
                    </h1>
                    <p class="text-gray-500 mt-2">View active tasks, deadlines, and team missions.</p>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-10 mt-10">
                    
                    <div class="p-6 bg-white rounded-xl shadow-lg border border-gray-100 col-span-1 md:col-span-2">
                        <p class="text-lg font-semibold text-gray-500">Total Score</p>
                        <div class="flex items-end justify-between mt-1">
                            <span class="text-5xl font-bold text-pink-600">${totalScore.toLocaleString()}</span>
                            <span class="text-sm text-green-500 font-medium flex items-center">
                                <i data-lucide="trending-up" class="w-4 h-4 mr-1"></i>
                                0% this week
                            </span>
                        </div>
                    </div>

                    <div class="p-6 bg-white rounded-xl shadow-lg border border-gray-100">
                        <p class="text-lg font-semibold text-gray-500">Completed</p>
                        <div class="flex items-end mt-1">
                            <span class="text-5xl font-bold text-gray-800">${completedCount}</span>
                        </div>
                    </div>

                    <div class="p-6 bg-white rounded-xl shadow-lg border border-gray-100">
                        <p class="text-lg font-semibold text-gray-500">Active</p>
                        <div class="flex items-end mt-1">
                            <span class="text-5xl font-bold text-blue-600">${activeCount}</span>
                        </div>
                    </div>
                </div>

                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg">
                    
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center border-b pb-5 mb-5">
                        <h2 class="text-3xl font-bold text-gray-800 mb-3 sm:mb-0">Active & Upcoming Missions</h2>
                        <div class="flex space-x-2 text-sm font-medium">
                            <button data-filter="active" class="filter-btn px-4 py-2 bg-pink-600 text-white rounded-lg shadow-md hover:bg-pink-700 transition">Active (${activeCount})</button>
                            <button data-filter="completed" class="filter-btn px-4 py-2 bg-gray-100 text-gray-700 rounded-lg hover:bg-gray-200 transition">Completed (${completedCount})</button>
                            <button data-filter="upcoming" class="filter-btn px-4 py-2 bg-gray-100 text-gray-700 rounded-lg hover:bg-gray-200 transition">Upcoming</button>
                            <button data-filter="all" class="filter-btn px-4 py-2 bg-gray-100 text-gray-700 rounded-lg hover:bg-gray-200 transition">All</button>
                        </div>
                    </div>

                    <div id="challenge-list" class="challenge-list space-y-4 pr-1">
                        <!-- Challenges will be injected by JavaScript here -->
                    </div>
                </div>
            `;
        }

        // --- Leaderboard Content HTML (Unchanged) ---
        function renderLeaderboardContent() {
            const user = leaderboardData[0]; 
            
            const listHtml = `
                <li class="p-4 flex items-center justify-between border-b last:border-b-0 bg-pink-50 border-r-4 border-pink-600 rounded-xl transition duration-150">
                    <div class="flex items-center space-x-4">
                        <span class="text-2xl font-extrabold text-yellow-500 w-8 text-center">1</span>
                        <div class="flex flex-col">
                            <span class="font-semibold text-pink-600">${user.name} (You)</span>
                            <span class="text-sm text-gray-500">${user.progress} Challenges Solved</span>
                        </div>
                    </div>
                    <div class="text-right">
                        <span class="text-3xl font-bold text-pink-600">${user.score.toLocaleString()}</span>
                        <span class="text-xs text-gray-400 block">Total Score</span>
                    </div>
                </li>
            `;

            return `
                <div class="text-left">
                    <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">
                        Global Leaderboard
                    </h1>
                    <p class="text-gray-500 mt-2">You are currently the only champion!</p>
                </div>

                <div class="mt-10 bg-white p-8 rounded-xl shadow-lg border border-gray-100">
                    <div class="flex justify-between items-center mb-6 border-b pb-4">
                        <h2 class="text-2xl font-bold text-gray-800">Top Coders</h2>
                        <span class="text-sm font-medium text-gray-500">Total Participants: 1</span>
                    </div>
                    
                    <ul class="space-y-2">
                        ${listHtml}
                    </ul>

                    <div class="mt-8 p-4 bg-gray-50 rounded-lg text-center">
                        <p class="font-medium text-gray-700">Your Current Rank: <span class="text-pink-600 font-bold">#1</span></p>
                    </div>
                </div>
            `;
        }
        
        // --- Social Hub Content (Unchanged) ---
        function renderSocialHubContent() {
            const friends = getFriendsList();
            
            const friendListHtml = friends.length > 0 ? friends.map(friend => `
                <div class="flex items-center justify-between p-4 bg-gray-50 rounded-lg">
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 bg-pink-500 rounded-full flex items-center justify-center text-white font-bold text-sm">${friend.name.split(' ').map(n => n[0]).join('').toUpperCase()}</div>
                        <div>
                            <p class="font-semibold text-gray-800">${friend.name}</p>
                            <p class="text-xs text-gray-500">${friend.id}</p>
                        </div>
                    </div>
                    <span class="text-green-500 font-medium text-sm flex items-center">
                        <i data-lucide="check-circle" class="w-4 h-4 mr-1"></i> Friend
                    </span>
                </div>
            `).join('') : `
                <div class="text-center p-8 bg-gray-100 rounded-lg text-gray-500">
                    <i data-lucide="user-plus" class="w-8 h-8 mx-auto mb-3"></i>
                    <p>Your friends list is empty. Add a friend using their ID above!</p>
                </div>
            `;

            return `
                <div class="text-left">
                    <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">
                        Social Hub
                    </h1>
                    <p class="text-gray-500 mt-2">Connect and collaborate with your friends.</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 mt-10">
                    
                    <!-- Your ID & Add Friend -->
                    <div class="lg:col-span-1 p-6 bg-white rounded-xl shadow-lg h-fit">
                        <h2 class="text-xl font-bold mb-4 border-b pb-3 text-gray-800">Your Connection ID</h2>
                        
                        <div class="p-4 bg-pink-50 rounded-lg border border-pink-200 mb-6">
                            <p class="text-sm text-pink-600 font-medium">Share this ID with friends:</p>
                            <p id="yourUserId" class="text-2xl font-mono font-bold text-pink-800 break-all mt-1">${uniqueUserId}</p>
                        </div>

                        <h2 class="text-xl font-bold mb-4 border-b pb-3 text-gray-800">Add Friend by ID</h2>
                        <div class="space-y-4">
                            <input type="text" id="friendIdInput" placeholder="Enter Friend's ID (e.g., XYZ123ABC)" 
                                class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 uppercase">
                            
                            <p id="friend-message" class="text-sm font-medium h-5"></p>

                            <button onclick="addFriendById()" class="w-full py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 transition">
                                Send Friend Request
                            </button>
                            
                            <p class="text-xs text-gray-400 pt-2 text-center">Mock IDs you can use: XYZ123ABC, TUV789WXY</p>
                        </div>
                    </div>

                    <!-- Friend List -->
                    <div class="lg:col-span-2 p-6 bg-white rounded-xl shadow-lg">
                        <h2 class="text-xl font-bold mb-5 border-b pb-3 text-gray-800">My Friends (${friends.length})</h2>
                        <div class="space-y-3 max-h-[500px] overflow-y-auto pr-2">
                            ${friendListHtml}
                        </div>
                    </div>
                </div>
            `;
        }

        // --- Creations Content HTML (Unchanged) ---
        function renderCreationsContent() {
            return `
                <div class="text-left">
                    <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">
                        Creations
                    </h1>
                    <p class="text-gray-500 mt-2">Choose your next creative project.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mt-10">
                    <!-- Option 1: Create a Game -->
                    <div onclick="console.log('Game creation path selected - Placeholder')" class="card bg-white p-8 rounded-xl shadow-lg border border-gray-100 cursor-pointer transition duration-300 hover:shadow-xl hover:scale-[1.01]">
                        <div class="p-4 bg-blue-100 rounded-full w-fit mb-4 text-blue-600">
                            <i data-lucide="gamepad-2" class="w-8 h-8"></i>
                        </div>
                        <h2 class="text-2xl font-bold text-gray-800 mb-2">Create a Game</h2>
                        <p class="text-gray-500">Jumpstart your game development with starter templates and engine tools.</p>
                        <button class="mt-4 px-6 py-2 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 transition">
                            Start Game Project
                        </button>
                    </div>

                    <!-- Option 2: Pixel Art Editor -->
                    <div onclick="navigate('pixel-art-editor')" class="card bg-white p-8 rounded-xl shadow-lg border border-gray-100 cursor-pointer transition duration-300 hover:shadow-xl hover:scale-[1.01]">
                        <div class="p-4 bg-pink-100 rounded-full w-fit mb-4 text-pink-600">
                            <i data-lucide="pencil-ruler" class="w-8 h-8"></i>
                        </div>
                        <h2 class="text-2xl font-bold text-gray-800 mb-2">Create Pixel Art</h2>
                        <p class="text-gray-500">Design sprites, tiles, and 8-bit characters using our integrated editor.</p>
                        <button class="mt-4 px-6 py-2 bg-pink-600 text-white rounded-lg font-semibold hover:bg-pink-700 transition">
                            Open Pixel Editor
                        </button>
                    </div>
                </div>
            `;
        }

        // --- Pixel Art Editor Content (Unchanged) ---
        function renderPixelArtEditor() {
            pixelArtState.history = [];
            pixelArtState.historyIndex = -1;
            pixelArtState.isErasing = false;
            
            return `
                <div class="flex items-center space-x-4 mb-8">
                    <button onclick="navigate('creations')" class="p-2 text-gray-500 hover:bg-gray-200 rounded-full transition">
                        <i data-lucide="arrow-left" class="w-6 h-6"></i>
                    </button>
                    <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">
                        Pixel Art Editor
                    </h1>
                </div>
                
                <div class="flex flex-col lg:flex-row gap-8">
                    
                    <!-- Controls Panel (Left/Top) -->
                    <div class="lg:w-1/4 w-full p-6 bg-white rounded-xl shadow-lg h-fit order-2 lg:order-1">
                        
                        <!-- Tools Section -->
                        <h2 class="text-xl font-bold mb-4 border-b pb-3 text-gray-800">Editor Tools</h2>

                        <!-- 1. Color Picker -->
                        <div class="mb-5">
                            <label class="block text-sm font-medium text-gray-700 mb-2">Primary Color</label>
                            <div class="flex items-center space-x-4">
                                <input type="color" id="colorPicker" value="${pixelArtState.currentColor}" class="w-10 h-10 border-2 border-gray-300 rounded-full cursor-pointer">
                                <span class="text-xl font-mono text-gray-800">${pixelArtState.currentColor}</span>
                            </div>
                        </div>
                        
                        <!-- Tool Selector (Pen/Eraser) -->
                        <div class="mb-5 flex space-x-2">
                            <button id="penBtn" class="flex-1 flex items-center justify-center p-3 bg-gray-50 rounded-lg font-semibold text-gray-700 hover:bg-gray-100 transition">
                                <i data-lucide="pen-tool" class="w-5 h-5 mr-2"></i> Pen
                            </button>
                            <button id="eraserBtn" class="flex-1 flex items-center justify-center p-3 bg-gray-50 rounded-lg font-semibold text-gray-700 hover:bg-gray-100 transition">
                                <i data-lucide="eraser" class="w-5 h-5 mr-2"></i> Eraser
                            </button>
                        </div>
                        
                        <!-- 2. Undo/Redo Buttons -->
                        <div class="mb-5 border-t pt-5">
                            <label class="block text-sm font-medium text-gray-700 mb-2">History</label>
                            <div class="flex space-x-2">
                                <button id="undoBtn" onclick="undo()" class="flex-1 flex items-center justify-center p-3 bg-white border border-gray-300 rounded-lg font-semibold text-gray-700 transition" disabled>
                                    <i data-lucide="undo-2" class="w-5 h-5 mr-2"></i> Undo
                                </button>
                                <button id="redoBtn" onclick="redo()" class="flex-1 flex items-center justify-center p-3 bg-white border border-gray-300 rounded-lg font-semibold text-gray-700 transition" disabled>
                                    <i data-lucide="redo-2" class="w-5 h-5 mr-2"></i> Redo
                                </button>
                            </div>
                        </div>
                        
                        <!-- Layers Section -->
                        <div class="mb-5 border-t pt-5">
                            <h2 class="text-xl font-bold mb-4 text-gray-800">Layers</h2>
                            
                            <div class="flex space-x-2 mb-4">
                                <button onclick="addLayer()" class="flex-1 flex items-center justify-center p-3 bg-green-600 text-white rounded-lg font-semibold hover:bg-green-700 transition">
                                    <i data-lucide="plus" class="w-5 h-5 mr-2"></i> New Layer
                                </button>
                                <button onclick="deleteActiveLayer()" id="deleteLayerBtn" class="flex-1 flex items-center justify-center p-3 bg-red-500 text-white rounded-lg font-semibold hover:bg-red-600 transition">
                                    <i data-lucide="trash-2" class="w-5 h-5 mr-2"></i> Delete Active
                                </button>
                            </div>

                            <div id="layersPanel" class="space-y-2 max-h-48 overflow-y-auto pr-2 border border-gray-200 p-2 rounded-lg">
                                <!-- Layer list injected here by renderLayersPanel() -->
                            </div>
                        </div>


                        <!-- 3. Size Control -->
                        <div class="mb-5 border-t pt-5">
                            <label class="block text-sm font-medium text-gray-700 mb-2">Grid Size: <span id="currentGridSize" class="font-bold text-pink-600">16x16</span></label>
                            <!-- Range for 8, 16, 24, 32, 40, 48, 56, 64 -->
                            <input type="range" id="gridSizeInput" min="8" max="64" step="8" value="${pixelArtState.canvasSize}" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer range-lg">
                            <div class="flex justify-between text-xs text-gray-500 mt-1">
                                <span>8x8</span>
                                <span>64x64</span>
                            </div>
                        </div>
                        
                        <!-- 4. Save/Export (Placeholder) -->
                        <div class="border-t pt-5">
                            <button onclick="console.log('Exporting image');" class="w-full py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 transition">
                                Export Image (PNG)
                            </button>
                        </div>

                    </div>

                    <!-- Canvas Area (Right/Bottom) -->
                    <div class="lg:w-3/4 w-full flex justify-center items-start p-2 lg:p-6 bg-gray-100 rounded-xl order-1 lg:order-2">
                        <div class="shadow-2xl rounded-lg overflow-hidden border-8 border-white bg-white w-full max-w-lg aspect-square">
                            <canvas id="pixelCanvas" class="w-full h-full block cursor-crosshair"></canvas>
                        </div>
                    </div>
                </div>
            `;
        }
        
        // --- Challenge Rendering Logic (Unchanged) ---
        function calculateDaysLeft(deadlineDate) {
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            const deadline = new Date(deadlineDate);
            deadline.setHours(0, 0, 0, 0);
            
            const diffTime = deadline - today;
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            
            if (diffDays < 0) return 'Overdue';
            if (diffDays === 0) return 'Today!';
            if (diffDays === 1) return '1 Day Left';
            return `${diffDays} Days Left`;
        }

        function getChallengeClasses(challenge) {
            let statusClasses = {};
            let borderClasses = '';
            let buttonClasses = 'px-4 py-2 text-white rounded-lg shadow-md transition';
            let buttonText = 'View Task';
            let isDisabled = false;

            if (challenge.status === 'Completed') {
                statusClasses = { bg: 'bg-green-100', text: 'text-green-700' };
                borderClasses = 'border-l-completed hover:border-l-completed-hover opacity-75';
                buttonClasses = 'px-4 py-2 bg-green-600 text-white rounded-lg shadow-md';
                buttonText = 'Review';
            } else if (challenge.status === 'Upcoming') {
                statusClasses = { bg: 'bg-gray-100', text: 'text-gray-500' };
                borderClasses = 'border-l-gray-300 hover:border-l-gray-500 opacity-70';
                buttonClasses = 'px-4 py-2 bg-gray-400 text-white rounded-lg cursor-not-allowed';
                buttonText = 'Starts Soon';
                isDisabled = true;
            } else { // In Progress
                const priority = challenge.priority;
                if (priority === 'high') {
                    statusClasses = { bg: 'bg-pink-100', text: 'text-pink-700' };
                    borderClasses = 'border-l-high hover:border-l-high-hover';
                    buttonClasses = 'px-4 py-2 bg-pink-600 text-white rounded-lg shadow-md hover:bg-pink-700';
                } else if (priority === 'medium') {
                    statusClasses = { bg: 'bg-yellow-100', text: 'text-yellow-700' };
                    borderClasses = 'border-l-medium hover:border-l-medium-hover';
                    buttonClasses = 'px-4 py-2 bg-yellow-600 text-white rounded-lg shadow-md hover:bg-yellow-700';
                } else { // low priority
                    statusClasses = { bg: 'bg-blue-100', text: 'text-blue-700' };
                    borderClasses = 'border-l-low hover:border-l-low-hover';
                    buttonClasses = 'px-4 py-2 bg-blue-600 text-white rounded-lg shadow-md hover:bg-blue-700';
                }
            }

            return { statusClasses, borderClasses, buttonClasses, buttonText, isDisabled };
        }

        function renderChallenges(filter) {
            currentFilter = filter; 
            const listContainer = document.getElementById('challenge-list');
            if (!listContainer) return;

            listContainer.innerHTML = ''; 

            let filteredChallenges = challengesData;

            if (filter !== 'all') {
                const statusMap = {
                    'active': 'In Progress',
                    'completed': 'Completed',
                    'upcoming': 'Upcoming'
                };
                const targetStatus = statusMap[filter];
                if (targetStatus) {
                    filteredChallenges = challengesData.filter(c => c.status === targetStatus);
                }
            }

            if (filteredChallenges.length === 0) {
                listContainer.innerHTML = `
                    <div class="text-center p-10 text-gray-500">
                        <i data-lucide="inbox" class="w-10 h-10 mx-auto mb-3"></i>
                        <p class="text-xl font-semibold">No challenges found.</p>
                        <p>Try changing your filter settings or checking back later!</p>
                    </div>
                `;
                lucide.createIcons();
                return;
            }

            filteredChallenges.forEach(challenge => {
                const { statusClasses, borderClasses, buttonClasses, buttonText, isDisabled } = getChallengeClasses(challenge);
                
                const daysLeftText = challenge.status === 'Completed' 
                    ? 'Completed' 
                    : calculateDaysLeft(challenge.deadline);

                const deadlineTextColor = (daysLeftText.includes('Day') || daysLeftText === 'Today!' || daysLeftText === 'Overdue') && challenge.status !== 'Completed' ? 'text-red-500' : 'text-gray-800';
                
                const cardHtml = `
                    <div class="challenge-card bg-white p-5 ${borderClasses}" data-id="${challenge.id}">
                        <div class="flex flex-col md:flex-row justify-between items-start md:items-center">
                            <div class="mb-3 md:mb-0">
                                <span class="status-badge ${statusClasses.bg} ${statusClasses.text}">${challenge.status}</span>
                                <h3 class="text-2xl font-bold text-gray-800 mt-2">${challenge.title}</h3>
                                <p class="text-gray-500 mt-1">${challenge.description}</p>
                            </div>
                            <div class="text-right flex items-center space-x-6 mt-4 md:mt-0">
                                <div class="hidden sm:block">
                                    <p class="text-sm text-gray-500">Points</p>
                                    <p class="text-xl font-bold text-pink-600">${challenge.points.toLocaleString()}</p>
                                </div>
                                <div>
                                    <p class="text-sm text-gray-500">${challenge.status === 'Upcoming' ? 'Starts' : 'Deadline'}</p>
                                    <p class="text-xl font-bold ${deadlineTextColor}">${daysLeftText}</p>
                                </div>
                                <button 
                                    onclick="event.stopPropagation(); viewChallengeDetails('${challenge.id}');" 
                                    class="${buttonClasses}"
                                    ${isDisabled ? 'disabled' : ''}
                                >
                                    ${buttonText}
                                </button>
                            </div>
                        </div>
                    </div>
                `;
                listContainer.insertAdjacentHTML('beforeend', cardHtml);
            });
            
            setupFilterListeners();
            lucide.createIcons();
        }

        function viewChallengeDetails(id) {
            const challenge = challengesData.find(c => c.id === id);
            if (challenge) {
                console.log(`Viewing details for Challenge: ${challenge.title} (ID: ${id})`);
                
                const messageBox = document.createElement('div');
                messageBox.className = 'fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50';
                messageBox.innerHTML = `
                    <div class="bg-white p-6 rounded-xl shadow-2xl w-96 max-w-full">
                        <h4 class="text-xl font-bold mb-2 text-gray-900">${challenge.title}</h4>
                        <p class="text-gray-700">${challenge.description}</p>
                        <p class="mt-4 text-sm font-medium">Status: <span class="text-pink-600">${challenge.status}</span> | Points: ${challenge.points}</p>
                        <button onclick="this.parentNode.parentNode.remove()" class="mt-5 w-full py-2 bg-pink-600 text-white rounded-lg hover:bg-pink-700 transition">
                            Close
                        </button>
                    </div>
                `;
                document.body.appendChild(messageBox);
            }
        }

        // --- Setup and Listeners (Unchanged) ---
        function setupFilterListeners() {
            document.querySelectorAll('.filter-btn').forEach(button => {
                 const newButton = button.cloneNode(true);
                 button.parentNode.replaceChild(newButton, button);
            });

            document.querySelectorAll('.filter-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const filter = this.getAttribute('data-filter');
                    
                    document.querySelectorAll('.filter-btn').forEach(btn => {
                        btn.classList.remove('bg-pink-600', 'text-white', 'shadow-md');
                        btn.classList.add('bg-gray-100', 'text-gray-700');
                    });
                    this.classList.remove('bg-gray-100', 'text-gray-700');
                    this.classList.add('bg-pink-600', 'text-white', 'shadow-md');

                    renderChallenges(filter);
                });
            });
        }

        // Initialize: Attach listeners to the sidebar links
        document.querySelectorAll('.nav-link').forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                const page = e.currentTarget.getAttribute('data-page');
                navigate(page);
            });
        });

        // Initialize the application to the Dashboard view on load
        window.onload = function() {
            updateUserInfo();
            navigate('dashboard');
        };

    </script>
</body>
</html>
