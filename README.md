# louis.github.io
```html
<!DOCTYPE html>
<html lang="fr" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RU-ronzier | UPHF-valenciennes</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        uphf: {
                            primary: '#dc2626',
                            dark: '#991b1b',
                            light: '#fef2f2',
                            accent: '#2563eb'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        .hide-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .hide-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="bg-gray-50 dark:bg-gray-950 text-gray-900 dark:text-gray-100 h-full flex flex-col justify-between selection:bg-red-500 selection:text-white transition-colors duration-200">

    <header class="bg-gradient-to-r from-red-700 via-red-600 to-rose-700 dark:from-red-900 dark:via-red-800 dark:to-rose-950 text-white shadow-lg sticky top-0 z-40 transition-colors duration-200">
        <div class="max-w-md mx-auto px-4 py-3 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="w-11 h-11 bg-white rounded-2xl flex items-center justify-center shadow-md text-red-600 font-black text-xl border-2 border-red-100">
                    <i class="fa-solid fa-utensils"></i>
                </div>
                <div>
                    <h1 class="text-xl font-extrabold tracking-tight flex items-center gap-1.5">
                        RU-ronzier <span class="text-xs bg-red-800 text-red-100 px-2 py-0.5 rounded-full font-medium">Live</span>
                    </h1>
                    <p class="text-xs text-red-100 font-medium tracking-wide">UPHF-valenciennes</p>
                </div>
            </div>
            
            <div class="flex items-center space-x-2">
                <!-- Language Switcher Flags -->
                <div class="flex items-center space-x-1 bg-white/10 p-1 rounded-xl backdrop-blur-sm border border-white/10">
                    <button onclick="setLanguage('fr')" id="langBtnFr" class="w-7 h-7 rounded-lg bg-white text-red-700 flex items-center justify-center text-xs font-bold shadow transition" title="Français">
                        🇫🇷
                    </button>
                    <button onclick="setLanguage('en')" id="langBtnEn" class="w-7 h-7 rounded-lg bg-transparent text-white flex items-center justify-center text-xs font-bold hover:bg-white/20 transition" title="English">
                        🇺🇸
                    </button>
                </div>

                <!-- Notification Bell -->
                <button onclick="openNotificationModal()" class="w-10 h-10 rounded-xl bg-white/10 hover:bg-white/20 flex items-center justify-center transition text-white relative" title="Notifications">
                    <i class="fa-regular fa-bell"></i>
                    <span id="notificationBadge" class="absolute top-2 right-2 w-2 h-2 bg-yellow-400 rounded-full"></span>
                </button>
            </div>
        </div>
    </header>

    <main id="mainContainer" class="flex-1 overflow-y-auto max-w-md w-full mx-auto pb-20 pt-4 px-4 space-y-4">
        
        <div id="tab-accueil" class="space-y-4 tab-content">
            
            <!-- RU Le Ronzier Occupancy Card -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800 text-center relative overflow-hidden transition-colors duration-200">
                <div class="absolute top-0 right-0 transform translate-x-4 -translate-y-4 w-28 h-28 bg-red-50 dark:bg-red-950/30 rounded-full pointer-events-none"></div>
                
                <div class="flex justify-between items-center mb-3">
                    <span id="txtRealtimeAff" class="text-xs font-semibold uppercase tracking-wider text-gray-400 dark:text-gray-500 flex items-center gap-1.5">
                        <i class="fa-solid fa-signal text-red-500"></i> Affluence RU Le Ronzier
                    </span>
                    <span id="lastUpdated" class="text-xs text-gray-400 dark:text-gray-500 bg-gray-50 dark:bg-gray-800 px-2.5 py-1 rounded-full border border-gray-100 dark:border-gray-700">
                        RAZ minuit active
                    </span>
                </div>

                <!-- Circular Gauge RU -->
                <div class="relative w-40 h-40 mx-auto my-2 flex items-center justify-center">
                    <svg class="w-full h-full transform -rotate-90" viewBox="0 0 120 120">
                        <circle cx="60" cy="60" r="50" stroke="currentColor" class="text-gray-100 dark:text-gray-800" stroke-width="12" fill="none"></circle>
                        <circle id="gaugeCircle" cx="60" cy="60" r="50" stroke="#dc2626" stroke-width="12" stroke-dasharray="314" stroke-dashoffset="314" stroke-linecap="round" fill="none" class="transition-all duration-700 ease-out"></circle>
                    </svg>
                    <div class="absolute flex flex-col items-center justify-center text-center">
                        <span id="occupancyPercentage" class="text-3xl font-black text-gray-800 dark:text-gray-100 tracking-tight">0%</span>
                        <span id="occupancyText" class="text-xs font-semibold text-emerald-600 dark:text-emerald-400 mt-0.5 uppercase tracking-wide">Vide (RAZ)</span>
                    </div>
                </div>

                <!-- People Estimation count (Max 500) -->
                <div class="bg-gray-50 dark:bg-gray-800/50 rounded-2xl p-3 my-3 border border-gray-100 dark:border-gray-800 flex items-center justify-around">
                    <div class="text-center">
                        <p id="txtEstimatedPeople" class="text-xs text-gray-500 dark:text-gray-400 font-medium">Personnes estimées</p>
                        <p id="estimatedPeopleCount" class="text-base font-bold text-gray-800 dark:text-gray-200">0 <span class="text-xs font-normal text-gray-500">/ 500 max</span></p>
                    </div>
                    <div class="h-6 w-px bg-gray-200 dark:bg-gray-700"></div>
                    <div class="text-center">
                        <p id="txtWaitTime" class="text-xs text-gray-500 dark:text-gray-400 font-medium">Temps d'attente</p>
                        <p id="estimatedWaitTime" class="text-base font-bold text-gray-800 dark:text-gray-200">0 min</p>
                    </div>
                </div>

                <!-- Clickable Button to Update RU Occupancy -->
                <button id="btnUpdateAffluenceHome" onclick="openReportModal()" class="w-full py-3 px-4 bg-gradient-to-r from-red-600 to-rose-600 hover:from-red-700 hover:to-rose-700 text-white font-bold rounded-2xl shadow-md shadow-red-500/20 active:scale-[0.98] transition flex items-center justify-center gap-2 text-xs">
                    <i class="fa-solid fa-bullhorn text-sm"></i> Modifier l'affluence du RU
                </button>
            </div>

            <!-- Cafétéria UPHF Occupancy Card (Max 150) with Wait Time -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800 text-center relative overflow-hidden transition-colors duration-200">
                <div class="absolute top-0 right-0 transform translate-x-4 -translate-y-4 w-28 h-28 bg-blue-50 dark:bg-blue-950/30 rounded-full pointer-events-none"></div>
                
                <div class="flex justify-between items-center mb-3">
                    <span id="txtCafetAff" class="text-xs font-semibold uppercase tracking-wider text-gray-400 dark:text-gray-500 flex items-center gap-1.5">
                        <i class="fa-solid fa-coffee text-blue-500"></i> Affluence Cafétéria UPHF
                    </span>
                    <span class="text-xs text-gray-400 dark:text-gray-500 bg-gray-50 dark:bg-gray-800 px-2.5 py-1 rounded-full border border-gray-100 dark:border-gray-700">
                        Capacité 150
                    </span>
                </div>

                <!-- Circular Gauge Cafet -->
                <div class="relative w-36 h-36 mx-auto my-2 flex items-center justify-center">
                    <svg class="w-full h-full transform -rotate-90" viewBox="0 0 120 120">
                        <circle cx="60" cy="60" r="50" stroke="currentColor" class="text-gray-100 dark:text-gray-800" stroke-width="12" fill="none"></circle>
                        <circle id="cafetGaugeCircle" cx="60" cy="60" r="50" stroke="#2563eb" stroke-width="12" stroke-dasharray="314" stroke-dashoffset="314" stroke-linecap="round" fill="none" class="transition-all duration-700 ease-out"></circle>
                    </svg>
                    <div class="absolute flex flex-col items-center justify-center text-center">
                        <span id="cafetPercentage" class="text-2xl font-black text-gray-800 dark:text-gray-100 tracking-tight">0%</span>
                        <span id="cafetStatusText" class="text-[11px] font-semibold text-emerald-600 dark:text-emerald-400 mt-0.5 uppercase tracking-wide">Calme</span>
                    </div>
                </div>

                <!-- Cafet People Count & Wait Time -->
                <div class="bg-gray-50 dark:bg-gray-800/50 rounded-2xl p-2.5 my-3 border border-gray-100 dark:border-gray-800 flex items-center justify-around">
                    <div class="text-center">
                        <p id="txtCafetPeople" class="text-xs text-gray-500 dark:text-gray-400 font-medium">Étudiants présents</p>
                        <p id="cafetPeopleCount" class="text-base font-bold text-gray-800 dark:text-gray-200">0 <span class="text-xs font-normal text-gray-500">/ 150 max</span></p>
                    </div>
                    <div class="h-6 w-px bg-gray-200 dark:bg-gray-700"></div>
                    <div class="text-center">
                        <p id="txtCafetWaitTime" class="text-xs text-gray-500 dark:text-gray-400 font-medium">Temps d'attente</p>
                        <p id="cafetWaitTime" class="text-base font-bold text-gray-800 dark:text-gray-200">0 min</p>
                    </div>
                </div>

                <!-- Clickable Button to Update Cafetaria Occupancy -->
                <button id="btnUpdateCafet" onclick="openCafetModal()" class="w-full py-3 px-4 bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-bold rounded-2xl shadow-md shadow-blue-500/20 active:scale-[0.98] transition flex items-center justify-center gap-2 text-xs">
                    <i class="fa-solid fa-mug-hot text-sm"></i> Modifier l'affluence Cafétéria
                </button>
            </div>

            <!-- Quick Preview of Today's Menu on Accueil -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800">
                <div class="flex items-center justify-between mb-3">
                    <h3 id="txtHomeMenuPrevTitle" class="text-sm font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                        <i class="fa-solid fa-utensils text-red-500"></i> Aperçu du Menu du Jour
                    </h3>
                    <button onclick="switchTab('menu')" class="text-xs text-red-600 dark:text-red-400 font-semibold hover:underline">Voir tout <i class="fa-solid fa-arrow-right"></i></button>
                </div>
                <div id="homeMenuPreview" class="text-xs text-gray-600 dark:text-gray-300">
                    <!-- Dynamically loaded preview -->
                </div>
            </div>
        </div>

        <div id="tab-menu" class="space-y-4 tab-content hidden">
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800">
                <div class="flex items-center justify-between mb-3">
                    <div>
                        <h2 id="txtMenuTitle" class="text-base font-bold text-gray-800 dark:text-gray-100">Menus & Cafétéria</h2>
                        <p id="txtMenuSub" class="text-xs text-gray-500 dark:text-gray-400">RU Le Ronzier & Cafétéria UPHF</p>
                    </div>
                    <button id="btnAdminRu" onclick="openAdminModal()" class="bg-red-50 dark:bg-red-950/50 hover:bg-red-100 dark:hover:bg-red-900/50 text-red-600 dark:text-red-400 text-xs font-bold px-3 py-1.5 rounded-xl border border-red-100 dark:border-red-900/50 transition flex items-center gap-1.5" title="Administration des Menus">
                        <i class="fa-solid fa-lock"></i> Admin Menus
                    </button>
                </div>

                <!-- Day Selector Pills -->
                <div class="flex space-x-2 overflow-x-auto pb-2 hide-scrollbar mb-3" id="daySelector">
                    <button onclick="selectDay('Lundi')" class="day-btn px-3.5 py-1.5 rounded-xl text-xs font-semibold bg-red-600 text-white shrink-0 shadow-sm transition" data-day="Lundi">Lundi</button>
                    <button onclick="selectDay('Mardi')" class="day-btn px-3.5 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 shrink-0 transition" data-day="Mardi">Mardi</button>
                    <button onclick="selectDay('Mercredi')" class="day-btn px-3.5 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 shrink-0 transition" data-day="Mercredi">Mercredi</button>
                    <button onclick="selectDay('Jeudi')" class="day-btn px-3.5 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 shrink-0 transition" data-day="Jeudi">Jeudi</button>
                    <button onclick="selectDay('Vendredi')" class="day-btn px-3.5 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 shrink-0 transition" data-day="Vendredi">Vendredi</button>
                </div>

                <!-- Menu Display Container -->
                <div id="menuContainer" class="space-y-4">
                    <!-- Dynamically loaded -->
                </div>
            </div>
        </div>

        <div id="tab-classement" class="space-y-4 tab-content hidden">
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800">
                <div class="flex items-center justify-between mb-3">
                    <div>
                        <h2 id="txtLeaderboardTitle" class="text-base font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                            <i class="fa-solid fa-trophy text-amber-500"></i> Classement des Contributeurs
                        </h2>
                        <p id="txtLeaderboardSub" class="text-xs text-gray-500 dark:text-gray-400">Meilleurs partageurs d'infos du RU</p>
                    </div>
                </div>

                <!-- Period Selector -->
                <div class="grid grid-cols-3 gap-1.5 p-1 bg-gray-100 dark:bg-gray-800 rounded-2xl mb-4">
                    <button onclick="switchLeaderboardPeriod('journalier')" id="lb-btn-journalier" class="py-2 px-3 rounded-xl text-xs font-bold bg-white dark:bg-gray-700 text-red-600 dark:text-red-400 shadow-sm transition">Journalier</button>
                    <button onclick="switchLeaderboardPeriod('hebdomadaire')" id="lb-btn-hebdomadaire" class="py-2 px-3 rounded-xl text-xs font-bold text-gray-600 dark:text-gray-400 hover:text-gray-800 transition">Hebdo</button>
                    <button onclick="switchLeaderboardPeriod('mensuel')" id="lb-btn-mensuel" class="py-2 px-3 rounded-xl text-xs font-bold text-gray-600 dark:text-gray-400 hover:text-gray-800 transition">Mensuel</button>
                </div>

                <div id="leaderboardList" class="space-y-2.5">
                    <!-- Dynamically populated -->
                </div>
            </div>
        </div>

        <div id="tab-discussion" class="space-y-4 tab-content hidden">
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800">
                <div class="flex items-center justify-between mb-3">
                    <div>
                        <h2 id="txtDiscussionTitle" class="text-base font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                            <i class="fa-solid fa-comments text-red-500"></i> Espace Discussion & Infos
                        </h2>
                        <p id="txtDiscussionSub" class="text-xs text-gray-500 dark:text-gray-400">RU Ronzier, Tertiales & Mont Houy</p>
                    </div>
                    <button id="btnPublishPost" onclick="openContributionModal()" class="bg-red-600 hover:bg-red-700 text-white font-bold text-xs px-3 py-2 rounded-xl shadow transition flex items-center gap-1.5">
                        <i class="fa-solid fa-plus"></i> Publier
                    </button>
                </div>

                <!-- Campus Filter Pills -->
                <div class="flex space-x-2 overflow-x-auto pb-2 hide-scrollbar mb-3" id="campusFilter">
                    <button onclick="filterDiscussion('all')" id="filter-all" class="filter-btn px-3 py-1.5 rounded-xl text-xs font-semibold bg-red-600 text-white shrink-0 shadow-sm transition">Tous les flux</button>
                    <button onclick="filterDiscussion('ru')" id="filter-ru" class="filter-btn px-3 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 shrink-0 transition">RU Le Ronzier</button>
                    <button onclick="filterDiscussion('tertiales')" id="filter-tertiales" class="filter-btn px-3 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 shrink-0 transition">Tertiales</button>
                    <button onclick="filterDiscussion('monthouy')" id="filter-monthouy" class="filter-btn px-3 py-1.5 rounded-xl text-xs font-semibold bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300 shrink-0 transition">Mont Houy</button>
                </div>

                <div id="discussionFeedList" class="space-y-2.5">
                    <!-- Dynamically populated -->
                </div>
            </div>
        </div>

        <div id="tab-infos" class="space-y-4 tab-content hidden">
            <!-- User Profile Card -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800">
                <div class="flex items-center space-x-4 mb-3">
                    <div class="w-14 h-14 rounded-2xl bg-gradient-to-tr from-red-600 to-rose-500 flex items-center justify-center text-white text-xl font-black shadow-md relative" id="userAvatar">
                        E
                        <span class="absolute -bottom-1 -right-1 bg-amber-400 text-white text-[10px] px-1.5 py-0.5 rounded-full font-bold shadow" id="userLevelBadge">Niv. 1</span>
                    </div>
                    <div class="flex-1">
                        <div class="flex items-center justify-between">
                            <h3 id="userNameDisplay" class="font-bold text-gray-800 dark:text-gray-100 text-sm">Étudiant UPHF</h3>
                            <button id="btnEditProfile" onclick="editProfile()" class="text-xs text-red-600 dark:text-red-400 hover:underline font-semibold flex items-center gap-1"><i class="fa-solid fa-pen"></i> Modifier</button>
                        </div>
                        <p id="userRoleBadge" class="text-xs font-medium text-amber-600 dark:text-amber-400 bg-amber-50 dark:bg-amber-950/50 px-2 py-0.5 rounded-md inline-block mt-1 border border-amber-100 dark:border-amber-900/50">
                            <i class="fa-solid fa-award mr-1"></i> Guide Local UPHF
                        </p>
                    </div>
                </div>

                <!-- XP & Progress Bar -->
                <div class="bg-gray-50 dark:bg-gray-800/50 rounded-2xl p-3 border border-gray-100 dark:border-gray-800 mb-3">
                    <div class="flex justify-between text-xs font-semibold mb-1">
                        <span id="txtXpProgLabel" class="text-gray-600 dark:text-gray-400">Progression XP (Profil Persistant)</span>
                        <span id="userXpText" class="text-red-600 dark:text-red-400">0 / 100 XP</span>
                    </div>
                    <div class="w-full bg-gray-200 dark:bg-gray-700 h-2 rounded-full overflow-hidden">
                        <div id="userXpBar" class="bg-gradient-to-r from-red-500 to-rose-600 h-full rounded-full transition-all duration-500" style="width: 0%;"></div>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-2.5 rounded-2xl border border-gray-100 dark:border-gray-800 text-center">
                        <p id="txtMyContribs" class="text-xs text-gray-500 dark:text-gray-400">Mes Contributions</p>
                        <p id="userContributionCount" class="text-base font-bold text-gray-800 dark:text-gray-200 mt-0.5">0</p>
                    </div>
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-2.5 rounded-2xl border border-gray-100 dark:border-gray-800 text-center">
                        <p id="txtAccountStatus" class="text-xs text-gray-500 dark:text-gray-400">Statut Compte</p>
                        <p class="text-xs font-bold text-emerald-600 dark:text-emerald-400 mt-1 uppercase tracking-wider bg-emerald-50 dark:bg-emerald-950/50 py-0.5 rounded">Enregistré</p>
                    </div>
                </div>

                <div>
                    <h4 id="txtProfileBadges" class="text-xs font-bold text-gray-400 dark:text-gray-500 uppercase tracking-wider mb-2">Badges de Profil</h4>
                    <div class="flex space-x-2 overflow-x-auto pb-1" id="badgeContainer">
                        <div class="flex flex-col items-center p-2 bg-red-50 dark:bg-red-950/40 rounded-2xl border border-red-100 dark:border-red-900/40 shrink-0 w-20 text-center">
                            <div class="w-8 h-8 rounded-xl bg-red-600 text-white flex items-center justify-center text-xs shadow-sm mb-1"><i class="fa-solid fa-fire"></i></div>
                            <span class="text-[10px] font-bold text-gray-700 dark:text-gray-300 leading-tight">Pionnier</span>
                        </div>
                    </div>
                </div>

                <div class="mt-3 pt-3 border-t border-gray-100 dark:border-gray-800">
                    <button id="btnNewsletterSub" onclick="openNewsletterModal()" class="w-full py-3 px-4 bg-gradient-to-r from-red-600 to-rose-600 hover:from-red-700 hover:to-rose-700 text-white font-semibold rounded-2xl shadow-sm transition flex items-center justify-center gap-2 text-xs">
                        <i class="fa-solid fa-envelope-open-text"></i> S'abonner à la newsletter (+50 XP)
                    </button>
                </div>
            </div>

            <!-- Settings -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800 space-y-3">
                <h3 id="txtAppSettings" class="text-sm font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                    <i class="fa-solid fa-gear text-red-500"></i> Paramètres de l'application
                </h3>

                <div class="space-y-2.5">
                    <div class="flex items-center justify-between p-3 bg-gray-50 dark:bg-gray-800/50 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <div class="flex items-center space-x-3">
                            <div class="w-8 h-8 rounded-xl bg-red-50 dark:bg-red-950/50 text-red-600 dark:text-red-400 flex items-center justify-center text-xs">
                                <i class="fa-solid fa-moon"></i>
                            </div>
                            <div>
                                <p id="txtDarkMode" class="text-xs font-bold text-gray-800 dark:text-gray-200">Mode Sombre</p>
                                <p id="txtDarkModeSub" class="text-[10px] text-gray-500 dark:text-gray-400">Activer le thème nuit</p>
                            </div>
                        </div>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="darkModeToggle" onchange="toggleDarkMode()" class="sr-only peer">
                            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-red-600"></div>
                        </label>
                    </div>

                    <div class="flex items-center justify-between p-3 bg-gray-50 dark:bg-gray-800/50 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <div class="flex items-center space-x-3">
                            <div class="w-8 h-8 rounded-xl bg-red-50 dark:bg-red-950/50 text-red-600 dark:text-red-400 flex items-center justify-center text-xs">
                                <i class="fa-solid fa-bell"></i>
                            </div>
                            <div>
                                <p id="txtPushNotif" class="text-xs font-bold text-gray-800 dark:text-gray-200">Notifications Push</p>
                                <p id="notifStatusText" class="text-[10px] text-gray-500 dark:text-gray-400">Alertes affluence RU</p>
                            </div>
                        </div>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="notificationToggle" onchange="toggleNotifications()" class="sr-only peer">
                            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-red-600"></div>
                        </label>
                    </div>
                </div>
            </div>

            <!-- REGLES DU SITE & SANCTIONS -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800 space-y-3">
                <h3 id="txtRulesTitle" class="text-sm font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                    <i class="fa-solid fa-shield-halved text-red-500"></i> Règles du site & Sanctions
                </h3>
                
                <div class="space-y-2.5 text-xs text-gray-600 dark:text-gray-300">
                    <div class="p-3 bg-gray-50 dark:bg-gray-800/50 rounded-2xl border border-gray-100 dark:border-gray-800 space-y-1.5">
                        <p id="txtRulesConduct" class="font-bold text-gray-800 dark:text-gray-200 flex items-center gap-1.5">
                            <i class="fa-solid fa-circle-check text-emerald-500"></i> Charte de bonne conduite
                        </p>
                        <ul id="txtRulesList" class="list-disc list-inside space-y-1 pl-1 text-[11px] leading-relaxed">
                            <li><strong>Exactitude :</strong> Les signalements d'affluence et discussions doivent refléter la réalité pour aider la communauté UPHF.</li>
                            <li><strong>Pseudos conformes :</strong> Seuls les noms réels au format « Prénom Nom » sont autorisés. Les pseudos bizarres ou injurieux sont interdits.</li>
                            <li><strong>Respect mutuel :</strong> Aucun propos haineux ou insultant dans les contributions n'est toléré.</li>
                        </ul>
                    </div>

                    <div class="p-3 bg-red-50 dark:bg-red-950/40 rounded-2xl border border-red-100 dark:border-red-900/40 space-y-1.5">
                        <p id="txtSanctionsTitle" class="font-bold text-red-900 dark:text-red-300 flex items-center gap-1.5">
                            <i class="fa-solid fa-triangle-exclamation text-red-600"></i> Sanctions encourues en cas d'abus
                        </p>
                        <ul id="txtSanctionsList" class="list-disc list-inside space-y-1 pl-1 text-[11px] leading-relaxed text-red-800 dark:text-red-300">
                            <li><strong>Avertissement :</strong> En cas de première infraction mineure.</li>
                            <li><strong>Pertes de points XP & Badges :</strong> Retrait des avantages accumulés en cas de faux signalements répétés.</li>
                            <li><strong>Bannissement temporaire :</strong> Blocage de l'accès aux contributions et alertes pour une durée déterminée.</li>
                            <li><strong>Bannissement définitif :</strong> Exclusion permanente du site pour les récidivistes ou profils non conformes.</li>
                        </ul>
                    </div>
                </div>
            </div>

            <!-- Practical Information -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800 space-y-3">
                <h3 id="txtPracticalInfo" class="text-sm font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                    <i class="fa-solid fa-circle-info text-red-500"></i> Informations Pratiques - RU Le Ronzier
                </h3>
                
                <div class="space-y-2.5 text-xs text-gray-600 dark:text-gray-300">
                    <div class="flex items-start space-x-3">
                        <div class="w-7 h-7 rounded-xl bg-red-50 dark:bg-red-950/50 text-red-600 dark:text-red-400 flex items-center justify-center shrink-0 mt-0.5"><i class="fa-solid fa-location-dot"></i></div>
                        <div>
                            <p id="txtExactAddress" class="font-bold text-gray-800 dark:text-gray-200">Adresse exacte</p>
                            <p>RU Le Ronzier, <strong>Campus des Tertiales</strong>, 59300 Valenciennes</p>
                        </div>
                    </div>

                    <div class="flex items-start space-x-3">
                        <div class="w-7 h-7 rounded-xl bg-red-50 dark:bg-red-950/50 text-red-600 dark:text-red-400 flex items-center justify-center shrink-0 mt-0.5"><i class="fa-solid fa-clock"></i></div>
                        <div>
                            <p id="txtOpeningHours" class="font-bold text-gray-800 dark:text-gray-200">Horaires d'ouverture</p>
                            <p>Du Lundi au Vendredi : 11h30 – 13h45</p>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-2.5 pt-1">
                    <a href="tel:0327510000" id="btnCallRu" class="py-2.5 px-3 bg-gray-50 dark:bg-gray-800 hover:bg-gray-100 dark:hover:bg-gray-700 text-gray-800 dark:text-gray-200 font-semibold rounded-2xl border border-gray-200 dark:border-gray-700 text-center transition flex items-center justify-center gap-2 text-xs">
                        <i class="fa-solid fa-phone text-red-600"></i> Appeler le RU
                    </a>
                    <a href="mailto:contact@uphf.fr" id="btnEmailRu" class="py-2.5 px-3 bg-gray-50 dark:bg-gray-800 hover:bg-gray-100 dark:hover:bg-gray-700 text-gray-800 dark:text-gray-200 font-semibold rounded-2xl border border-gray-200 dark:border-gray-700 text-center transition flex items-center justify-center gap-2 text-xs">
                        <i class="fa-solid fa-envelope text-red-600"></i> Envoyer un e-mail
                    </a>
                </div>

                <div class="pt-2 border-t border-gray-100 dark:border-gray-800 flex items-center justify-around">
                    <a href="https://www.uphf.fr" target="_blank" class="text-xs text-red-600 dark:text-red-400 hover:underline font-semibold flex items-center gap-1">
                        <i class="fa-solid fa-globe"></i> Site officiel UPHF
                    </a>
                    <a href="https://www.crous-lille.fr" target="_blank" class="text-xs text-red-600 dark:text-red-400 hover:underline font-semibold flex items-center gap-1">
                        <i class="fa-solid fa-utensils"></i> CROUS Lille
                    </a>
                </div>
            </div>

            <!-- FAQ Section with admin password completely removed/hidden -->
            <div class="bg-white dark:bg-gray-900 rounded-3xl p-5 shadow-sm border border-gray-100 dark:border-gray-800 space-y-3">
                <h3 id="txtFaqTitle" class="text-sm font-bold text-gray-800 dark:text-gray-100 flex items-center gap-2">
                    <i class="fa-solid fa-circle-question text-red-500"></i> Foire Aux Questions (FAQ)
                </h3>

                <div id="faqContainer" class="space-y-2.5 text-xs">
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> Quels sont les moyens de paiement acceptés au RU ?</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">Le paiement s'effectue principalement via la carte étudiante UPHF chargée avec le service <strong>Izly</strong> (CROUS), ainsi que par carte bancaire sans contact.</p>
                    </div>

                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> Comment fonctionne la jauge d'affluence ?</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">Elle repose sur les contributions en temps réel des étudiants (capacité max de 500 places). Pour garantir des données fiables, les compteurs sont <strong>remis à 0 automatiquement chaque jour à minuit</strong>.</p>
                    </div>

                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> Comment mettre à jour le menu officiel ?</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">L'affichage des menus est géré exclusivement par l'administration du RU via le bouton « Admin Menus » en utilisant vos identifiants d'accès sécurisés fournis par l'établissement.</p>
                    </div>

                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> Comment gagner des points XP et des badges ?</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">Chaque signalement d'affluence, publication ou inscription à la newsletter vous rapporte de l'XP pour progresser, débloquer des badges exclusifs et grimper dans le classement.</p>
                    </div>
                </div>
            </div>

            <button id="btnContactDev" onclick="openDevModal()" class="w-full py-3 px-4 bg-gray-900 dark:bg-gray-800 hover:bg-black text-white font-semibold rounded-2xl shadow-md transition flex items-center justify-center gap-2 text-xs">
                <i class="fa-solid fa-code"></i> Contacter le développeur / Signaler un bug
            </button>
        </div>
    </main>

    <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto bg-white/95 dark:bg-gray-900/95 backdrop-blur-md border-t border-gray-100 dark:border-gray-800 px-2 py-1.5 z-40 shadow-lg transition-colors duration-200">
        <div class="grid grid-cols-5 gap-1">
            <button onclick="switchTab('accueil')" id="nav-accueil" class="nav-btn flex flex-col items-center justify-center py-1.5 rounded-2xl text-red-600 dark:text-red-400 transition">
                <i class="fa-solid fa-chart-pie text-sm mb-0.5"></i>
                <span id="navTxtAccueil" class="text-[9px] font-bold">Accueil</span>
            </button>
            <button onclick="switchTab('menu')" id="nav-menu" class="nav-btn flex flex-col items-center justify-center py-1.5 rounded-2xl text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition">
                <i class="fa-solid fa-utensils text-sm mb-0.5"></i>
                <span id="navTxtMenuTab" class="text-[9px] font-medium">Menu</span>
            </button>
            <button onclick="switchTab('classement')" id="nav-classement" class="nav-btn flex flex-col items-center justify-center py-1.5 rounded-2xl text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition">
                <i class="fa-solid fa-trophy text-sm mb-0.5"></i>
                <span id="navTxtLeaderboard" class="text-[9px] font-medium">Classement</span>
            </button>
            <button onclick="switchTab('discussion')" id="nav-discussion" class="nav-btn flex flex-col items-center justify-center py-1.5 rounded-2xl text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition">
                <i class="fa-solid fa-comments text-sm mb-0.5"></i>
                <span id="navTxtDiscussion" class="text-[9px] font-medium">Discussion</span>
            </button>
            <button onclick="switchTab('infos')" id="nav-infos" class="nav-btn flex flex-col items-center justify-center py-1.5 rounded-2xl text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition">
                <i class="fa-solid fa-user-circle text-sm mb-0.5"></i>
                <span id="navTxtProfile" class="text-[9px] font-medium">Profil & Infos</span>
            </button>
        </div>
    </nav>

    <!-- MODAL: Update Affluence (RU Le Ronzier) -->
    <div id="reportModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl transform transition-all">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalAffTitle" class="font-bold text-gray-800 dark:text-gray-100 text-base">Actualiser l'affluence du RU</h3>
                <button onclick="closeReportModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500 hover:bg-gray-200 transition"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <p id="modalAffDesc" class="text-xs text-gray-500 dark:text-gray-400 mb-5">Sélectionnez l'ambiance actuelle ou ajustez le nombre de personnes estimé sur place (max 500).</p>

            <div class="grid grid-cols-3 gap-3 mb-5">
                <button onclick="submitAffluenceLevel('calme', 30, 'Calme 😊')" class="p-4 rounded-2xl border-2 border-emerald-100 dark:border-emerald-900/50 bg-emerald-50 dark:bg-emerald-950/40 hover:bg-emerald-100 text-center transition">
                    <span class="text-2xl block mb-1">🟢</span>
                    <span id="btnCalm" class="text-xs font-bold text-emerald-800 dark:text-emerald-300">Calme</span>
                    <span class="text-[10px] text-emerald-600 dark:text-emerald-400 block mt-0.5">&lt; 30%</span>
                </button>
                <button onclick="submitAffluenceLevel('modere', 60, 'Modéré ⚡')" class="p-4 rounded-2xl border-2 border-amber-100 dark:border-amber-900/50 bg-amber-50 dark:bg-amber-950/40 hover:bg-amber-100 text-center transition">
                    <span class="text-2xl block mb-1">🟡</span>
                    <span id="btnMod" class="text-xs font-bold text-amber-800 dark:text-amber-300">Modéré</span>
                    <span class="text-[10px] text-amber-600 dark:text-amber-400 block mt-0.5">30% - 70%</span>
                </button>
                <button onclick="submitAffluenceLevel('bonde', 95, 'Bondé 🔥')" class="p-4 rounded-2xl border-2 border-red-100 dark:border-red-900/50 bg-red-50 dark:bg-red-950/40 hover:bg-red-100 text-center transition">
                    <span class="text-2xl block mb-1">🔴</span>
                    <span id="btnCrowded" class="text-xs font-bold text-red-800 dark:text-red-300">Bondé</span>
                    <span class="text-[10px] text-red-600 dark:text-red-400 block mt-0.5">&gt; 70%</span>
                </button>
            </div>

            <div class="mb-4">
                <label id="modalSliderLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-2">Estimation précise : <span id="sliderValDisplay" class="text-red-600">150</span> personnes / 500</label>
                <input type="range" id="peopleSlider" min="0" max="500" value="150" oninput="document.getElementById('sliderValDisplay').innerText = this.value" class="w-full accent-red-600 cursor-pointer">
            </div>

            <button id="modalAffSubmit" onclick="submitSliderAffluence()" class="w-full py-3.5 bg-red-600 hover:bg-red-700 text-white font-bold rounded-2xl shadow transition text-xs">
                Valider et gagner +15 XP
            </button>
        </div>
    </div>

    <!-- MODAL: Update Cafetaria Affluence (Max 150) -->
    <div id="cafetModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl transform transition-all">
            <div class="flex justify-between items-center mb-4">
                <h3 class="font-bold text-gray-800 dark:text-gray-100 text-base">Actualiser l'affluence Cafétéria UPHF</h3>
                <button onclick="closeCafetModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500 hover:bg-gray-200 transition"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <p class="text-xs text-gray-500 dark:text-gray-400 mb-5">Ajustez le nombre d'étudiants présents à la cafet (max 150).</p>

            <div class="mb-4">
                <label class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-2">Étudiants présents : <span id="cafetSliderValDisplay" class="text-blue-600">45</span> / 150</label>
                <input type="range" id="cafetPeopleSlider" min="0" max="150" value="45" oninput="document.getElementById('cafetSliderValDisplay').innerText = this.value" class="w-full accent-blue-600 cursor-pointer">
            </div>

            <button onclick="submitCafetAffluence()" class="w-full py-3.5 bg-blue-600 hover:bg-blue-700 text-white font-bold rounded-2xl shadow transition text-xs">
                Valider et gagner +10 XP
            </button>
        </div>
    </div>

    <!-- MODAL: Post Contribution / Discussion -->
    <div id="contributionModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalPostTitle" class="font-bold text-gray-800 dark:text-gray-100 text-base">Publier un message / signalement</h3>
                <button onclick="closeContributionModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <div class="space-y-4 mb-5">
                <div>
                    <label id="modalPostCampusLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Campus / Établissement concerné</label>
                    <select id="contribCampus" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                        <option value="ru">🍽️ RU Le Ronzier (Restauration)</option>
                        <option value="tertiales">🏛️ UPHF Campus des Tertiales</option>
                        <option value="monthouy">🎓 UPHF Campus du Mont Houy</option>
                    </select>
                </div>
                <div>
                    <label id="modalPostTypeLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Type de publication</label>
                    <select id="contribType" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                        <option value="attente">⏳ État / Temps d'attente</option>
                        <option value="bonplan">💡 Bon plan / Info utile</option>
                        <option value="info">📢 Annonce générale / Actualité</option>
                    </select>
                </div>
                <div>
                    <label id="modalPostMsgLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Votre message</label>
                    <textarea id="contribMessage" rows="3" placeholder="Ex: Amphi A bondé aux Tertiales ou file rapide au RU !" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500 resize-none"></textarea>
                </div>
            </div>

            <button id="modalPostSubmit" onclick="postContribution()" class="w-full py-3.5 bg-red-600 hover:bg-red-700 text-white font-bold rounded-2xl shadow transition text-xs">
                Publier sur l'espace discussion (+20 XP)
            </button>
        </div>
    </div>

    <!-- MODAL: Newsletter Subscription -->
    <div id="newsletterModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalNewsTitle" class="font-bold text-gray-800 dark:text-gray-100 text-base flex items-center gap-1.5"><i class="fa-solid fa-envelope-open-text text-red-600"></i> Newsletter RU-ronzier</h3>
                <button onclick="closeNewsletterModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <p id="modalNewsDesc" class="text-xs text-gray-600 dark:text-gray-300 mb-4 leading-relaxed">
                Recevez les actualités, les menus spéciaux et les alertes du RU Le Ronzier directement dans votre boîte mail. Gagnez immédiatement <strong class="text-red-600">+50 XP</strong> et un badge exclusif !
            </p>

            <div class="space-y-4 mb-5">
                <div>
                    <label id="modalNewsEmailLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Votre adresse e-mail UPHF / Perso</label>
                    <input type="email" id="newsletterEmailInput" placeholder="prenom.nom@uphf.fr" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                </div>
            </div>

            <button id="modalNewsSubmit" onclick="submitNewsletterSubscription()" class="w-full py-3.5 bg-gradient-to-r from-red-600 to-rose-600 hover:from-red-700 hover:to-rose-700 text-white font-bold rounded-2xl shadow transition text-xs flex items-center justify-center gap-2">
                <i class="fa-solid fa-check"></i> S'inscrire et récupérer mes +50 XP
            </button>
        </div>
    </div>

    <!-- MODAL: Contact Dev -->
    <div id="devModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalDevTitle" class="font-bold text-gray-800 dark:text-gray-100 text-base">Contacter le Développeur</h3>
                <button onclick="closeDevModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <p id="modalDevDesc" class="text-xs text-gray-600 dark:text-gray-300 mb-4 leading-relaxed">
                Application collaborative PWA pour les étudiants de l'UPHF Valenciennes (RU Le Ronzier - Campus des Tertiales & Mont Houy, capacité max 500).
            </p>
            <div class="bg-gray-50 dark:bg-gray-800 p-4 rounded-2xl border border-gray-100 dark:border-gray-700 mb-5 space-y-2 text-xs text-gray-700 dark:text-gray-300">
                <p><strong>Développeur :</strong> Projet Étudiant UPHF</p>
                <p><strong>Contact e-mail :</strong> <a href="mailto:support-ru@uphf.fr" class="text-red-600 dark:text-red-400 font-semibold underline">support-ru@uphf.fr</a></p>
            </div>
            <button onclick="closeDevModal()" class="modalDevClose w-full py-3 bg-gray-900 dark:bg-gray-800 text-white font-bold rounded-2xl text-xs">Fermer</button>
        </div>
    </div>

    <!-- MODAL: Notifications -->
    <div id="notificationModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalNotifTitle" class="font-bold text-gray-800 dark:text-gray-100 text-base">Notifications RU & UPHF</h3>
                <button onclick="closeNotificationModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="space-y-3 mb-5">
                <div class="p-3 bg-emerald-50 dark:bg-emerald-950/50 rounded-2xl border border-emerald-100 dark:border-emerald-900/50 text-xs text-emerald-900 dark:text-emerald-300">
                    <p id="modalNotifResetTitle" class="font-bold mb-0.5"><i class="fa-solid fa-rotate mr-1"></i> Réinitialisation journalière active</p>
                    <p id="modalNotifResetDesc" class="text-emerald-700 dark:text-emerald-400">Les compteurs d'affluence sont remis à 0 chaque jour à minuit.</p>
                </div>
            </div>
            <button onclick="closeNotificationModal()" class="modalNotifClose w-full py-3 bg-red-600 text-white font-bold rounded-2xl text-xs">Compris</button>
        </div>
    </div>

    <!-- MODAL: Admin Menu Manager -->
    <div id="adminModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-md rounded-t-3xl sm:rounded-3xl p-6 shadow-2xl max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalAdminTitle" class="font-bold text-gray-800 dark:text-gray-100 text-base flex items-center gap-1.5"><i class="fa-solid fa-lock text-red-600"></i> Espace Admin Menus</h3>
                <button onclick="closeAdminModal()" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500"><i class="fa-solid fa-xmark"></i></button>
            </div>

            <div id="adminAuthSection" class="space-y-4 mb-2">
                <p id="modalAdminAuthDesc" class="text-xs text-gray-500 dark:text-gray-400">Veuillez saisir le code administrateur pour modifier les menus officiels du RU et de la Cafétéria.</p>
                <div>
                    <label id="modalAdminPwdLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Code Admin</label>
                    <input type="password" id="adminPasswordInput" placeholder="Code secret" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                </div>
                <button id="modalAdminLoginBtn" onclick="verifyAdminPassword()" class="w-full py-3 bg-red-600 hover:bg-red-700 text-white font-bold rounded-2xl text-xs shadow">Se connecter en tant qu'Admin</button>
            </div>

            <div id="adminEditorSection" class="space-y-4 hidden">
                <div class="p-3 bg-emerald-50 dark:bg-emerald-950/50 rounded-2xl border border-emerald-100 dark:border-emerald-900/50 text-xs text-emerald-800 dark:text-emerald-300 flex items-center justify-between">
                    <span id="modalAdminActive"><i class="fa-solid fa-check-circle mr-1"></i> Mode Admin actif</span>
                    <button id="modalAdminLogout" onclick="logoutAdmin()" class="font-bold text-red-600 dark:text-red-400 underline">Déconnexion</button>
                </div>
                <div>
                    <label class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Section à modifier</label>
                    <select id="adminSectionSelect" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                        <option value="ru">🍽️ Menu RU Le Ronzier</option>
                        <option value="cafet">☕ Menu Cafétéria UPHF</option>
                    </select>
                </div>
                <div>
                    <label id="modalAdminDayLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Jour à modifier</label>
                    <select id="adminDaySelect" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                        <option value="Lundi">Lundi</option>
                        <option value="Mardi">Mardi</option>
                        <option value="Mercredi">Mercredi</option>
                        <option value="Jeudi">Jeudi</option>
                        <option value="Vendredi">Vendredi</option>
                    </select>
                </div>
                <div>
                    <label id="modalAdminEntreesLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Entrées / Snacks (séparés par virgule)</label>
                    <input type="text" id="adminEntrees" placeholder="Ex: Salade, Sandwich thon maïs" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                </div>
                <div>
                    <label id="modalAdminPlatsLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Plats / Formules (séparés par virgule)</label>
                    <input type="text" id="adminPlats" placeholder="Ex: Steak purée, Panini poulet cheddar" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                </div>
                <div>
                    <label id="modalAdminDessertsLabel" class="block text-xs font-bold text-gray-700 dark:text-gray-300 mb-1">Desserts / Boissons (séparés par virgule)</label>
                    <input type="text" id="adminDesserts" placeholder="Ex: Yaourt, Tarte aux pommes, Coca-Cola" class="w-full p-3 rounded-2xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-xs focus:outline-none focus:border-red-500">
                </div>
                <button id="modalAdminSaveBtn" onclick="saveAdminMenu()" class="w-full py-3 bg-red-600 hover:bg-red-700 text-white font-bold rounded-2xl text-xs shadow">Enregistrer le menu</button>
            </div>
        </div>
    </div>

    <!-- MODAL: Privacy Policy with fully masked email -->
    <div id="privacyModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-lg rounded-3xl p-6 shadow-2xl max-h-[85vh] overflow-y-auto space-y-4">
            <div class="flex justify-between items-center border-b border-gray-100 dark:border-gray-800 pb-3">
                <h3 id="modalPrivacyTitle" class="font-bold text-base flex items-center gap-2"><i class="fa-solid fa-shield text-red-600"></i> Politique de Confidentialité</h3>
                <button onclick="closePolicyModal('privacyModal')" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500 hover:bg-gray-200 transition"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div id="modalPrivacyContent" class="space-y-3 text-xs text-gray-600 dark:text-gray-300 leading-relaxed">
                <p><strong>1. Collecte des données :</strong> L'application RU-ronzier (UPHF-valenciennes) stocke uniquement vos informations de profil (nom, XP, badges) et vos préférences directement dans le stockage local (<code class="bg-gray-100 dark:bg-gray-800 px-1 py-0.5 rounded text-red-600">localStorage</code>) de votre appareil.</p>
                <p><strong>2. Réinitialisation journalière :</strong> Les données d'affluence du restaurant universitaire, de la cafétéria et les signalements communautaires sont réinitialisés automatiquement à 0 chaque jour à minuit.</p>
                <p><strong>3. Newsletter :</strong> Les adresses e-mail renseignées lors de l'inscription à la newsletter sont transmises à l'équipe administrative de gestion (<code class="bg-gray-100 dark:bg-gray-800 px-1 py-0.5 rounded text-red-600">••••••••••••@••••••.com</code>) dans le seul but de vous informer des actualités du RU et de la cafétéria.</p>
                <p><strong>4. Vos droits :</strong> Vous pouvez à tout moment réinitialiser votre profil ou effacer vos données locales directement depuis les paramètres de votre navigateur.</p>
            </div>
            <button onclick="closePolicyModal('privacyModal')" class="modalPrivacyClose w-full py-3 bg-red-600 text-white font-bold rounded-2xl text-xs mt-2">J'ai compris</button>
        </div>
    </div>

    <!-- MODAL: Terms of Service -->
    <div id="termsModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 w-full max-w-lg rounded-3xl p-6 shadow-2xl max-h-[85vh] overflow-y-auto space-y-4">
            <div class="flex justify-between items-center border-b border-gray-100 dark:border-gray-800 pb-3">
                <h3 id="modalTermsTitle" class="font-bold text-base flex items-center gap-2"><i class="fa-solid fa-file-contract text-red-600"></i> Conditions d'Utilisation</h3>
                <button onclick="closePolicyModal('termsModal')" class="w-8 h-8 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center text-gray-500 hover:bg-gray-200 transition"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div id="modalTermsContent" class="space-y-3 text-xs text-gray-600 dark:text-gray-300 leading-relaxed">
                <p><strong>1. Objet :</strong> RU-ronzier est une application Web Progressive (PWA) collaborative destinée aux étudiants et personnels de l'UPHF (Campus des Tertiales et Mont Houy) pour suivre en temps réel l'affluence du restaurant universitaire et de la cafétéria.</p>
                <p><strong>2. Pseudos et identité :</strong> L'utilisation d'un nom réel au format « Prénom Nom » est obligatoire. Tout pseudonyme fantaisiste, injurieux ou déplacé sera rejeté ou entraînera un bannissement du service.</p>
                <p><strong>3. Engagements de l'utilisateur :</strong> Les signalements d'affluence et messages postés sur l'espace discussion doivent être honnêtes et respectueux. Tout abus ou fausse alerte répétée entraînera des sanctions (suppression d'XP, suspension ou exclusion définitive).</p>
                <p><strong>4. Responsabilité :</strong> Les estimations d'affluence et temps d'attente sont indicatifs et basés sur les contributions participatives des utilisateurs.</p>
            </div>
            <button onclick="closePolicyModal('termsModal')" class="modalTermsClose w-full py-3 bg-red-600 text-white font-bold rounded-2xl text-xs mt-2">J'ai compris</button>
        </div>
    </div>

    <footer class="max-w-md mx-auto w-full px-4 py-3 text-center pb-16 text-[10px] text-gray-400 space-y-1">
        <div class="flex justify-center space-x-3">
            <button onclick="openPolicyModal('privacyModal')" id="footerPrivacy" class="hover:underline text-red-600 dark:text-red-400 font-semibold">Politique de confidentialité</button>
            <span>•</span>
            <button onclick="openPolicyModal('termsModal')" id="footerTerms" class="hover:underline text-red-600 dark:text-red-400 font-semibold">Conditions d'utilisation</button>
        </div>
        <p>© project uphf-RU</p>
    </footer>

    <script>
        const translations = {
            fr: {
                txtRealtimeAff: "Affluence RU Le Ronzier",
                lastUpdated: "RAZ minuit active",
                txtEstimatedPeople: "Personnes estimées",
                txtWaitTime: "Temps d'attente",
                btnUpdateAffluenceHome: "Modifier l'affluence du RU",
                txtCafetAff: "Affluence Cafétéria UPHF",
                txtCafetPeople: "Étudiants présents",
                txtCafetWaitTime: "Temps d'attente",
                btnUpdateCafet: "Modifier l'affluence Cafétéria",
                txtHomeMenuPrevTitle: "Aperçu du Menu du Jour",
                txtMenuTitle: "Menus & Cafétéria",
                txtMenuSub: "RU Le Ronzier & Cafétéria UPHF",
                btnAdminRu: "Admin Menus",
                txtLeaderboardTitle: "Classement des Contributeurs",
                txtLeaderboardSub: "Meilleurs partageurs d'infos du RU",
                lbJournalier: "Journalier",
                lbHebdo: "Hebdo",
                lbMensuel: "Mensuel",
                txtDiscussionTitle: "Espace Discussion & Infos",
                txtDiscussionSub: "RU Ronzier, Tertiales & Mont Houy",
                btnPublishPost: "Publier",
                filterAll: "Tous les flux",
                filterRu: "RU Le Ronzier",
                filterTertiales: "Tertiales",
                filterMonthouy: "Mont Houy",
                txtXpProgLabel: "Progression XP (Profil Persistant)",
                txtMyContribs: "Mes Contributions",
                txtAccountStatus: "Statut Compte",
                txtProfileBadges: "Badges de Profil",
                badgePionnier: "Pionnier",
                badgeNews: "Abonné News",
                btnNewsletterSub: "S'abonner à la newsletter (+50 XP)",
                txtAppSettings: "Paramètres de l'application",
                txtDarkMode: "Mode Sombre",
                txtDarkModeSub: "Activer le thème nuit",
                txtPushNotif: "Notifications Push",
                notifStatusText: "Alertes affluence RU",
                txtRulesTitle: "Règles du site & Sanctions",
                txtRulesConduct: "Charte de bonne conduite",
                txtRulesList: `
                    <li><strong>Exactitude :</strong> Les signalements d'affluence et discussions doivent refléter la réalité pour aider la communauté UPHF.</li>
                    <li><strong>Pseudos conformes :</strong> Seuls les noms réels au format « Prénom Nom » sont autorisés. Les pseudos bizarres ou injurieux sont interdits.</li>
                    <li><strong>Respect mutuel :</strong> Aucun propos haineux ou insultant dans les contributions n'est toléré.</li>
                `,
                txtSanctionsTitle: "Sanctions encourues en cas d'abus",
                txtSanctionsList: `
                    <li><strong>Avertissement :</strong> En cas de première infraction mineure.</li>
                    <li><strong>Pertes de points XP & Badges :</strong> Retrait des avantages accumulés en cas de faux signalements répétés.</li>
                    <li><strong>Bannissement temporaire :</strong> Blocage de l'accès aux contributions et alertes pour une durée déterminée.</li>
                    <li><strong>Bannissement définitif :</strong> Exclusion permanente du site pour les récidivistes ou profils non conformes.</li>
                `,
                txtPracticalInfo: "Informations Pratiques - RU Le Ronzier",
                txtExactAddress: "Adresse exacte",
                txtOpeningHours: "Horaires d'ouverture",
                btnCallRu: "Appeler le RU",
                btnEmailRu: "Envoyer un e-mail",
                txtFaqTitle: "Foire Aux Questions (FAQ)",
                faq1Q: "Quels sont les moyens de paiement acceptés au RU ?",
                faq1A: "Le paiement s'effectue principalement via la carte étudiante UPHF chargée avec le service <strong>Izly</strong> (CROUS), ainsi que par carte bancaire sans contact.",
                faq2Q: "Comment fonctionne la jauge d'affluence ?",
                faq2A: "Elle repose sur les contributions en temps réel des étudiants (capacité max de 500 places pour le RU et 150 pour la cafet). Pour garantir des données fiables, les compteurs sont <strong>remis à 0 automatiquement chaque jour à minuit</strong>.",
                faq3Q: "Comment mettre à jour le menu officiel ?",
                faq3A: "L'affichage des menus est géré exclusivement par l'administration du RU via le bouton « Admin Menus » en utilisant vos identifiants d'accès sécurisés fournis par l'établissement.",
                faq4Q: "Comment gagner des points XP et des badges ?",
                faq4A: "Chaque signalement d'affluence, publication ou inscription à la newsletter vous rapporte de l'XP pour progresser, débloquer des badges exclusifs et grimper dans le classement.",
                btnContactDev: "Contacter le développeur / Signaler un bug",
                navTxtAccueil: "Accueil",
                navTxtMenuTab: "Menu",
                navTxtLeaderboard: "Classement",
                navTxtDiscussion: "Discussion",
                navTxtProfile: "Profil & Infos",
                modalAffTitle: "Actualiser l'affluence du RU",
                modalAffDesc: "Sélectionnez l'ambiance actuelle ou ajustez le nombre de personnes estimé sur place (max 500).",
                btnCalm: "Calme",
                btnMod: "Modéré",
                btnCrowded: "Bondé",
                modalSliderLabel: "Estimation précise :",
                modalAffSubmit: "Valider et gagner +15 XP",
                modalPostTitle: "Publier un message / signalement",
                modalPostCampusLabel: "Campus / Établissement concerné",
                modalPostTypeLabel: "Type de publication",
                modalPostMsgLabel: "Votre message",
                modalPostSubmit: "Publier sur l'espace discussion (+20 XP)",
                modalNewsTitle: "Newsletter RU-ronzier",
                modalNewsDesc: "Recevez les actualités, les menus spéciaux et les alertes du RU Le Ronzier directement dans votre boîte mail. Gagnez immédiatement <strong class=\"text-red-600\">+50 XP</strong> et un badge exclusif !",
                modalNewsEmailLabel: "Votre adresse e-mail UPHF / Perso",
                modalNewsSubmit: "S'inscrire et récupérer mes +50 XP",
                modalDevTitle: "Contacter le Développeur",
                modalDevDesc: "Application collaborative PWA pour les étudiants de l'UPHF Valenciennes (RU Le Ronzier - Campus des Tertiales & Mont Houy, capacité max 500).",
                modalDevClose: "Fermer",
                modalNotifTitle: "Notifications RU & UPHF",
                modalNotifResetTitle: "Réinitialisation journalière active",
                modalNotifResetDesc: "Les compteurs d'affluence sont remis à 0 chaque jour à minuit.",
                modalNotifClose: "Compris",
                modalAdminTitle: "Espace Admin Menus",
                modalAdminAuthDesc: "Veuillez saisir le code administrateur pour modifier les menus officiels du RU et de la Cafétéria.",
                modalAdminPwdLabel: "Code Admin",
                modalAdminLoginBtn: "Se connecter en tant qu'Admin",
                modalAdminActive: "Mode Admin actif",
                modalAdminLogout: "Déconnexion",
                modalAdminDayLabel: "Jour à modifier",
                modalAdminEntreesLabel: "Entrées / Snacks (séparés par virgule)",
                modalAdminPlatsLabel: "Plats / Formules (séparés par virgule)",
                modalAdminDessertsLabel: "Desserts / Boissons (séparés par virgule)",
                modalAdminSaveBtn: "Enregistrer le menu",
                modalPrivacyTitle: "Politique de Confidentialité",
                modalPrivacyClose: "J'ai compris",
                modalTermsTitle: "Conditions d'Utilisation",
                modalTermsClose: "J'ai compris",
                footerPrivacy: "Politique de confidentialité",
                footerTerms: "Conditions d'utilisation"
            },
            en: {
                txtRealtimeAff: "RU Le Ronzier Occupancy",
                lastUpdated: "Midnight reset active",
                txtEstimatedPeople: "Estimated People",
                txtWaitTime: "Wait Time",
                btnUpdateAffluenceHome: "Update RU Occupancy",
                txtCafetAff: "UPHF Cafeteria Occupancy",
                txtCafetPeople: "Students present",
                txtCafetWaitTime: "Wait Time",
                btnUpdateCafet: "Update Cafeteria Occupancy",
                txtHomeMenuPrevTitle: "Today's Menu Preview",
                txtMenuTitle: "Menus & Cafeteria",
                txtMenuSub: "RU Le Ronzier & UPHF Cafeteria",
                btnAdminRu: "Menu Admin",
                txtLeaderboardTitle: "Contributor Leaderboard",
                txtLeaderboardSub: "Top RU info sharers",
                lbJournalier: "Daily",
                lbHebdo: "Weekly",
                lbMensuel: "Monthly",
                txtDiscussionTitle: "Discussion & News Feed",
                txtDiscussionSub: "RU Ronzier, Tertiales & Mont Houy",
                btnPublishPost: "Post",
                filterAll: "All feeds",
                filterRu: "RU Le Ronzier",
                filterTertiales: "Tertiales",
                filterMonthouy: "Mont Houy",
                txtXpProgLabel: "XP Progression (Persistent Profile)",
                txtMyContribs: "My Contributions",
                txtAccountStatus: "Account Status",
                txtProfileBadges: "Profile Badges",
                badgePionnier: "Pioneer",
                badgeNews: "News Sub",
                btnNewsletterSub: "Subscribe to Newsletter (+50 XP)",
                txtAppSettings: "App Settings",
                txtDarkMode: "Dark Mode",
                txtDarkModeSub: "Enable night theme",
                txtPushNotif: "Push Notifications",
                notifStatusText: "RU occupancy alerts",
                txtRulesTitle: "Site Rules & Sanctions",
                txtRulesConduct: "Conduct Charter",
                txtRulesList: `
                    <li><strong>Accuracy:</strong> Occupancy reports and discussions must reflect reality to help the UPHF community.</li>
                    <li><strong>Valid Pseudonyms:</strong> Only real names in the format "First Last" are allowed. Strange or offensive names are banned.</li>
                    <li><strong>Mutual Respect:</strong> No hateful or insulting comments in contributions are tolerated.</li>
                `,
                txtSanctionsTitle: "Sanctions for Abuses",
                txtSanctionsList: `
                    <li><strong>Warning:</strong> In case of a first minor infraction.</li>
                    <li><strong>Loss of XP & Badges:</strong> Removal of accumulated perks for repeated false reports.</li>
                    <li><strong>Temporary Ban:</strong> Blocking access to contributions and alerts for a fixed duration.</li>
                    <li><strong>Permanent Ban:</strong> Permanent site exclusion for repeat offenders or non-compliant profiles.</li>
                `,
                txtPracticalInfo: "Practical Info - RU Le Ronzier",
                txtExactAddress: "Exact Address",
                txtOpeningHours: "Opening Hours",
                btnCallRu: "Call RU",
                btnEmailRu: "Send Email",
                txtFaqTitle: "Frequently Asked Questions (FAQ)",
                faq1Q: "What payment methods are accepted at the RU?",
                faq1A: "Payment is mainly made via the UPHF student card loaded with the <strong>Izly</strong> (CROUS) service, as well as contactless credit card.",
                faq2Q: "How does the occupancy gauge work?",
                faq2A: "It relies on real-time student contributions (max capacity of 500 spots for RU and 150 for cafet). To ensure reliable data, counters are <strong>automatically reset to 0 every day at midnight</strong>.",
                faq3Q: "How to update the official menu?",
                faq3A: "Menu displays are managed exclusively by RU administration via the « Menu Admin » button using secure credentials provided by the institution.",
                faq4Q: "How to earn XP points and badges?",
                faq4A: "Each occupancy report, post, or newsletter signup earns you XP to progress, unlock exclusive badges, and climb the leaderboard.",
                btnContactDev: "Contact Developer / Report a Bug",
                navTxtAccueil: "Home",
                navTxtMenuTab: "Menu",
                navTxtLeaderboard: "Leaderboard",
                navTxtDiscussion: "Discussion",
                navTxtProfile: "Profile & Info",
                modalAffTitle: "Update RU Occupancy",
                modalAffDesc: "Select current vibe or adjust estimated people on site (max 500).",
                btnCalm: "Calm",
                btnMod: "Moderate",
                btnCrowded: "Crowded",
                modalSliderLabel: "Precise estimate:",
                modalAffSubmit: "Submit and earn +15 XP",
                modalPostTitle: "Post a Message / Report",
                modalPostCampusLabel: "Campus / Facility involved",
                modalPostTypeLabel: "Post Type",
                modalPostMsgLabel: "Your Message",
                modalPostSubmit: "Post to Discussion Feed (+20 XP)",
                modalNewsTitle: "RU-ronzier Newsletter",
                modalNewsDesc: "Receive news, special menus, and alerts from RU Le Ronzier directly in your inbox. Instantly earn <strong class=\"text-red-600\">+50 XP</strong> and an exclusive badge!",
                modalNewsEmailLabel: "Your UPHF / Personal Email",
                modalNewsSubmit: "Subscribe & Get +50 XP",
                modalDevTitle: "Contact Developer",
                modalDevDesc: "Collaborative PWA for UPHF Valenciennes students (RU Le Ronzier - Tertiales & Mont Houy Campus, max capacity 500).",
                modalDevClose: "Close",
                modalNotifTitle: "RU & UPHF Notifications",
                modalNotifResetTitle: "Daily reset active",
                modalNotifResetDesc: "Occupancy counters are reset to 0 every day at midnight.",
                modalNotifClose: "Got it",
                modalAdminTitle: "Menu Admin Area",
                modalAdminAuthDesc: "Please enter the admin credentials to modify official RU and Cafeteria menus.",
                modalAdminPwdLabel: "Admin Code",
                modalAdminLoginBtn: "Log in as Admin",
                modalAdminActive: "Admin mode active",
                modalAdminLogout: "Log out",
                modalAdminDayLabel: "Day to modify",
                modalAdminEntreesLabel: "Starters / Snacks (comma separated)",
                modalAdminPlatsLabel: "Mains / Formulas (comma separated)",
                modalAdminDessertsLabel: "Desserts / Drinks (comma separated)",
                modalAdminSaveBtn: "Save Menu",
                modalPrivacyTitle: "Privacy Policy",
                modalPrivacyClose: "I understand",
                modalTermsTitle: "Terms of Use",
                modalTermsClose: "I understand",
                footerPrivacy: "Privacy Policy",
                footerTerms: "Terms of Use"
            }
        };

        let defaultState = {
            lang: 'fr',
            user: {
                name: "Jean Dupont",
                xp: 0,
                level: 1,
                contributionsCount: 0,
                newsletterSubscribed: false
            },
            settings: {
                darkMode: false,
                notifications: false
            },
            dailyData: {
                date: new Date().toDateString(),
                occupancyPercentage: 0,
                estimatedPeople: 0,
                waitTime: 0,
                cafetPeople: 0,
                cafetWaitTime: 0,
                reports: [
                    { id: 1, user: "Thomas Martin", time: "Il y a 10 min", campus: "ru", type: "attente", text: "La file d'attente avance assez vite au RU Le Ronzier !" },
                    { id: 2, user: "Sarah Bernard", time: "Il y a 25 min", campus: "tertiales", type: "info", text: "Amphi 2 un peu chargé aux Tertiales ce matin pour le cours magistral." },
                    { id: 3, user: "Lucas Leroy", time: "Il y a 1h", campus: "monthouy", type: "bonplan", text: "Le RU du Mont Houy propose un super dessert au chocolat aujourd'hui." }
                ]
            },
            menus: {
                "Lundi": {
                    ru: [
                        { category: "Entrées", items: ["Salade composée", "Velouté de légumes"] },
                        { category: "Plats chauds", items: ["Steak haché purée", "Pavé de saumon riz"] },
                        { category: "Desserts", items: ["Yaourt bio", "Tarte aux pommes"] }
                    ],
                    cafet: [
                        { category: "Snacks", items: ["Sandwich Poulet-Crudités", "Panini 3 fromages"] },
                        { category: "Boissons & Douceurs", items: ["Cookie aux pépites de chocolat", "Jus d'orange pressé"] }
                    ]
                },
                "Mardi": { ru: [], cafet: [] },
                "Mercredi": { ru: [], cafet: [] },
                "Jeudi": { ru: [], cafet: [] },
                "Vendredi": { ru: [], cafet: [] }
            },
            leaderboard: {
                journalier: [
                    { rank: 1, name: "Thomas Martin", xp: 145, contributions: 6 },
                    { rank: 2, name: "Sarah Bernard", xp: 120, contributions: 5 },
                    { rank: 3, name: "Lucas Leroy", xp: 95, contributions: 4 }
                ],
                hebdomadaire: [
                    { rank: 1, name: "Camille Petit", xp: 680, contributions: 28 },
                    { rank: 2, name: "Thomas Martin", xp: 540, contributions: 22 },
                    { rank: 3, name: "Nicolas Moreau", xp: 490, contributions: 19 }
                ],
                mensuel: [
                    { rank: 1, name: "Camille Petit", xp: 2450, contributions: 110 },
                    { rank: 2, name: "Lucas Leroy", xp: 2100, contributions: 94 },
                    { rank: 3, name: "Sarah Bernard", xp: 1850, contributions: 82 }
                ]
            }
        };

        let appState = JSON.parse(localStorage.getItem('uphf_ru_state')) || defaultState;

        if(!appState.lang) appState.lang = 'fr';
        if(!appState.menus) appState.menus = defaultState.menus;
        if(appState.menus['Lundi'] && Array.isArray(appState.menus['Lundi'])) {
            let oldLundi = appState.menus['Lundi'];
            appState.menus = {
                "Lundi": { ru: oldLundi, cafet: [] },
                "Mardi": { ru: appState.menus['Mardi'] || [], cafet: [] },
                "Mercredi": { ru: appState.menus['Mercredi'] || [], cafet: [] },
                "Jeudi": { ru: appState.menus['Jeudi'] || [], cafet: [] },
                "Vendredi": { ru: appState.menus['Vendredi'] || [], cafet: [] }
            };
        }
        if(!appState.settings) appState.settings = { darkMode: false, notifications: false };
        if(appState.user.newsletterSubscribed === undefined) appState.user.newsletterSubscribed = false;
        if(!appState.leaderboard) appState.leaderboard = defaultState.leaderboard;
        if(!appState.dailyData.reports) appState.dailyData.reports = defaultState.dailyData.reports;
        if(appState.dailyData.cafetPeople === undefined) appState.dailyData.cafetPeople = 0;
        if(appState.dailyData.cafetWaitTime === undefined) appState.dailyData.cafetWaitTime = 0;

        function checkDailyReset() {
            let currentDate = new Date().toDateString();
            if (appState.dailyData.date !== currentDate) {
                appState.dailyData = {
                    date: currentDate,
                    occupancyPercentage: 0,
                    estimatedPeople: 0,
                    waitTime: 0,
                    cafetPeople: 0,
                    cafetWaitTime: 0,
                    reports: []
                };
                saveState();
                showToast(appState.lang === 'fr' ? "📅 Nouvelle journée : Compteurs réinitialisés à 0 !" : "📅 New day: Counters reset to 0!");
            }
        }

        function saveState() {
            localStorage.setItem('uphf_ru_state', JSON.stringify(appState));
        }

        let currentSelectedDay = 'Lundi';
        let currentLeaderboardPeriod = 'journalier';
        let currentCampusFilter = 'all';

        window.onload = function() {
            checkDailyReset();
            applyLanguage(appState.lang);
            
            if(appState.settings.darkMode) {
                document.documentElement.classList.add('dark');
                document.getElementById('darkModeToggle').checked = true;
            } else {
                document.documentElement.classList.remove('dark');
                document.getElementById('darkModeToggle').checked = false;
            }

            if(appState.settings.notifications) {
                document.getElementById('notificationToggle').checked = true;
                document.getElementById('notifStatusText').innerText = appState.lang === 'fr' ? "Alertes activées" : "Alertes enabled";
            } else {
                document.getElementById('notificationToggle').checked = false;
                document.getElementById('notifStatusText').innerText = translations[appState.lang].notifStatusText;
            }

            if(!localStorage.getItem('uphf_ru_configured')) {
                let msg = appState.lang === 'fr' ? "Bienvenue sur RU-ronzier ! Entrez votre Prénom et Nom de famille (Format: Prénom Nom) :" : "Welcome to RU-ronzier! Enter your First and Last Name (Format: First Last):";
                let configuredName = prompt(msg, "Jean Dupont");
                validateAndSetProfileName(configuredName, true);
            }

            updateGaugeUI();
            updateCafetUI();
            renderDiscussionFeed();
            selectDay('Lundi');
            renderLeaderboard(currentLeaderboardPeriod);
            loadUserProfile();
        };

        function setLanguage(lang) {
            appState.lang = lang;
            saveState();
            applyLanguage(lang);
            showToast(lang === 'fr' ? "🇫🇷 Langue changée en Français" : "🇺🇸 Language changed to English");
        }

        function applyLanguage(lang) {
            let t = translations[lang];
            
            if(lang === 'fr') {
                document.getElementById('langBtnFr').className = "w-7 h-7 rounded-lg bg-white text-red-700 flex items-center justify-center text-xs font-bold shadow transition";
                document.getElementById('langBtnEn').className = "w-7 h-7 rounded-lg bg-transparent text-white flex items-center justify-center text-xs font-bold hover:bg-white/20 transition";
            } else {
                document.getElementById('langBtnEn').className = "w-7 h-7 rounded-lg bg-white text-red-700 flex items-center justify-center text-xs font-bold shadow transition";
                document.getElementById('langBtnFr').className = "w-7 h-7 rounded-lg bg-transparent text-white flex items-center justify-center text-xs font-bold hover:bg-white/20 transition";
            }

            const mapIds = [
                'txtRealtimeAff', 'lastUpdated', 'txtEstimatedPeople', 'txtWaitTime', 'btnUpdateAffluenceHome',
                'txtCafetAff', 'txtCafetPeople', 'txtCafetWaitTime', 'btnUpdateCafet', 'txtHomeMenuPrevTitle',
                'txtMenuTitle', 'txtMenuSub', 'btnAdminRu', 'txtLeaderboardTitle', 'txtLeaderboardSub',
                'lb-btn-journalier', 'lb-btn-hebdomadaire', 'lb-btn-mensuel', 'txtDiscussionTitle', 'txtDiscussionSub',
                'btnPublishPost', 'filter-all', 'filter-ru', 'filter-tertiales', 'filter-monthouy',
                'txtXpProgLabel', 'txtMyContribs', 'txtAccountStatus', 'txtProfileBadges', 'btnNewsletterSub',
                'txtAppSettings', 'txtDarkMode', 'txtDarkModeSub', 'txtPushNotif', 'notifStatusText',
                'txtRulesTitle', 'txtRulesConduct', 'txtSanctionsTitle', 'txtPracticalInfo', 'txtExactAddress',
                'txtOpeningHours', 'btnCallRu', 'btnEmailRu', 'txtFaqTitle', 'btnContactDev', 'navTxtAccueil',
                'navTxtMenuTab', 'navTxtLeaderboard', 'navTxtDiscussion', 'navTxtProfile', 'modalAffTitle', 'modalAffDesc',
                'btnCalm', 'btnMod', 'btnCrowded', 'modalSliderLabel', 'modalAffSubmit', 'modalPostTitle',
                'modalPostCampusLabel', 'modalPostTypeLabel', 'modalPostMsgLabel', 'modalPostSubmit', 'modalNewsTitle',
                'modalNewsDesc', 'modalNewsEmailLabel', 'modalNewsSubmit', 'modalDevTitle', 'modalDevDesc',
                'modalNotifTitle', 'modalNotifResetTitle', 'modalNotifResetDesc', 'modalAdminTitle', 'modalAdminAuthDesc',
                'modalAdminPwdLabel', 'modalAdminLoginBtn', 'modalAdminActive', 'modalAdminLogout', 'modalAdminDayLabel',
                'modalAdminEntreesLabel', 'modalAdminPlatsLabel', 'modalAdminDessertsLabel', 'modalAdminSaveBtn',
                'modalPrivacyTitle', 'modalTermsTitle', 'footerPrivacy', 'footerTerms'
            ];

            mapIds.forEach(id => {
                let el = document.getElementById(id);
                if(el) {
                    if(id === 'lb-btn-journalier') el.innerText = t.lbJournalier;
                    else if(id === 'lb-btn-hebdomadaire') el.innerText = t.lbHebdo;
                    else if(id === 'lb-btn-mensuel') el.innerText = t.lbMensuel;
                    else if(id === 'filter-all') el.innerText = t.filterAll;
                    else if(id === 'filter-ru') el.innerText = t.filterRu;
                    else if(id === 'filter-tertiales') el.innerText = t.filterTertiales;
                    else if(id === 'filter-monthouy') el.innerText = t.filterMonthouy;
                    else if(t[id]) el.innerHTML = (el.tagName === 'BUTTON' && el.querySelector('i')) ? `<i class="${el.querySelector('i').className}"></i> ${t[id]}` : t[id];
                }
            });

            let rulesList = document.getElementById('txtRulesList');
            if(rulesList) rulesList.innerHTML = t.txtRulesList;

            let sanctionsList = document.getElementById('txtSanctionsList');
            if(sanctionsList) sanctionsList.innerHTML = t.txtSanctionsList;

            let faqContainer = document.getElementById('faqContainer');
            if(faqContainer) {
                faqContainer.innerHTML = `
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> ${t.faq1Q}</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">${t.faq1A}</p>
                    </div>
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> ${t.faq2Q}</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">${t.faq2A}</p>
                    </div>
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> ${t.faq3Q}</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">${t.faq3A}</p>
                    </div>
                    <div class="bg-gray-50 dark:bg-gray-800/50 p-3 rounded-2xl border border-gray-100 dark:border-gray-800">
                        <p class="font-bold text-gray-800 dark:text-gray-200 mb-1 flex items-center gap-1.5"><i class="fa-solid fa-circle-dot text-[10px] text-red-500"></i> ${t.faq4Q}</p>
                        <p class="text-gray-600 dark:text-gray-400 leading-relaxed">${t.faq4A}</p>
                    </div>
                `;
            }

            loadUserProfile();
            selectDay(currentSelectedDay);
            updateHomeMenuPreview();
        }

        function validateAndSetProfileName(inputName, isInitial = false) {
            if (!inputName || typeof inputName !== 'string') {
                if (isInitial) {
                    appState.user.name = "Jean Dupont";
                    localStorage.setItem('uphf_ru_configured', 'true');
                    saveState();
                }
                return false;
            }

            let cleaned = inputName.trim().replace(/\s+/g, ' ');
            let parts = cleaned.split(' ');

            if (parts.length < 2 || parts.some(p => p.length < 2)) {
                let errText = appState.lang === 'fr' ? "❌ Nom refusé : Le format 'Prénom + Nom de famille' est obligatoire (ex: Marie Curie)." : "❌ Name rejected: 'First Name + Last Name' format is required (e.g. John Doe).";
                showToast(errText);
                return false;
            }

            let formattedName = parts.map(p => p.charAt(0).toUpperCase() + p.slice(1).toLowerCase()).join(' ');
            appState.user.name = formattedName;
            localStorage.setItem('uphf_ru_configured', 'true');
            saveState();
            loadUserProfile();
            showToast((appState.lang === 'fr' ? "✅ Profil mis à jour : " : "✅ Profile updated: ") + formattedName);
            return true;
        }

        function editProfile() {
            let promptText = appState.lang === 'fr' ? "Modifier votre profil (Format obligatoire : Prénom + Nom de famille) :\nEx: Thomas Martin" : "Edit your profile (Required format: First + Last Name):\nEx: Thomas Martin";
            let newName = prompt(promptText, appState.user.name);
            if(newName !== null) {
                validateAndSetProfileName(newName, false);
            }
        }

        function toggleDarkMode() {
            let isDark = document.getElementById('darkModeToggle').checked;
            appState.settings.darkMode = isDark;
            saveState();

            if (isDark) {
                document.documentElement.classList.add('dark');
                showToast(appState.lang === 'fr' ? "Mode sombre activé 🌙" : "Dark mode enabled 🌙");
            } else {
                document.documentElement.classList.remove('dark');
                showToast(appState.lang === 'fr' ? "Mode clair activé ☀️" : "Light mode enabled ☀️");
            }
        }

        function toggleNotifications() {
            let isNotif = document.getElementById('notificationToggle').checked;
            if (isNotif) {
                if ("Notification" in window) {
                    Notification.requestPermission().then(permission => {
                        if (permission === "granted") {
                            appState.settings.notifications = true;
                            document.getElementById('notifStatusText').innerText = appState.lang === 'fr' ? "Alertes activées" : "Alertes enabled";
                            saveState();
                            showToast(appState.lang === 'fr' ? "Notifications push activées 🔔" : "Push notifications enabled 🔔");
                        } else {
                            document.getElementById('notificationToggle').checked = false;
                            appState.settings.notifications = false;
                            saveState();
                            showToast(appState.lang === 'fr' ? "Permission refusée par le navigateur." : "Permission denied by browser.");
                        }
                    });
                } else {
                    appState.settings.notifications = true;
                    document.getElementById('notifStatusText').innerText = appState.lang === 'fr' ? "Alertes activées" : "Alertes enabled";
                    saveState();
                    showToast(appState.lang === 'fr' ? "Notifications activées !" : "Notifications enabled!");
                }
            } else {
                appState.settings.notifications = false;
                document.getElementById('notifStatusText').innerText = translations[appState.lang].notifStatusText;
                saveState();
                showToast(appState.lang === 'fr' ? "Notifications désactivées." : "Notifications disabled.");
            }
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById('tab-' + tabId).classList.remove('hidden');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-red-600', 'dark:text-red-400');
                btn.classList.add('text-gray-400');
                let span = btn.querySelector('span');
                if(span) {
                    span.classList.remove('font-bold');
                    span.classList.add('font-medium');
                }
            });

            let activeBtn = document.getElementById('nav-' + tabId);
            activeBtn.classList.remove('text-gray-400');
            activeBtn.classList.add('text-red-600');
            let activeSpan = activeBtn.querySelector('span');
            if(activeSpan) {
                activeSpan.classList.remove('font-medium');
                activeSpan.classList.add('font-bold');
            }

            document.getElementById('mainContainer').scrollTop = 0;
        }

        function updateGaugeUI() {
            let pct = appState.dailyData.occupancyPercentage;
            document.getElementById('occupancyPercentage').innerText = pct + '%';
            document.getElementById('estimatedPeopleCount').innerHTML = `${appState.dailyData.estimatedPeople} <span class="text-xs font-normal text-gray-500">/ 500 max</span>`;
            document.getElementById('estimatedWaitTime').innerText = appState.dailyData.waitTime + ' min';

            let circle = document.getElementById('gaugeCircle');
            let strokeOffset = 314 - (314 * pct) / 100;
            circle.style.strokeDashoffset = strokeOffset;

            let statusText = document.getElementById('occupancyText');
            let circleColor = '#10b981';

            if(pct === 0) {
                statusText.innerText = appState.lang === 'fr' ? 'Vide (RAZ)' : 'Empty (Reset)';
                statusText.className = 'text-xs font-semibold text-emerald-600 dark:text-emerald-400 mt-0.5 uppercase tracking-wide';
                circleColor = '#10b981';
            } else if(pct < 40) {
                statusText.innerText = appState.lang === 'fr' ? 'Calme 😊' : 'Calm 😊';
                statusText.className = 'text-xs font-semibold text-emerald-600 dark:text-emerald-400 mt-0.5 uppercase tracking-wide';
                circleColor = '#10b981';
            } else if(pct < 75) {
                statusText.innerText = appState.lang === 'fr' ? 'Modéré ⚡' : 'Moderate ⚡';
                statusText.className = 'text-xs font-semibold text-amber-500 mt-0.5 uppercase tracking-wide';
                circleColor = '#f59e0b';
            } else {
                statusText.innerText = appState.lang === 'fr' ? 'Bondé 🔥' : 'Crowded 🔥';
                statusText.className = 'text-xs font-semibold text-red-600 dark:text-red-400 mt-0.5 uppercase tracking-wide';
                circleColor = '#dc2626';
            }
            circle.style.stroke = circleColor;
        }

        function updateCafetUI() {
            let cafetCount = appState.dailyData.cafetPeople || 0;
            let cafetWait = appState.dailyData.cafetWaitTime || 0;
            let pct = Math.min(100, Math.round((cafetCo
