<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NexusAI - All AI Tools in One Place</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <!-- Three.js for 3D Background -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <style>
        :root {
            --bg-primary: #0a0a0e;
            --bg-secondary: #13131a;
            --accent: #6366f1;
            --accent-hover: #4f46e5;
            --text-primary: #ffffff;
            --text-secondary: #a1a1aa;
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }

        body {
            background-color: var(--bg-primary);
            color: var(--text-primary);
            overflow-x: hidden;
            position: relative;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg-primary);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--bg-secondary);
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--accent);
        }

        /* Background Canvas */
        #canvas-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.4;
        }

        /* Glassmorphism */
        .glass {
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
        }

        .glass-card {
            background: linear-gradient(135deg, rgba(255,255,255,0.05) 0%, rgba(255,255,255,0.01) 100%);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 12px;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .glass-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(99, 102, 241, 0.1), transparent);
            transition: 0.5s;
        }

        .glass-card:hover {
            transform: translateY(-5px);
            border-color: rgba(99, 102, 241, 0.3);
            box-shadow: 0 10px 30px -10px rgba(99, 102, 241, 0.2);
        }

        .glass-card:hover::before {
            left: 100%;
        }

        /* Animations */
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .animate-float {
            animation: float 6s ease-in-out infinite;
        }

        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 15px rgba(99, 102, 241, 0.3); }
            50% { box-shadow: 0 0 25px rgba(99, 102, 241, 0.6); }
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--accent) 0%, #818cf8 100%);
            color: white;
            padding: 0.75rem 1.5rem;
            border-radius: 9999px;
            font-weight: 600;
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .btn-primary:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px rgba(99, 102, 241, 0.5);
        }

        .btn-primary:active {
            transform: scale(0.95);
        }

        /* Category Tabs */
        .category-tab {
            padding: 0.5rem 1.25rem;
            border-radius: 9999px;
            background: transparent;
            border: 1px solid rgba(255,255,255,0.1);
            color: var(--text-secondary);
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.875rem;
            font-weight: 500;
        }

        .category-tab:hover {
            background: rgba(255,255,255,0.05);
            color: var(--text-primary);
        }

        .category-tab.active {
            background: var(--accent);
            color: white;
            border-color: var(--accent);
        }

        /* Modal */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(5px);
            z-index: 1000;
            display: none;
            align-items: center;
            justify-content: center;
            padding: 1rem;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .modal-overlay.active {
            display: flex;
            opacity: 1;
        }

        .modal-content {
            background: var(--bg-secondary);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            width: 100%;
            max-width: 500px;
            max-height: 90vh;
            overflow-y: auto;
            transform: scale(0.9);
            transition: transform 0.3s ease;
            position: relative;
            padding: 2rem;
        }

        .modal-overlay.active .modal-content {
            transform: scale(1);
        }

        .close-modal {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: rgba(255,255,255,0.1);
            border: none;
            color: white;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.2s;
        }

        .close-modal:hover {
            background: rgba(255,50,50,0.3);
            transform: rotate(90deg);
        }

        /* Utility Classes */
        .text-gradient {
            background: linear-gradient(135deg, #ffffff 0%, #a5b4fc 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .text-gradient-primary {
            background: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .shadow-glow {
            box-shadow: 0 0 40px rgba(99, 102, 241, 0.15);
        }

        /* Mobile Responsiveness */
        @media (max-width: 768px) {
            .hero-title {
                font-size: 2.5rem;
                line-height: 1.1;
            }
            
            .nav-links {
                display: none;
            }
            
            .mobile-menu-btn {
                display: block;
            }

            .grid-ai {
                grid-template-columns: 1fr !important;
            }
        }

        @media (min-width: 769px) {
            .mobile-menu-btn {
                display: none;
            }
        }

        /* Loading Spinner */
        .spinner {
            width: 20px;
            height: 20px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease-in-out infinite;
            margin: 0 auto;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Search Bar Animation */
        .search-container:focus-within {
            border-color: var(--accent);
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.2);
        }

        /* Feature Badge */
        .feature-badge {
            background: rgba(239, 68, 68, 0.2);
            color: #fca5a5;
            padding: 2px 8px;
            border-radius: 4px;
            font-size: 0.7rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            border: 1px solid rgba(239, 68, 68, 0.3);
        }

        .new-badge {
            background: rgba(34, 197, 94, 0.2);
            color: #86efac;
            border-color: rgba(34, 197, 94, 0.3);
        }
    </style>
</head>
<body>

    <!-- 3D Background Canvas -->
    <div id="canvas-container"></div>

    <!-- Navigation -->
    <nav class="glass fixed top-0 left-0 right-0 z-50 px-4 md:px-8 py-3 mt-2 mx-4 md:mx-8">
        <div class="flex justify-between items-center max-w-7xl mx-auto">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center shadow-glow">
                    <i class="fa-solid fa-brain text-white text-sm"></i>
                </div>
                <span class="text-xl font-bold tracking-tight">Nexus<span class="text-gradient-primary">AI</span></span>
            </div>
            
            <div class="nav-links flex gap-6 text-sm font-medium text-gray-300">
                <a href="#features" class="hover:text-white transition-colors">Features</a>
                <a href="#tools" class="hover:text-white transition-colors">Tools</a>
                <a href="#pricing" class="hover:text-white transition-colors">Pricing</a>
            </div>

            <div class="flex items-center gap-3">
                <button class="btn-primary mobile-menu-btn text-sm" onclick="toggleMobileMenu()">
                    <i class="fa-solid fa-bars"></i>
                </button>
                <button class="hidden md:block px-4 py-2 rounded-full border border-white/10 hover:bg-white/10 transition-colors text-sm font-medium" onclick="openModal('login')">
                    Sign In
                </button>
                <button class="hidden md:block btn-primary text-sm" onclick="openModal('signup')">
                    Get Started
                </button>
            </div>
        </div>

        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden mt-4 pt-4 border-t border-white/10">
            <div class="flex flex-col gap-3">
                <a href="#features" class="text-gray-300 hover:text-white py-2" onclick="toggleMobileMenu()">Features</a>
                <a href="#tools" class="text-gray-300 hover:text-white py-2" onclick="toggleMobileMenu()">Tools</a>
                <a href="#pricing" class="text-gray-300 hover:text-white py-2" onclick="toggleMobileMenu()">Pricing</a>
                <div class="flex gap-2 mt-2">
                    <button class="flex-1 px-4 py-2 rounded-full border border-white/10 hover:bg-white/10 transition-colors text-sm font-medium" onclick="openModal('login')">Sign In</button>
                    <button class="flex-1 px-4 py-2 rounded-full btn-primary text-sm" onclick="openModal('signup')">Get Started</button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <header class="relative pt-40 pb-20 px-4 md:px-8 min-h-screen flex flex-col justify-center items-center text-center max-w-6xl mx-auto">
        <div class="animate-float mb-6">
            <span class="glass px-4 py-2 rounded-full text-sm text-indigo-300 font-medium border-indigo-500/30">
                <i class="fa-solid fa-star mr-2"></i> The Ultimate AI Hub - Now with 100+ Tools
            </span>
        </div>
        
        <h1 class="hero-title text-5xl md:text-7xl font-bold leading-tight mb-6 tracking-tight">
            Access All AI <br />
            <span class="text-gradient">In One Place</span>
        </h1>
        
        <p class="text-lg md:text-xl text-gray-400 max-w-2xl mb-10 leading-relaxed">
            NexusAI aggregates the best AI tools from around the world. 
            Chat, generate images, create videos, code, and more without switching tabs.
        </p>

        <div class="flex flex-col sm:flex-row gap-4 mb-12">
            <button class="btn-primary px-8 py-4 text-lg shadow-glow" onclick="scrollToTools()">
                Explore Tools <i class="fa-solid fa-arrow-right ml-2"></i>
            </button>
            <button class="glass px-8 py-4 text-lg hover:bg-white/5 transition-all font-medium" onclick="openModal('demo')">
                <i class="fa-solid fa-play mr-2 text-indigo-400"></i> Watch Demo
            </button>
        </div>

        <!-- Stats -->
        <div class="grid grid-cols-2 md:grid-cols-4 gap-6 w-full max-w-4xl glass p-6 rounded-2xl">
            <div>
                <div class="text-3xl font-bold text-white mb-1">100+</div>
                <div class="text-sm text-gray-400">AI Tools</div>
            </div>
            <div>
                <div class="text-3xl font-bold text-white mb-1">50k+</div>
                <div class="text-sm text-gray-400">Users</div>
            </div>
            <div>
                <div class="text-3xl font-bold text-white mb-1">24/7</div>
                <div class="text-sm text-gray-400">Access</div>
            </div>
            <div>
                <div class="text-3xl font-bold text-white mb-1">1</div>
                <div class="text-sm text-gray-400">Subscription</div>
            </div>
        </div>
    </header>

    <!-- Features Section -->
    <section id="features" class="py-20 px-4 md:px-8 relative">
        <div class="max-w-7xl mx-auto">
            <div class="text-center mb-16">
                <h2 class="text-4xl md:text-5xl font-bold mb-4">Why Choose Nexus<span class="text-gradient-primary">AI</span>?</h2>
                <p class="text-gray-400 text-lg">Everything you need in one dashboard</p>
            </div>

            <div class="grid md:grid-cols-3 gap-6">
                <!-- Feature 1 -->
                <div class="glass-card p-6">
                    <div class="w-12 h-12 rounded-lg bg-indigo-500/20 flex items-center justify-center mb-4 text-indigo-400 text-xl">
                        <i class="fa-solid fa-layer-group"></i>
                    </div>
                    <h3 class="text-xl font-bold mb-2">Unified Dashboard</h3>
                    <p class="text-gray-400 leading-relaxed">
                        No more tab switching. Access ChatGPT, DALL-E, Midjourney, and all other tools from a single, sleek interface.
                    </p>
                </div>

                <!-- Feature 2 -->
                <div class="glass-card p-6">
                    <div class="w-12 h-12 rounded-lg bg-purple-500/20 flex items-center justify-center mb-4 text-purple-400 text-xl">
                        <i class="fa-solid fa-wallet"></i>
                    </div>
                    <h3 class="text-xl font-bold mb-2">One Subscription</h3>
                    <p class="text-gray-400 leading-relaxed">
                        Stop paying for 10 different services. Get access to premium features across all tools with one plan.
                    </p>
                </div>

                <!-- Feature 3 -->
                <div class="glass-card p-6">
                    <div class="w-12 h-12 rounded-lg bg-pink-500/20 flex items-center justify-center mb-4 text-pink-400 text-xl">
                        <i class="fa-solid fa-bolt"></i>
                    </div>
                    <h3 class="text-xl font-bold mb-2">Lightning Fast</h3>
                    <p class="text-gray-400 leading-relaxed">
                        Optimized routing ensures you get the fastest response times from each AI provider.
                    </p>
                </div>
            </div>
        </div>
    </section>

    <!-- Tools Grid Section -->
    <section id="tools" class="py-20 px-4 md:px-8 relative">
        <div class="max-w-7xl mx-auto">
            <!-- Header & Search -->
            <div class="flex flex-col md:flex-row justify-between items-center mb-10 gap-4">
                <h2 class="text-3xl md:text-4xl font-bold">Available Tools</h2>
                
                <div class="relative w-full md:w-96 search-container">
                    <input 
                        type="text" 
                        id="searchInput" 
                        placeholder="Search AI tools..." 
                        class="w-full bg-[#13131a] border border-white/10 rounded-full py-3 pl-10 pr-4 focus:outline-none focus:border-indigo-500 transition-all text-sm"
                        oninput="filterTools()"
                    >
                    <i class="fa-solid fa-magnifying-glass absolute left-4 top-1/2 -translate-y-1/2 text-gray-500"></i>
                </div>
            </div>

            <!-- Categories -->
            <div class="flex flex-wrap gap-2 mb-8" id="categoryContainer">
                <button class="category-tab active" data-category="all" onclick="filterByCategory('all', this)">All</button>
                <button class="category-tab" data-category="assistants" onclick="filterByCategory('assistants', this)">AI Assistants</button>
                <button class="category-tab" data-category="image" onclick="filterByCategory('image', this)">Image Gen</button>
                <button class="category-tab" data-category="video" onclick="filterByCategory('video', this)">Video Gen</button>
                <button class="category-tab" data-category="productivity" onclick="filterByCategory('productivity', this)">Productivity</button>
                <button class="category-tab" data-category="coding" onclick="filterByCategory('coding', this)">Coding</button>
                <button class="category-tab" data-category="audio" onclick="filterByCategory('audio', this)">Audio/Music</button>
                <button class="category-tab" data-category="other" onclick="filterByCategory('other', this)">Other</button>
            </div>

            <!-- Grid -->
            <div id="toolsGrid" class="grid-ai grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6">
                <!-- Tools will be injected here by JS -->
            </div>

            <div id="noResults" class="hidden text-center py-12">
                <i class="fa-solid fa-robot text-6xl text-gray-700 mb-4"></i>
                <p class="text-gray-400 text-lg">No tools found matching your search.</p>
            </div>
        </div>
    </section>

    <!-- Stats/Impact Section -->
    <section class="py-20 px-4 md:px-8 relative overflow-hidden">
        <div class="absolute inset-0 bg-gradient-to-b from-transparent to-indigo-900/10 pointer-events-none"></div>
        <div class="max-w-7xl mx-auto text-center relative z-10">
            <h2 class="text-4xl md:text-5xl font-bold mb-6">Transform Your Workflow</h2>
            <p class="text-xl text-gray-400 mb-12 max-w-2xl mx-auto">
                Join thousands of creators, developers, and businesses using NexusAI to supercharge their productivity.
            </p>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-4xl mx-auto">
                <div class="glass p-8 rounded-2xl">
                    <div class="text-4xl font-bold text-indigo-400 mb-2">95%</div>
                    <div class="text-gray-300">Time Saved on Multi-Tool Tasks</div>
                </div>
                <div class="glass p-8 rounded-2xl">
                    <div class="text-4xl font-bold text-purple-400 mb-2">10x</div>
                    <div class="text-gray-300">Faster Content Creation</div>
                </div>
                <div class="glass p-8 rounded-2xl">
                    <div class="text-4xl font-bold text-pink-400 mb-2">∞</div>
                    <div class="text-gray-300">Creative Possibilities</div>
                </div>
            </div>
        </div>
    </section>

    <!-- Pricing Section -->
    <section id="pricing" class="py-20 px-4 md:px-8 relative">
        <div class="max-w-7xl mx-auto">
            <div class="text-center mb-16">
                <h2 class="text-4xl md:text-5xl font-bold mb-4">Simple, Transparent Pricing</h2>
                <p class="text-gray-400 text-lg">Access all tools without hidden fees</p>
            </div>

            <div class="grid md:grid-cols-3 gap-6 max-w-5xl mx-auto">
                <!-- Free -->
                <div class="glass-card p-8 rounded-2xl flex flex-col">
                    <div class="mb-4">
                        <span class="text-2xl font-bold">Free</span>
                        <div class="text-gray-400 text-sm mt-1">Hobby & Testing</div>
                    </div>
                    <div class="text-4xl font-bold mb-6">$0<span class="text-lg font-normal text-gray-500">/mo</span></div>
                    <ul class="space-y-3 mb-8 flex-1 text-gray-300 text-sm">
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-green-500"></i> 100 Requests/month</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-green-500"></i> Access to 20+ Tools</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-green-500"></i> Community Support</li>
                    </ul>
                    <button class="w-full py-3 rounded-xl border border-white/20 hover:bg-white/10 transition-all font-medium" onclick="openModal('signup')">Start Free</button>
                </div>

                <!-- Pro -->
                <div class="glass-card p-8 rounded-2xl flex flex-col border-indigo-500/50 relative shadow-glow">
                    <div class="absolute top-0 right-0 bg-indigo-500 text-white text-xs font-bold px-3 py-1 rounded-bl-xl rounded-tr-xl">POPULAR</div>
                    <div class="mb-4">
                        <span class="text-2xl font-bold">Pro</span>
                        <div class="text-gray-400 text-sm mt-1">For Professionals</div>
                    </div>
                    <div class="text-4xl font-bold mb-6">$29<span class="text-lg font-normal text-gray-500">/mo</span></div>
                    <ul class="space-y-3 mb-8 flex-1 text-gray-300 text-sm">
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-indigo-400"></i> Unlimited Requests</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-indigo-400"></i> All 100+ Tools</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-indigo-400"></i> Priority Processing</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-indigo-400"></i> Premium Support</li>
                    </ul>
                    <button class="w-full py-3 rounded-xl btn-primary font-bold" onclick="openModal('signup')">Get Pro</button>
                </div>

                <!-- Enterprise -->
                <div class="glass-card p-8 rounded-2xl flex flex-col">
                    <div class="mb-4">
                        <span class="text-2xl font-bold">Enterprise</span>
                        <div class="text-gray-400 text-sm mt-1">Teams & Business</div>
                    </div>
                    <div class="text-4xl font-bold mb-6">$99<span class="text-lg font-normal text-gray-500">/mo</span></div>
                    <ul class="space-y-3 mb-8 flex-1 text-gray-300 text-sm">
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-purple-500"></i> Team Collaboration</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-purple-500"></i> API Access</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-purple-500"></i> Custom Integrations</li>
                        <li class="flex items-center gap-2"><i class="fa-solid fa-check text-purple-500"></i> Dedicated Manager</li>
                    </ul>
                    <button class="w-full py-3 rounded-xl border border-white/20 hover:bg-white/10 transition-all font-medium" onclick="openModal('contact')">Contact Sales</button>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="border-t border-white/10 pt-16 pb-8 px-4 md:px-8 bg-[#0a0a0e]">
        <div class="max-w-7xl mx-auto">
            <div class="grid grid-cols-2 md:grid-cols-4 gap-8 mb-12">
                <div>
                    <div class="flex items-center gap-2 mb-4">
                        <div class="w-6 h-6 rounded bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center text-xs">
                            <i class="fa-solid fa-brain"></i>
                        </div>
                        <span class="font-bold">Nexus<span class="text-gradient-primary">AI</span></span>
                    </div>
                    <p class="text-gray-500 text-sm leading-relaxed">
                        The ultimate hub for all AI tools. Unifying the future of creativity and productivity.
                    </p>
                </div>
                
                <div>
                    <h4 class="font-semibold mb-4">Product</h4>
                    <ul class="space-y-2 text-sm text-gray-400">
                        <li><a href="#" class="hover:text-white transition-colors">Features</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Tools</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Pricing</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">API</a></li>
                    </ul>
                </div>

                <div>
                    <h4 class="font-semibold mb-4">Company</h4>
                    <ul class="space-y-2 text-sm text-gray-400">
                        <li><a href="#" class="hover:text-white transition-colors">About</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Blog</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Careers</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Contact</a></li>
                    </ul>
                </div>

                <div>
                    <h4 class="font-semibold mb-4">Legal</h4>
                    <ul class="space-y-2 text-sm text-gray-400">
                        <li><a href="#" class="hover:text-white transition-colors">Privacy</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Terms</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Security</a></li>
                    </ul>
                </div>
            </div>

            <div class="pt-8 border-t border-white/5 flex flex-col md:flex-row justify-between items-center gap-4">
                <div class="text-gray-500 text-sm">
                    © 2024 NexusAI Inc. All rights reserved.
                </div>
                <div class="flex gap-4 text-gray-400">
                    <a href="#" class="hover:text-indigo-400 transition-colors"><i class="fa-brands fa-twitter"></i></a>
                    <a href="#" class="hover:text-indigo-400 transition-colors"><i class="fa-brands fa-discord"></i></a>
                    <a href="#" class="hover:text-indigo-400 transition-colors"><i class="fa-brands fa-github"></i></a>
                </div>
            </div>
        </div>
    </footer>

    <!-- Modal Overlay -->
    <div id="modalOverlay" class="modal-overlay" onclick="closeModal(event)">
        <div class="modal-content" onclick="event.stopPropagation()">
            <button class="close-modal" onclick="closeModal()">
                <i class="fa-solid fa-xmark"></i>
            </button>
            
            <div id="modalBody">
                <!-- Dynamic Content -->
            </div>
        </div>
    </div>

    <script>
        // --- DATA: AI Tools Database ---
        const toolsData = [
            // AI Assistants
            { id: 1, name: "ChatGPT", category: "assistants", icon: "fa-comments", desc: "An AI chatbot based on large language models", badge: "Popular", color: "text-green-400" },
            { id: 2, name: "Copilot", category: "assistants", icon: "fa-robot", desc: "Complete answers to your questions", badge: "", color: "text-blue-400" },
            { id: 3, name: "Grok", category: "assistants", icon: "fa-fire", desc: "Conversational AI and multimodal chatbot", badge: "New", color: "text-orange-400" },
            { id: 4, name: "Claude", category: "assistants", icon: "fa-feather", desc: "Advanced AI assistant for various tasks", badge: "", color: "text-purple-400" },
            { id: 5, name: "Gemini", category: "assistants", icon: "fa-star", desc: "Google's AI assistant and chatbot", badge: "", color: "text-indigo-400" },
            
            // Image Generation
            { id: 6, name: "DALL-E 3", category: "image", icon: "fa-image", desc: "AI image generator with high accuracy", badge: "Pro", color: "text-emerald-400" },
            { id: 7, name: "Midjourney", category: "image", icon: "fa-palette", desc: "AI-powered text-to-image generator", badge: "Popular", color: "text-cyan-400" },
            { id: 8, name: "Stable Diffusion", category: "image", icon: "fa-layer-group", desc: "Open source text-to-image model", badge: "", color: "text-yellow-400" },
            { id: 9, name: "Adobe Firefly", category: "image", icon: "fa-swatchbook", desc: "Standalone web application for images", badge: "", color: "text-red-400" },
            { id: 10, name: "Canva", category: "image", icon: "fa-pen-to-square", desc: "Text-to-image generation tool", badge: "", color: "text-pink-400" },

            // Productivity
            { id: 11, name: "Notion AI", category: "productivity", icon: "fa-note-sticky", desc: "Automating tasks and generating content", badge: "", color: "text-gray-300" },
            { id: 12, name: "Miro AI", category: "productivity", icon: "fa-chalkboard", desc: "Enhances creativity and collaboration", badge: "", color: "text-orange-300" },
            { id: 13, name: "Gamma", category: "productivity", icon: "fa-clone", desc: "Creating engaging content", badge: "", color: "text-lime-300" },

            // Video Generation
            { id: 14, name: "Sora", category: "video", icon: "fa-video", desc: "OpenAI's text-to-video generation", badge: "New", color: "text-red-400" },
            { id: 15, name: "Heygen", category: "video", icon: "fa-user-circle", desc: "AI video generation tool", badge: "", color: "text-indigo-300" },
            { id: 16, name: "Invideo AI", category: "video", icon: "fa-film", desc: "Text-to-video tool", badge: "", color: "text-blue-300" },

            // Audio & Music
            { id: 17, name: "Suno AI", category: "audio", icon: "fa-music", desc: "Generate songs with AI", badge: "New", color: "text-pink-400" },
            { id: 18, name: "ElevenLab", category: "audio", icon: "fa-microphone-lines", desc: "Advanced AI speech synthesis", badge: "", color: "text-violet-400" },

            // Coding
            { id: 19, name: "GitHub Copilot", category: "coding", icon: "fa-code", desc: "AI pair programmer", badge: "Popular", color: "text-orange-400" },
            { id: 20, name: "Replit", category: "coding", icon: "fa-terminal", desc: "AI-powered code generation", badge: "", color: "text-green-400" },
            { id: 21, name: "Cursor", category: "coding", icon: "fa-cursor", desc: "AI-first code editor", badge: "New", color: "text-yellow-400" },

            // Other Tools
            { id: 22, name: "DeepL", category: "other", icon: "fa-globe", desc: "AI-powered translation tool", badge: "", color: "text-blue-400" },
            { id: 23, name: "Tableau AI", category: "other", icon: "fa-chart-pie", desc: "Intelligent analytics platform", badge: "", color: "text-indigo-400" },
            { id: 24, name: "Lovable", category: "other", icon: "fa-heart", desc: "AI for design and branding", badge: "New", color: "text-red-400" },
            { id: 25, name: "Ebook Gen", category: "other", icon: "fa-book-open", desc: "Automated ebook creation", badge: "", color: "text-emerald-400" },
        ];

        // --- STATE MANAGEMENT ---
        let currentCategory = 'all';
        let searchTerm = '';

        // --- INITIALIZATION ---
        document.addEventListener('DOMContentLoaded', () => {
            renderTools();
            initThreeJS();
        });

        // --- RENDER TOOLS ---
        function renderTools() {
            const grid = document.getElementById('toolsGrid');
            const noResults = document.getElementById('noResults');
            
            grid.innerHTML = '';
            
            // Filter Logic
            const filteredTools = toolsData.filter(tool => {
                const matchesCategory = currentCategory === 'all' || tool.category === currentCategory;
                const matchesSearch = tool.name.toLowerCase().includes(searchTerm.toLowerCase()) || 
                                     tool.desc.toLowerCase().includes(searchTerm.toLowerCase());
                return matchesCategory && matchesSearch;
            });

            if (filteredTools.length === 0) {
                grid.classList.add('hidden');
                noResults.classList.remove('hidden');
                return;
            } else {
                grid.classList.remove('hidden');
                noResults.classList.add('hidden');
            }

            // Generate Cards
            filteredTools.forEach(tool => {
                const card = document.createElement('div');
                card.className = 'glass-card p-5 flex flex-col justify-between h-full cursor-pointer group';
                card.innerHTML = `
                    <div>
                        <div class="flex justify-between items-start mb-3">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-lg bg-white/5 flex items-center justify-center ${tool.color} text-lg group-hover:scale-110 transition-transform">
                                    <i class="fa-solid ${tool.icon}"></i>
                                </div>
                                <div>
                                    <h3 class="font-bold text-lg">${tool.name}</h3>
                                    <div class="text-xs text-gray-500 capitalize">${tool.category}</div>
                                </div>
                            </div>
                            ${tool.badge ? `<span class="feature-badge ${tool.badge === 'New' ? 'new-badge' : ''}">${tool.badge}</span>` : ''}
                        </div>
                        <p class="text-gray-400 text-sm leading-relaxed mb-4">${tool.desc}</p>
                    </div>
                    <div class="flex items-center gap-2 mt-auto">
                        <button class="flex-1 py-2 rounded-lg bg-white/5 hover:bg-white/10 text-sm font-medium transition-colors" onclick="openToolDetails(${tool.id})">
                            Try Now
                        </button>
                        <button class="w-9 h-9 rounded-lg bg-white/5 hover:bg-indigo-500/30 hover:text-indigo-400 flex items-center justify-center transition-colors" title="Details">
                            <i class="fa-solid fa-info text-xs"></i>
                        </button>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        // --- FILTERING ---
        function filterByCategory(category, element) {
            currentCategory = category;
            
            // Update UI
            document.querySelectorAll('.category-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            if (element) element.classList.add('active');
            
            // Reset search when changing category
            document.getElementById('searchInput').value = '';
            searchTerm = '';
            
            renderTools();
        }

        function filterTools() {
            const input = document.getElementById('searchInput');
            searchTerm = input.value;
            renderTools();
        }

        // --- MODALS ---
        const modalOverlay = document.getElementById('modalOverlay');
        const modalBody = document.getElementById('modalBody');

        function openModal(type) {
            const mobileMenu = document.getElementById('mobile-menu');
            if (!mobileMenu.classList.contains('hidden')) toggleMobileMenu();

            modalOverlay.classList.add('active');
            
            let content = '';
            
            if (type === 'login') {
                content = `
                    <h2 class="text-2xl font-bold mb-6">Welcome Back</h2>
                    <form class="space-y-4" onsubmit="event.preventDefault(); closeModal();">
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">Email</label>
                            <input type="email" class="w-full bg-[#1a1a22] border border-white/10 rounded-lg p-3 focus:border-indigo-500 focus:outline-none" placeholder="you@example.com" required>
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">Password</label>
                            <input type="password" class="w-full bg-[#1a1a22] border border-white/10 rounded-lg p-3 focus:border-indigo-500 focus:outline-none" placeholder="••••••••" required>
                        </div>
                        <button type="submit" class="w-full btn-primary py-3 mt-4">Sign In</button>
                    </form>
                    <div class="mt-6 text-center text-sm text-gray-500">
                        Don't have an account? <button onclick="openModal('signup')" class="text-indigo-400 hover:underline">Sign up</button>
                    </div>
                `;
            } else if (type === 'signup') {
                content = `
                    <h2 class="text-2xl font-bold mb-6">Get Started Free</h2>
                    <div class="space-y-3 mb-6">
                        <button class="w-full flex items-center justify-center gap-2 bg-white/5 hover:bg-white/10 rounded-lg p-3 transition-colors">
                            <i class="fa-brands fa-google text-xl"></i>
                            <span>Continue with Google</span>
                        </button>
                        <button class="w-full flex items-center justify-center gap-2 bg-white/5 hover:bg-white/10 rounded-lg p-3 transition-colors">
                            <i class="fa-brands fa-github text-xl"></i>
                            <span>Continue with GitHub</span>
                        </button>
                    </div>
                    <div class="relative flex py-2 items-center">
                        <div class="flex-grow border-t border-white/10"></div>
                        <span class="flex-shrink-0 mx-4 text-xs text-gray-500">OR</span>
                        <div class="flex-grow border-t border-white/10"></div>
                    </div>
                    <form class="space-y-4 mt-4" onsubmit="event.preventDefault(); closeModal();">
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">Email</label>
                            <input type="email" class="w-full bg-[#1a1a22] border border-white/10 rounded-lg p-3 focus:border-indigo-500 focus:outline-none" placeholder="you@example.com" required>
                        </div>
                        <button type="submit" class="w-full btn-primary py-3">Create Account</button>
                    </form>
                    <div class="mt-6 text-center text-sm text-gray-500">
                        Already have an account? <button onclick="openModal('login')" class="text-indigo-400 hover:underline">Sign in</button>
                    </div>
                `;
            } else if (type === 'contact') {
                content = `
                    <h2 class="text-2xl font-bold mb-6">Contact Sales</h2>
                    <form class="space-y-4" onsubmit="event.preventDefault(); closeModal();">
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">Company</label>
                            <input type="text" class="w-full bg-[#1a1a22] border border-white/10 rounded-lg p-3 focus:border-indigo-500 focus:outline-none" placeholder="Acme Corp" required>
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">Email</label>
                            <input type="email" class="w-full bg-[#1a1a22] border border-white/10 rounded-lg p-3 focus:border-indigo-500 focus:outline-none" placeholder="you@company.com" required>
                        </div>
                        <div>
                            <label class="block text-sm text-gray-400 mb-2">Message</label>
                            <textarea class="w-full bg-[#1a1a22] border border-white/10 rounded-lg p-3 focus:border-indigo-500 focus:outline-none h-24" placeholder="Tell us about your needs..." required></textarea>
                        </div>
                        <button type="submit" class="w-full btn-primary py-3">Send Message</button>
                    </form>
                `;
            } else if (type === 'demo') {
                content = `
                    <div class="text-center">
                        <div class="w-16 h-16 bg-indigo-500/20 rounded-full flex items-center justify-center mx-auto mb-4 text-indigo-400 text-2xl">
                            <i class="fa-solid fa-play"></i>
                        </div>
                        <h2 class="text-2xl font-bold mb-2">Product Demo</h2>
                        <p class="text-gray-400 mb-6">Experience the NexusAI platform in action.</p>
                        <div class="w-full aspect-video bg-black/50 rounded-xl flex items-center justify-center border border-white/10">
                            <div class="spinner"></div>
                        </div>
                        <p class="text-xs text-gray-500 mt-4">Demo placeholder - In production, video would load here.</p>
                    </div>
                `;
            }

            modalBody.innerHTML = content;
        }

        function openToolDetails(id) {
            const tool = toolsData.find(t => t.id === id);
            if (!tool) return;

            modalOverlay.classList.add('active');
            modalBody.innerHTML = `
                <div class="flex items-center gap-3 mb-4">
                    <div class="w-12 h-12 rounded-lg bg-white/10 flex items-center justify-center ${tool.color} text-xl">
                        <i class="fa-solid ${tool.icon}"></i>
                    </div>
                    <div>
                        <h2 class="text-2xl font-bold">${tool.name}</h2>
                        <span class="text-xs text-gray-400 capitalize">${tool.category}</span>
                    </div>
                </div>
                
                <p class="text-gray-300 mb-6 leading-relaxed">${tool.desc}. Access ${tool.name} directly through the NexusAI unified dashboard. Powered by our high-speed routing algorithm.</p>
                
                <div class="glass p-4 rounded-xl mb-6">
                    <h3 class="font-semibold text-sm mb-2 text-indigo-300">Integrated Features</h3>
                    <ul class="text-sm text-gray-400 space-y-1">
                        <li><i class="fa-solid fa-check text-indigo-500 mr-2"></i> Instant Access</li>
                        <li><i class="fa-solid fa-check text-indigo-500 mr-2"></i> No API Keys Required</li>
                        <li><i class="fa-solid fa-check text-indigo-500 mr-2"></i> Unified History</li>
                    </ul>
                </div>

                <div class="flex gap-3">
                    <button class="flex-1 btn-primary py-3" onclick="openModal('login')">Launch Tool</button>
                    <button class="px-4 py-3 rounded-xl border border-white/10 hover:bg-white/10" onclick="closeModal()">Close</button>
                </div>
            `;
        }

        function closeModal(event) {
            if (event && event.target !== modalOverlay && event.target !== modalBody && !event.target.closest('.modal-content')) {
                return;
            }
            modalOverlay.classList.remove('active');
        }

        // --- UI HELPERS ---
        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        }

        function scrollToTools() {
            document.getElementById('tools').scrollIntoView({ behavior: 'smooth' });
        }

        // --- THREE.JS BACKGROUND (Floating Orbs) ---
        function initThreeJS() {
            const container = document.getElementById('canvas-container');
            
            // Scene Setup
            const scene = new THREE.Scene();
            const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
            
            renderer.setSize(window.innerWidth, window.innerHeight);
            container.appendChild(renderer.domElement);

            // Particles/Orbs
            const geometry = new THREE.SphereGeometry(0.5, 32, 32);
            const material = new THREE.MeshBasicMaterial({ 
                color: 0x6366f1, 
                wireframe: true,
                transparent: true,
                opacity: 0.3
            });

            const particles = [];
            const particleCount = 25;

            for (let i = 0; i < particleCount; i++) {
                const mesh = new THREE.Mesh(geometry, material.clone());
                
                // Random positions
                mesh.position.x = (Math.random() - 0.5) * 50;
                mesh.position.y = (Math.random() - 0.5) * 50;
                mesh.position.z = (Math.random() - 0.5) * 50;
                
                // Random scales
                const scale = Math.random() * 0.5 + 0.1;
                mesh.scale.set(scale, scale, scale);

                // Random colors
                const colors = [0x6366f1, 0xa855f7, 0xec4899, 0x3b82f6];
                mesh.material.color.setHex(colors[Math.floor(Math.random() * colors.length)]);

                scene.add(mesh);
                particles.push({
                    mesh: mesh,
                    speedX: (Math.random() - 0.5) * 0.02,
                    speedY: (Math.random() - 0.5) * 0.02,
                    speedZ: (Math.random() - 0.5) * 0.02,
                    rotSpeed: (Math.random() - 0.5) * 0.02
                });
            }

            camera.position.z = 20;

            // Animation Loop
            function animate() {
                requestAnimationFrame(animate);

                particles.forEach(p => {
                    p.mesh.position.x += p.speedX;
                    p.mesh.position.y += p.speedY;
                    p.mesh.position.z += p.speedZ;
                    p.mesh.rotation.x += p.rotSpeed;
                    p.mesh.rotation.y += p.rotSpeed;

                    // Boundary check - bounce back
                    if (Math.abs(p.mesh.position.x) > 25) p.speedX *= -1;
                    if (Math.abs(p.mesh.position.y) > 25) p.speedY *= -1;
                    if (Math.abs(p.mesh.position.z) > 10) p.speedZ *= -1;
                });

                renderer.render(scene, camera);
            }

            animate();

            // Handle Resize
            window.addEventListener('resize', () => {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
            });
        }
    </script>
</body>
</html>
