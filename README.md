<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Treadmill Pace Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .backdrop-blur-lg {
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
        }
    </style>
</head>
<body class="bg-gradient-to-br from-gray-900 to-black text-white min-h-screen">
    <div id="app" class="p-4 sm:p-6 lg:p-8">
        <div class="max-w-4xl mx-auto">
            <!-- Header -->
            <header class="flex items-center mb-8">
                <div id="dumbbell-icon-container"></div>
                <h1 class="text-3xl sm:text-4xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-gray-100 to-gray-400">Treadmill Pace Calculator</h1>
            </header>
            
            <!-- Global Error Message -->
            <div id="error-message" class="hidden bg-red-500/20 border border-red-500 text-red-300 px-4 py-3 rounded-lg mb-6 text-sm" role="alert">
                <p></p>
            </div>
            
            <!-- Add Workout Form -->
            <div class="bg-white/5 backdrop-blur-lg p-6 rounded-2xl border border-white/10 shadow-lg mb-8">
                <h2 class="text-xl font-semibold mb-4 text-gray-300">Log New Workout</h2>
                <form id="workout-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                    <!-- Distance -->
                    <div class="flex flex-col">
                        <label for="distance" class="text-sm font-medium text-gray-400 mb-1">Distance (mi)</label>
                        <input id="distance" type="number" step="0.01" placeholder="e.g., 5.0" class="bg-gray-700/50 border border-gray-600 rounded-md px-3 py-2 text-white placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 transition">
                    </div>
                    <!-- Duration -->
                    <div class="flex flex-col">
                        <label for="duration" class="text-sm font-medium text-gray-400 mb-1">Duration</label>
                        <input id="duration" type="text" placeholder="HH:MM:SS" class="bg-gray-700/50 border border-gray-600 rounded-md px-3 py-2 text-white placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 transition">
                    </div>
                    <!-- Calories -->
                    <div class="flex flex-col">
                        <label for="calories" class="text-sm font-medium text-gray-400 mb-1">Calories (Optional)</label>
                        <input id="calories" type="number" placeholder="e.g., 820" class="bg-gray-700/50 border border-gray-600 rounded-md px-3 py-2 text-white placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 transition">
                    </div>
                    <!-- Submit Button -->
                    <div class="flex flex-col justify-end">
                        <button type="submit" class="w-full bg-gradient-to-r from-cyan-500 to-blue-600 hover:from-cyan-600 hover:to-blue-700 text-white font-bold py-2 px-4 rounded-md transition duration-300 ease-in-out transform hover:scale-105 shadow-lg">
                            Add Workout
                        </button>
                    </div>
                </form>
            </div>

            <!-- Totals Section -->
             <div class="mb-8">
                <h3 class="text-lg font-semibold text-gray-300 mb-3">Lifetime Totals</h3>
                <div class="grid grid-cols-2 md:grid-cols-3 gap-4 text-center">
                    <div class="bg-white/5 backdrop-blur-sm p-4 rounded-xl border border-white/10">
                        <p class="text-sm text-gray-400">Total Distance</p>
                        <p id="total-distance" class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-blue-400">0.00 mi</p>
                    </div>
                    <div class="bg-white/5 backdrop-blur-sm p-4 rounded-xl border border-white/10">
                        <p class="text-sm text-gray-400">Total Time</p>
                        <p id="total-duration" class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-blue-400">00:00:00</p>
                    </div>
                    <div class="bg-white/5 backdrop-blur-sm p-4 rounded-xl border border-white/10 col-span-2 md:col-span-1">
                        <p class="text-sm text-gray-400">Total Calories</p>
                        <p id="total-calories" class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-blue-400">0</p>
                    </div>
                </div>
            </div>

            <!-- AI Insights Section -->
            <div class="mb-8">
                <h2 class="text-xl font-semibold mb-4 text-gray-300">✨ AI Insights</h2>
                <div class="bg-white/5 backdrop-blur-lg p-6 rounded-2xl border border-white/10 shadow-lg">
                    <button id="generate-insights-btn" class="w-full bg-gradient-to-r from-purple-500 to-indigo-600 hover:from-purple-600 hover:to-indigo-700 text-white font-bold py-2 px-4 rounded-md transition duration-300 ease-in-out transform hover:scale-105 shadow-lg disabled:opacity-50 disabled:cursor-not-allowed disabled:scale-100">
                        Generate Workout Summary & Suggestion
                    </button>
                    <div id="insights-output">
                        <p id="ai-helper-text" class="text-gray-400 text-sm mt-3 text-center">Log at least 3 workouts to unlock AI insights!</p>
                        <p id="ai-error" class="text-red-400 text-sm mt-3 hidden"></p>
                        <div id="ai-loading" class="text-center p-4 mt-4 text-gray-400 hidden">
                            <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-cyan-400 mx-auto"></div>
                            <p class="mt-2">Thinking...</p>
                        </div>
                        <div id="ai-insights-content" class="mt-4 p-4 bg-gray-700/30 rounded-lg border border-gray-600 hidden">
                           <p class="text-gray-200 whitespace-pre-wrap"></p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Workout History -->
            <div>
                <h2 class="text-xl font-semibold mb-4 text-gray-300">Workout History</h2>
                <div id="workout-history-container">
                    <div id="history-loading" class="text-center text-gray-400">Initializing...</div>
                    <div id="history-empty" class="bg-gray-800/50 p-6 rounded-xl text-center text-gray-400 border border-white/10 hidden">
                        <p>No workouts logged yet. Add your first one above!</p>
                    </div>
                    <div id="history-list" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, doc, deleteDoc, query, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- App State ---
        let db;
        let auth;
        let userId;
        let workouts = [];

        // --- DOM Elements ---
        const errorMessage = document.getElementById('error-message');
        const workoutForm = document.getElementById('workout-form');
        const distanceInput = document.getElementById('distance');
        const durationInput = document.getElementById('duration');
        const caloriesInput = document.getElementById('calories');
        const historyContainer = document.getElementById('workout-history-container');
        const historyLoading = document.getElementById('history-loading');
        const historyEmpty = document.getElementById('history-empty');
        const historyList = document.getElementById('history-list');
        const totalDistanceEl = document.getElementById('total-distance');
        const totalDurationEl = document.getElementById('total-duration');
        const totalCaloriesEl = document.getElementById('total-calories');
        const insightsBtn = document.getElementById('generate-insights-btn');
        const aiHelperText = document.getElementById('ai-helper-text');
        const aiError = document.getElementById('ai-error');
        const aiLoading = document.getElementById('ai-loading');
        const aiInsightsContent = document.getElementById('ai-insights-content');

        // --- SVG Icons ---
        const dumbbellIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-8 w-8 mr-3 text-cyan-400"><path d="M14.4 14.4 9.6 9.6" /><path d="M18.657 21.485a2 2 0 1 1-2.829-2.828l-1.767 1.768a2 2 0 1 1-2.829-2.829l6.364-6.364a2 2 0 1 1 2.829 2.829l-1.768 1.767a2 2 0 1 1 2.828 2.829z" /><path d="m21.5 21.5-1.4-1.4" /><path d="M3.9 3.9 2.5 2.5" /><path d="M6.343 12.343a2 2 0 1 1-2.829-2.829l1.768-1.767a2 2 0 1 1 2.829-2.829l6.363 6.364a2 2 0 1 1-2.829 2.829l-1.767-1.768a2 2 0 1 1-2.829 2.829z" /></svg>`;
        const runIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-4 w-4 text-gray-400"><path d="M16 13a3 3 0 1 0-3-3 3 3 0 0 0 3 3z"/><path d="M13 13V2a3 3 0 0 0-6 0v4"/><path d="m11.1 13.7-2.5 2.5a4.4 4.4 0 0 0 0 6.2l.2.2a4.4 4.4 0 0 0 6.2 0l2-2"/><path d="m5.5 18 3-3"/></svg>`;
        const clockIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-4 w-4 text-gray-400"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>`;
        const flameIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-4 w-4 text-gray-400"><path d="M8.5 14.5A2.5 2.5 0 0 0 11 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 1 1-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 0 0 2.5 2.5z"/></svg>`;
        const gaugeIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-4 w-4 text-gray-400"><path d="m12 14 4-4"/><path d="M3.34 19a10 10 0 1 1 17.32 0"/></svg>`;
        const trashIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-5 w-5"><path d="M3 6h18" /><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2" /><line x1="10" y1="11" x2="10" y2="17" /><line x1="14" y1="11" x2="14" y2="17" /></svg>`;

        document.getElementById('dumbbell-icon-container').innerHTML = dumbbellIcon;

        // --- Helper Functions ---
        const showError = (msg) => {
            errorMessage.querySelector('p').textContent = msg;
            errorMessage.classList.remove('hidden');
        };

        const hideError = () => {
            errorMessage.classList.add('hidden');
        };
        
        const parseDurationToSeconds = (durationStr) => {
            const parts = durationStr.split(':').map(Number);
            if (parts.some(isNaN)) return null;
            let seconds = 0;
            if (parts.length === 3) seconds = parts[0] * 3600 + parts[1] * 60 + parts[2];
            else if (parts.length === 2) seconds = parts[0] * 60 + parts[1];
            else if (parts.length === 1) seconds = parts[0];
            else return null;
            return seconds;
        };

        const formatDuration = (totalSeconds) => {
            if (totalSeconds === null || totalSeconds === undefined || isNaN(totalSeconds)) return '00:00:00';
            const hours = Math.floor(totalSeconds / 3600);
            const minutes = Math.floor((totalSeconds % 3600) / 60);
            const seconds = Math.floor(totalSeconds % 60);
            return `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
        };

        const calculatePace = (dist, durationStr) => {
            if (!dist || !durationStr || parseFloat(dist) <= 0) return '00:00';
            const totalSeconds = parseDurationToSeconds(durationStr);
            if (totalSeconds === null || totalSeconds <= 0) return '00:00';
            const paceInSeconds = totalSeconds / parseFloat(dist);
            const paceMinutes = Math.floor(paceInSeconds / 60);
            const paceSeconds = Math.floor(paceInSeconds % 60);
            return `${String(paceMinutes).padStart(2, '0')}:${String(paceSeconds).padStart(2, '0')}`;
        };

        const formatDate = (timestamp) => {
            if (!timestamp || !timestamp.seconds) return new Date().toLocaleDateString();
            return new Date(timestamp.seconds * 1000).toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' });
        };

        // --- Rendering Functions ---
        const renderTotals = () => {
            const stats = workouts.reduce((acc, workout) => {
                acc.distance += workout.distance || 0;
                acc.calories += workout.calories || 0;
                acc.duration += workout.duration || 0;
                return acc;
            }, { distance: 0, calories: 0, duration: 0 });

            totalDistanceEl.textContent = `${stats.distance.toFixed(2)} mi`;
            totalDurationEl.textContent = formatDuration(stats.duration);
            totalCaloriesEl.textContent = stats.calories.toLocaleString();
        };

        const renderWorkouts = () => {
            historyLoading.classList.add('hidden');
            if (workouts.length === 0) {
                historyEmpty.classList.remove('hidden');
                historyList.classList.add('hidden');
            } else {
                historyEmpty.classList.add('hidden');
                historyList.classList.remove('hidden');
                historyList.innerHTML = workouts.map(workout => `
                    <div class="bg-white/5 backdrop-blur-lg p-4 rounded-xl border border-white/10 shadow-md flex flex-col sm:flex-row sm:items-center sm:justify-between transition duration-300 hover:scale-[1.02] hover:border-white/20">
                        <div class="flex-grow mb-4 sm:mb-0">
                            <p class="font-bold text-lg text-gray-200">${formatDate(workout.createdAt)}</p>
                            <div class="grid grid-cols-2 sm:grid-cols-4 gap-x-4 gap-y-2 mt-2 text-sm">
                                <div class="flex items-center" title="Distance">${runIcon}<span class="ml-2 text-gray-300">${workout.distance.toFixed(2)} mi</span></div>
                                <div class="flex items-center" title="Duration">${clockIcon}<span class="ml-2 text-gray-300">${formatDuration(workout.duration)}</span></div>
                                <div class="flex items-center" title="Calories">${flameIcon}<span class="ml-2 text-gray-300">${workout.calories ? `${workout.calories} kcal` : '--'}</span></div>
                                <div class="flex items-center" title="Pace per mile">${gaugeIcon}<span class="ml-2 text-gray-300">${calculatePace(workout.distance, formatDuration(workout.duration))} /mi</span></div>
                            </div>
                        </div>
                        <div class="flex-shrink-0">
                            <button data-id="${workout.id}" class="delete-btn p-2 rounded-full text-gray-400 hover:bg-red-500/20 hover:text-red-400 transition-colors" aria-label="Delete workout">
                                ${trashIcon}
                            </button>
                        </div>
                    </div>
                `).join('');
            }
            updateAIHelperText();
        };
        
        const updateAIHelperText = () => {
            insightsBtn.disabled = workouts.length < 3;
            if (workouts.length < 3) {
                const needed = 3 - workouts.length;
                aiHelperText.textContent = `Log at least ${needed} more workout(s) to unlock AI insights!`;
                aiHelperText.classList.remove('hidden');
            } else {
                aiHelperText.classList.add('hidden');
            }
        };

        // --- Event Handlers ---
        const handleSubmit = async (e) => {
            e.preventDefault();
            if (!db || !userId) {
                showError("Database not connected.");
                return;
            }

            const distNum = parseFloat(distanceInput.value);
            const calsNum = parseInt(caloriesInput.value, 10) || 0;
            const durationSec = parseDurationToSeconds(durationInput.value);

            if (!distNum || distNum <= 0) return showError("Please enter a valid distance.");
            if (durationSec === null || durationSec <= 0) return showError("Please enter a valid duration (e.g., 46:51 or 00:46:51).");
            if (caloriesInput.value && (isNaN(calsNum) || calsNum < 0)) return showError("Please enter a valid number for calories.");
            
            hideError();

            try {
                const workoutsCollection = collection(db, `artifacts/${typeof __app_id !== 'undefined' ? __app_id : 'default-app-id'}/users/${userId}/workouts`);
                await addDoc(workoutsCollection, {
                    distance: distNum,
                    duration: durationSec,
                    calories: calsNum,
                    createdAt: serverTimestamp()
                });
                workoutForm.reset();
            } catch (err) {
                console.error("Error adding workout:", err);
                showError("Failed to save workout.");
            }
        };

        const handleDelete = async (id) => {
            if (!db || !userId) return;
            const workoutDocRef = doc(db, `artifacts/${typeof __app_id !== 'undefined' ? __app_id : 'default-app-id'}/users/${userId}/workouts`, id);
            try {
                await deleteDoc(workoutDocRef);
            } catch (err) {
                console.error("Error deleting workout:", err);
                showError("Failed to delete workout.");
            }
        };

        const getWorkoutInsights = async () => {
            if (workouts.length < 3) return;

            insightsBtn.disabled = true;
            insightsBtn.textContent = 'Analyzing...';
            aiLoading.classList.remove('hidden');
            aiInsightsContent.classList.add('hidden');
            aiError.classList.add('hidden');

            const systemPrompt = "You are a friendly and encouraging personal trainer. Analyze the user's recent treadmill workouts and provide a short, motivational summary of their progress. Then, suggest one specific, actionable idea for their next workout. Keep the entire response to 3-4 sentences. Format as plain text.";
            const recentWorkouts = workouts.slice(0, 10).map(w => `Date: ${formatDate(w.createdAt)}, Distance: ${w.distance.toFixed(2)} mi, Duration: ${formatDuration(w.duration)}, Calories: ${w.calories} kcal`).join('\n');
            const userQuery = `Here are my recent workouts:\n${recentWorkouts}`;
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
            const payload = { contents: [{ parts: [{ text: userQuery }] }], systemInstruction: { parts: [{ text: systemPrompt }] } };

            try {
                const response = await fetch(apiUrl, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) });
                if (!response.ok) throw new Error(`API call failed with status: ${response.status}`);
                const result = await response.json();
                const text = result.candidates?.[0]?.content?.parts?.[0]?.text;
                if (text) {
                    aiInsightsContent.querySelector('p').textContent = text;
                    aiInsightsContent.classList.remove('hidden');
                } else {
                    throw new Error("Could not parse AI response.");
                }
            } catch (err) {
                console.error("Gemini API error:", err);
                aiError.textContent = "Sorry, I couldn't generate insights right now. Please try again later.";
                aiError.classList.remove('hidden');
            } finally {
                insightsBtn.disabled = false;
                insightsBtn.textContent = 'Generate Workout Summary & Suggestion';
                aiLoading.classList.add('hidden');
                updateAIHelperText();
            }
        };

        // --- Initialization ---
        document.addEventListener('DOMContentLoaded', () => {
            try {
                const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
                if (!firebaseConfig) throw new Error("Firebase configuration is missing.");

                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                onAuthStateChanged(auth, async (user) => {
                    if (user) {
                        userId = user.uid;
                    } else {
                        const token = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                        await (token ? signInWithCustomToken(auth, token) : signInAnonymously(auth));
                    }
                    
                    if (userId) {
                        // Setup Firestore listener
                        const workoutsCollection = collection(db, `artifacts/${typeof __app_id !== 'undefined' ? __app_id : 'default-app-id'}/users/${userId}/workouts`);
                        const q = query(workoutsCollection);
                        onSnapshot(q, (snapshot) => {
                            const workoutsData = [];
                            snapshot.forEach(doc => workoutsData.push({ id: doc.id, ...doc.data() }));
                            workouts = workoutsData.sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
                            renderWorkouts();
                            renderTotals();
                        }, (err) => {
                            console.error("Error fetching workouts:", err);
                            showError("Failed to load workouts.");
                        });
                    }
                });

                // Attach event listeners
                workoutForm.addEventListener('submit', handleSubmit);
                historyList.addEventListener('click', (e) => {
                    const deleteButton = e.target.closest('.delete-btn');
                    if (deleteButton) {
                        handleDelete(deleteButton.dataset.id);
                    }
                });
                insightsBtn.addEventListener('click', getWorkoutInsights);
                
            } catch (e) {
                console.error("Initialization failed:", e);
                showError("Application failed to start. " + e.message);
            }
        });

    </script>
</body>
</html>
