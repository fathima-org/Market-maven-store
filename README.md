<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AURUM LUXE 2.1: Robust Retail Vault</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for the Premium Black/Gold Theme */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0A0A0A; /* Deepest Black Background */
            color: #E5E5E5; /* Off-White Text */
        }
        .premium-bg {
            background-color: #1A1A1A; /* Slightly Lighter Black for contrast */
        }
        .premium-gold {
            color: #C9A464; /* Sophisticated Pale Gold */
        }
        .premium-border {
            border-color: #2D2D2D;
        }
        .btn-premium {
            background-color: #C9A464;
            color: #0A0A0A; /* Black text on Gold */
            transition: background-color 0.2s, transform 0.1s, box-shadow 0.2s;
            font-weight: 700;
            letter-spacing: 0.025em;
        }
        .btn-premium:hover:not(:disabled) {
            background-color: #B28F50;
            transform: translateY(-2px);
            box-shadow: 0 6px 15px -3px rgba(201, 164, 100, 0.4);
        }
        .btn-premium:disabled {
            background-color: #444444;
            color: #777777;
            cursor: not-allowed;
        }
        .card-shadow {
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
            transition: all 0.3s ease;
        }
        .card-shadow:hover {
            box-shadow: 0 8px 20px rgba(201, 164, 100, 0.15);
        }
        /* Focus styles for inputs */
        input:focus, textarea:focus {
            border-color: #C9A464 !important;
            box-shadow: 0 0 0 3px rgba(201, 164, 100, 0.3);
            background-color: #1A1A1A;
            color: #fff;
        }
        /* Hide scrollbar but allow scrolling */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        /* Loading spinner animation */
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .spinner {
            animation: spin 1s linear infinite;
        }
    </style>
</head>
<body>
    <!-- Main App Container -->
    <div id="app" class="min-h-screen">
        
        <!-- Header (Sticky, Deep Black) -->
        <header id="main-header" class="bg-[#0A0A0A] sticky top-0 z-40 shadow-2xl border-b border-[#2D2D2D]">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3 flex justify-between items-center">
                <!-- Brand Name -->
                <a href="#" class="flex items-center space-x-2">
                    <span class="premium-gold text-2xl sm:text-3xl font-extrabold flex items-center">
                        <svg class="w-7 h-7 sm:w-8 sm:h-8 mr-2 fill-current" viewBox="0 0 24 24"><path d="M12 2L2 12l10 10 10-10L12 2zm0 18.5L4.5 12 12 4.5 19.5 12 12 20.5z"/><path d="M12 8l-2 4h4l-2 4"/></svg>
                        AURUM LUXE
                    </span>
                </a>
                
                <!-- Search Bar (Desktop/Tablet) -->
                <div class="flex flex-1 max-w-lg mx-4 relative hidden sm:flex">
                    <input type="text" id="search-input" placeholder="Search 60 Million assets..."
                        class="flex-1 p-3 text-white rounded-l-xl focus:outline-none border-2 border-[#333] focus:premium-border bg-[#1A1A1A] transition"
                    />
                    <button id="search-button" class="p-3 btn-premium rounded-r-xl" aria-label="Search">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
                    </button>
                </div>

                <!-- User and Cart Button -->
                <div class="flex items-center space-x-4 sm:space-x-6">
                    <div id="user-display" class="hidden md:flex text-sm text-gray-500 font-medium items-center">
                         <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-5 h-5 mr-1 premium-gold"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
                        Authenticating...
                    </div>
                    <button id="cart-button" class="relative p-2.5 sm:p-3 rounded-xl bg-[#1A1A1A] hover:bg-[#2A2A2A] transition shadow-inner border border-[#2D2D2D]" aria-label="Open Shopping Cart">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#C9A464" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><circle cx="9" cy="21" r="1"/><circle cx="20" cy="21" r="1"/><path d="M1 1h4l2.68 12.08a2 2 0 0 0 2 1.92h9.23a2 2 0 0 0 2-1.92L23 6H6"/></svg> 
                        <span id="cart-count" class="absolute top-0 right-0 transform translate-x-1/4 -translate-y-1/4 bg-red-600 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center border-2 border-[#0A0A0A]">
                            0
                        </span>
                    </button>
                </div>
            </div>
        </header>

        <!-- Mobile Search Bar -->
        <div class="sm:hidden px-4 py-3 bg-[#111] border-b border-[#2D2D2D] shadow-md">
            <div class="flex relative">
                <input type="text" id="search-input-mobile" placeholder="Search 60 Million assets..."
                    class="flex-1 p-3 text-white rounded-l-xl focus:outline-none border-2 border-[#333] focus:premium-border bg-[#1A1A1A] transition"
                />
                <button id="search-button-mobile" class="p-3 btn-premium rounded-r-xl" aria-label="Search">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
                </button>
            </div>
        </div>

        <!-- Category Navigation Bar (Scrollable) -->
        <div class="bg-[#111] border-b border-[#2D2D2D] sticky top-[52px] sm:top-[75px] z-30 shadow-md">
            <div id="category-nav" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-2 flex space-x-3 overflow-x-auto whitespace-nowrap no-scrollbar">
                <!-- Category buttons will be inserted here -->
            </div>
        </div>
        
        <!-- Main Content Area -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">
            <h2 class="text-3xl font-extrabold text-white mb-8 flex items-center premium-gold">
                 <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="premium-gold mr-3"><path d="M22 11V6a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h8"/><rect x="4" y="8" width="16" height="4"/><path d="M15 22v-4"/><path d="M15 18a4 4 0 0 1 4-4h2a2 2 0 0 1 2 2v1a2 2 0 0 1-2 2h-2a4 4 0 0 1-4-4z"/></svg>
                The Exclusive Collection of 60 Million Assets
            </h2>

            <!-- Loading Indicator & Product Grid -->
            <div id="loading-indicator" class="premium-bg p-8 rounded-xl text-center">
                <svg class="spinner w-8 h-8 text-white mx-auto mb-3 premium-gold" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path>
                </svg>
                <p class="text-white premium-gold font-semibold">Authorizing access to the private vault...</p>
                <p class="text-xs text-gray-500 mt-1">Fetching initial inventory and authentication status.</p>
            </div>
            
            <div id="product-grid" class="hidden grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4 sm:gap-6">
                <!-- Product cards will be inserted here -->
            </div>
        </main>
        
        <!-- Variant Selection Modal (Product Details) -->
        <div id="variant-modal" class="hidden fixed inset-0 bg-black/95 z-50 items-center justify-center p-4">
            <div id="variant-content" class="premium-bg rounded-2xl shadow-2xl w-full max-w-xl max-h-[95vh] overflow-y-auto p-6 border-2 premium-border transform transition-all duration-300 scale-95 opacity-0">
                <div class="flex justify-between items-start border-b premium-border pb-4 mb-4">
                    <h3 id="variant-title" class="text-2xl font-bold text-white">Configure Your Order</h3>
                    <button id="close-variant-btn" class="text-gray-400 hover:premium-gold transition p-2 rounded-full hover:bg-[#2A2A2A]">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6L6 18"/><path d="M6 6l12 12"/></svg>
                    </button>
                </div>
                
                <div id="variant-product-details" class="flex flex-col sm:flex-row gap-6 mb-6">
                    <!-- Image and base price -->
                </div>
                
                <div id="variant-options-container" class="space-y-6 border-t border-[#2D2D2D] pt-6">
                    <!-- Variant selectors will be dynamically rendered here -->
                </div>
                
                <div class="flex flex-col sm:flex-row justify-between items-center mt-8 pt-6 border-t premium-border">
                    <div class="text-xl font-bold text-white mb-4 sm:mb-0">
                        Total Price: <span id="variant-price-display" class="text-3xl premium-gold">₹0</span>
                    </div>
                    <div class="flex space-x-4 w-full sm:w-auto">
                        <button id="buy-now-btn" class="flex-1 py-3 px-5 rounded-xl text-lg font-semibold bg-[#2A2A2A] text-white border border-gray-600 hover:border-white transition-colors">
                            Buy Now
                        </button>
                        <button id="add-to-cart-btn-modal" class="flex-1 py-3 px-5 rounded-xl text-lg font-semibold btn-premium transition-colors">
                            Add to Portfolio
                        </button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Cart Modal -->
        <div id="cart-modal" class="hidden fixed inset-0 bg-black/70 z-50 flex justify-end">
            <div id="cart-content" class="w-full sm:w-[480px] bg-[#111] h-full p-6 sm:p-8 flex flex-col shadow-2xl border-l premium-border transform translate-x-full transition-transform duration-300">
                <div class="flex justify-between items-center border-b premium-border pb-5 mb-5">
                    <h2 class="text-2xl font-bold text-white flex items-center premium-gold">
                        <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-7 h-7 mr-3"><circle cx="9" cy="21" r="1"/><circle cx="20" cy="21" r="1"/><path d="M1 1h4l2.68 12.08a2 2 0 0 0 2 1.92h9.23a2 2 0 0 0 2-1.92L23 6H6"/></svg>
                        Your Portfolio
                    </h2>
                    <button id="close-cart-btn" class="text-gray-400 hover:premium-gold transition p-2 rounded-full bg-[#222]">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6L6 18"/><path d="M6 6l12 12"/></svg>
                    </button>
                </div>
                
                <div id="cart-items-container" class="flex-grow overflow-y-auto space-y-5 pr-2">
                    <!-- Cart items will be inserted here -->
                </div>

                <div class="border-t premium-border pt-5 mt-5">
                    <div id="cart-summary" class="text-sm space-y-2 mb-3 text-gray-400">
                        <!-- Summary details (Subtotal, Tax, Shipping) -->
                    </div>

                    <div class="flex justify-between text-xl sm:text-2xl font-extrabold mb-5 border-t border-[#222] pt-3">
                        <span class='text-white'>Grand Total:</span>
                        <span id="grand-total-display" class="premium-gold">₹0</span>
                    </div>

                    <button id="checkout-button" class="w-full py-4 btn-premium text-xl font-bold rounded-xl shadow-2xl" disabled>
                        Authorize Purchase
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Checkout Modal (Simulated Payment) -->
        <div id="checkout-modal" class="hidden fixed inset-0 bg-black/95 z-50 items-center justify-center p-4">
            <div id="checkout-content" class="premium-bg rounded-2xl shadow-2xl w-full max-w-sm p-6 sm:p-8 border-2 premium-border transform transition-all duration-300 scale-95 opacity-0">
                <div class="flex justify-between items-start border-b premium-border pb-4 mb-6">
                    <h3 class="text-2xl font-bold text-white">Exclusive Transaction</h3>
                    <button id="close-checkout-btn" class="text-gray-400 hover:premium-gold transition p-2 rounded-full hover:bg-[#2A2A2A]">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6L6 18"/><path d="M6 6l12 12"/></svg>
                    </button>
                </div>
                
                <form id="payment-form" class="space-y-6">
                    <h4 class="text-lg font-semibold premium-gold border-b border-[#2D2D2D] pb-2">Delivery Contact & Address</h4>
                    <div>
                        <label for="phone-number" class="block text-sm font-medium text-gray-400">Secure Phone Number</label>
                        <input type="tel" id="phone-number" name="phone-number" required
                               class="mt-1 block w-full rounded-lg border-gray-700 shadow-sm p-3 border focus:premium-border bg-[#0A0A0A] text-white" placeholder="+91 XXXXXXXXXX">
                    </div>
                    <div>
                        <label for="shipping-address" class="block text-sm font-medium text-gray-400">Exclusive Drop Address</label>
                        <textarea id="shipping-address" name="shipping-address" required rows="3"
                               class="mt-1 block w-full rounded-lg border-gray-700 shadow-sm p-3 border focus:premium-border bg-[#0A0A0A] text-white"></textarea>
                    </div>

                    <h4 class="text-lg font-semibold pt-4 border-t border-[#2D2D2D] mt-4 premium-gold border-b pb-2">Payment Authorization</h4>
                    <div>
                        <label for="credit-card" class="block text-sm font-medium text-gray-400">Encrypted Card Number (Payment Method)</label>
                        <input type="text" id="credit-card" name="credit-card" required pattern="[0-9]{16}"
                               class="mt-1 block w-full rounded-lg border-gray-700 shadow-sm p-3 border focus:premium-border bg-[#0A0A0A] text-white" placeholder="XXXX XXXX XXXX XXXX">
                    </div>
                    
                    <button type="submit" class="w-full py-4 btn-premium text-xl font-bold rounded-xl mt-8 shadow-lg">
                        Confirm Purchase <span id="final-price-checkout"></span>
                    </button>
                </form>
            </div>
        </div>

        <!-- Global Message Box (Alert/Confirm replacement) -->
        <div id="global-message-box" class="hidden fixed inset-0 bg-black/95 z-[60] items-center justify-center p-4">
            <div id="message-content" class="premium-bg p-8 rounded-2xl shadow-2xl w-full max-w-sm text-center border-2 premium-border transform transition-all duration-300 ease-in-out scale-95 opacity-0">
                <div id="message-icon" class="mx-auto mb-4"></div>
                <h3 id="message-title" class="text-xl font-bold text-white mb-3"></h3>
                <p id="message-body" class="text-gray-400 mb-6"></p>
                <button id="message-ok-btn" class="w-full py-3 btn-premium font-semibold rounded-xl transition">
                    Acknowledge
                </button>
            </div>
        </div>

        <!-- Footer -->
        <footer class="bg-[#0A0A0A] text-gray-500 py-8 mt-16 border-t border-[#2D2D2D]">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-sm">
                <p class="text-xl font-extrabold premium-gold mb-3">AURUM LUXE</p>
                <p class="mb-3 text-gray-600">Discretion. Opulence. Legacy.</p>
                <p id="footer-userid" class="text-xs text-gray-700">Client ID: Loading...</p>
                <p class="text-xs text-gray-700 mt-2">Personal Concierge Hotline: +91 99999 00000</p>
            </div>
        </footer>

    </div>

    <!-- Firebase Script and Application Logic -->
    <script type="module">
        // --- FIREBASE IMPORTS ---
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { 
            getFirestore, 
            doc, 
            setDoc, 
            onSnapshot, 
            collection, 
            query, 
            getDocs,
            deleteDoc,
            setLogLevel 
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- GLOBAL CONFIGS & FIREBASE SETUP ---
        
        const TAX_RATE = 0.05; // 5% GST
        const SHIPPING_CHARGE = 500000.00; // Half a million rupees
        const FREE_SHIPPING_THRESHOLD = 50000000.00; // 50 Million Rupees (5 Crore)
        const SALES_OFFER_THRESHOLD = 100000000.00; // 100 Million Rupees (10 Crore) - 10% Off
        
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null; // Fixed typo

        let app;
        let db;
        let auth;
        let userId = null;
        let isAuthReady = false;
        let allProducts = [];
        let cartItems = [];
        let selectedCategory = 'All';
        let currentSearchTerm = '';
        let selectedProductForVariant = null; 
        let currentVariants = {}; 

        // Function to generate the massive, diverse set of mock products
        const generateDiverseMockProducts = () => {
            const productBase = [
                // --- HIGH-VALUE ASSETS (Core Luxuries) ---
         
