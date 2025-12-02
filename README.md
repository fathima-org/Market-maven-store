<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mega Store | Product Variants & Search</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Configure Tailwind with the Black/Pink/Brown/White theme -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'primary-black': '#121212',      /* Deep Black for text/headers */
                        'primary-pink': '#FF4081',       /* Vibrant Pink for CTA/Accent (High-Energy) */
                        'secondary-brown': '#A0522D',    /* Sienna Brown for secondary accents/contrast */
                        'soft-bg': '#FAFAFA',            /* Off-white body background */
                        'card-white': '#FFFFFF',         /* Pure White for cards/containers */
                        'deal-tag': '#E91E63',           /* Slightly darker pink for deal tags */
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        /* Custom Styles for the professional Black/Pink look */
        body {
            background-color: #FAFAFA; /* Soft background */
            padding-bottom: 60px;
        }
        .header-top { background-color: #121212; }
        .location-bar {
            background-color: #A0522D; /* Secondary Brown */
            color: #FFFFFF;
            padding: 8px 16px;
            cursor: pointer;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
        .search-input-container {
            border: 2px solid #FF4081; /* Pink border on search bar */
            border-radius: 8px;
            overflow: hidden;
        }
        .category-scroller {
            overflow-x: scroll;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
            background-color: #FFFFFF; 
            border-bottom: 1px solid #F0F0F0;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }
        .category-scroller::-webkit-scrollbar {
            display: none;
        }
        /* Modal Overlay */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .modal-content {
            background-color: #FFFFFF;
            color: #121212;
            max-width: 95%;
            width: 450px;
            height: 90vh; /* Fixed height for scrollable content */
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
            display: flex;
            flex-direction: column;
        }
        /* Loading Spinner */
        .loader {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #FF4081; /* Pink spinner */
            border-radius: 50%;
            width: 30px;
            height: 30px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="font-sans min-h-screen flex flex-col text-primary-black">

    <!-- === HEADER: SEARCH BAR & LOCATION BAR === -->
    <header class="sticky top-0 z-50">
        <!-- Top Search Area (Primary Black) -->
        <div class="header-top p-2 flex items-center space-x-2">
            <!-- Search Bar -->
            <div class="flex-grow flex items-center bg-card-white rounded-md shadow-md search-input-container">
                <span class="p-2 text-gray-500">🔍</span>
                <input type="text" id="search-input" placeholder="Search products by name or category..." 
                       class="flex-grow p-2 text-base focus:outline-none rounded-md" onkeydown="if(event.key === 'Enter') handleSearch()">
                <button onclick="handleSearch()" class="p-2 text-primary-pink hover:text-deal-tag transition duration-150">
                    🔎
                </button>
            </div>
            <!-- Empty space where notification icon was -->
            <div class="w-8 h-6"></div>
        </div>

        <!-- === LOCATION BAR (Secondary Brown) === -->
        <div class="location-bar flex justify-between items-center text-sm" onclick="openLocationModal()">
            <div class="flex items-center space-x-1 font-medium">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd" />
                </svg>
                <span id="delivery-location-text">Delivering to: <strong id="current-location-text"></strong> - Update PIN</span>
            </div>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white/80" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
        </div>

        <!-- Filter/Sort Bar -->
        <div class="sticky top-[78px] bg-card-white shadow-sm flex border-b border-gray-100 z-40">
            <button onclick="openFilterModal()" class="flex-1 p-3 flex justify-center items-center space-x-2 border-r border-gray-100 text-sm font-semibold text-primary-black hover:bg-gray-50">
                <span class="text-xl">⚙️</span>
                <span>Filter</span>
            </button>
            <button onclick="showCustomMessage('Sorting by Price: Low to High', 'info')" class="flex-1 p-3 flex justify-center items-center space-x-2 text-sm font-semibold text-primary-black hover:bg-gray-50">
                <span class="text-xl">↕️</span>
                <span>Sort</span>
            </button>
        </div>
    </header>

    <!-- === MAIN CONTENT VIEW CONTAINER === -->
    <main id="view-container" class="flex-grow max-w-7xl mx-auto w-full"></main>

    <!-- === INFINITE SCROLL LOADER === -->
    <div id="loading-indicator" class="hidden my-4 py-4 flex justify-center items-center">
        <div class="loader mr-3"></div>
        <p class="text-primary-black font-semibold">Loading more products...</p>
    </div>

    <!-- === MODALS (Hidden by default) === -->
    <div id="modal-overlay" class="modal-overlay" onclick="if(event.target.id === 'modal-overlay') closeModal()">
        <div id="modal-content" class="modal-content">
            <!-- Content injected here for Location, Filter, or Product Detail -->
        </div>
    </div>
    
    <!-- === FIXED BOTTOM NAVIGATION BAR (Primary Black, Pink Accent) === -->
    <nav class="fixed bottom-0 left-0 w-full bg-primary-black border-t border-primary-pink z-50 shadow-2xl">
        <div class="flex justify-around items-center h-14 max-w-xl mx-auto">
            <button onclick="changeView('home')" class="flex flex-col items-center text-xs font-medium text-primary-pink w-1/2">
                <span class="text-xl">🏠</span>
                Home
            </button>
            <button onclick="changeView('cart')" class="flex flex-col items-center text-xs font-medium text-white/70 hover:text-white w-1/2 relative">
                <span class="text-xl">🛒</span>
                Cart (<span id="cart-count" class="absolute top-0 right-1/4 bg-deal-tag text-white text-[10px] px-1 rounded-full -mt-1 -mr-2">0</span>)
            </button>
        </div>
    </nav>


    <!-- === SCRIPTS === -->
    <script>
        // --- CONSTANTS ---
        const DELIVERY_CHARGE = 80.00; 
        const BATCH_SIZE = 10; 
        
        // Mock Categories (Electronics and Fashion heavy)
        const CATEGORIES = [
            { name: "Electronics", icon: "💻" },
            { name: "Fashion", icon: "👕" },
            { name: "Home & Decor", icon: "🛋️" },
            { name: "Stationary & Books", icon: "📚" },
            { name: "Kids & Toys", icon: "🧸" },
            { name: "Accessories", icon: "👜" },
        ];
        
        // Global State
        let currentLocation = { city: 'New Delhi', pinCode: '110001' }; 
        let cart = []; 
        let currentPage = 0;
        let isLoading = false;
        let totalProductsSimulated = 400000; 
        let currentSearchQuery = '';
        window.selectedVariants = { color: null, size: null }; // Global variant state for modal

        // --- UTILITY FUNCTIONS ---
        // Function to format price into Indian Rupees (INR) format
        const formatPrice = (price) => `₹ ${price.toLocaleString('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;

        function getRandomPrice(min, max) {
            return (Math.random() * (max - min) + min).toFixed(2);
        }

        function closeModal() {
            document.getElementById('modal-overlay').style.display = 'none';
        }

        function showCustomMessage(message, type = 'info') {
            const colorMap = {
                success: 'bg-primary-pink text-white',
                info: 'bg-secondary-brown text-white',
                error: 'bg-deal-tag text-white'
            };

            const messageBox = document.createElement('div');
            messageBox.className = `fixed top-20 right-5 z-[1001] p-4 rounded-lg shadow-xl ${colorMap[type]} transition-opacity duration-300`;
            messageBox.textContent = message;

            document.body.appendChild(messageBox); 

            setTimeout(() => {
                messageBox.style.opacity = '0';
                setTimeout(() => messageBox.remove(), 300);
            }, 3000);
        }
        
        // --- PRODUCT DATA GENERATION ---

        // Static list of possible product titles for better search matching
        const MOCK_PRODUCT_NAMES = [
            // User-requested items - now reflecting higher value
            "Exclusive Collector's Edition Book", "Hand-Woven Pure Silk Saree (Designer)", "Luxury Embroidered Churidar Set", 
            "Premium Smart Home Automation System", "Executive Stationary Desk Organizer", "Bridal Heavy Work Lehenga Choli", 
            "Professional Marathon Running Shoes (Carbon Plate)", "Italian Leather Designer Hand Bag", "Advanced Trauma Medicine Pouch Kit", 
            "Architectural Grade Pencil Set", "Giant Industrial Eraser", "Smart Temperature Control Bottle", 
            "Premium Insulated Lunch Box Bag", "Ergonomic School Kit Backpack Pro", "University-Level Science Kit", 
            "Handmade Leather Journal Kit", "Artisan Glazed Flowerpot", "High-End Business Tablet (5G)", 
            "Flagship 5G Smart Phone Pro", "Rare Academic Textbooks Set", "Professional Calligraphy Pens Set",
            
            // Existing high-value items
            "Luxury Smart Watch with ECG", "Audiophile Noise-Cancelling Headphones", "High-Fidelity Bluetooth Speaker", "8K QLED Ultra HD TV",
            "Japanese Selvedge Denim Jeans", "Organic Pima Cotton T-Shirt", "Bespoke Leather Wallet", 
            "Imported Modular Sofa Set", "Original Oil Painting Wall Art", "Zero Gravity Massage Chair", "Whole-Home Smart Hub Ecosystem",
            "Advanced STEM Coding Robot Kit", "Premium Interactive Learning Tablet", "Collector's Edition Building Blocks",
            "Professional Match Football", "Smart Tracking Yoga Mat", "High-Performance Electric Scooter"
        ];


        function generateProductData(index) {
            const id = index;
            // Use modulo operator to assign categories from the smaller CATEGORIES list
            const categoryIndex = id % CATEGORIES.length;
            const category = CATEGORIES[categoryIndex];
            
            // *** CHANGE: INCREASED PRICE RANGE FOR MORE COSTLY ITEMS ***
            // Minimum price is now 5,000 INR, Maximum is 250,000 INR
            const price = parseFloat(getRandomPrice(5000, 250000)); 
            // ************************************************************

            const originalPrice = (price * 1.5).toFixed(2); // Still 50% markup for original price
            const discount = Math.round(((originalPrice - price) / originalPrice) * 100);
            const rating = (Math.random() * 1.5 + 3.5).toFixed(1); 
            const reviews = Math.floor(Math.random() * 9999) + 100;
            const imageSize = 400 + (id % 10);
            const isDeal = id % 5 === 0;
            
            // Assign a name from the static list for better search targeting
            const baseName = MOCK_PRODUCT_NAMES[id % MOCK_PRODUCT_NAMES.length];

            const productDetails = {
                id,
                name: `${baseName} - Model ${id % 100}`, // Ensure unique name by adding model number
                category,
                price,
                originalPrice: parseFloat(originalPrice),
                discount,
                rating,
                reviews: reviews.toLocaleString(),
                image: `https://placehold.co/${imageSize}x${imageSize}/121212/FAFAFA?text=P${id}`,
                isDeal,
                // Variant Data
                sizes: category.name === 'Fashion' || category.name === 'Kids & Toys' ? ['S', 'M', 'L', 'XL'] : ['One Size'],
                colors: [{ name: 'Black', hex: '#121212' }, { name: 'Pink', hex: '#FF4081' }, { name: 'Brown', hex: '#A0522D' }, { name: 'White', hex: '#FFFFFF' }],
            };

            // Search filtering logic
            if (currentSearchQuery) {
                const query = currentSearchQuery.toLowerCase();
                const matches = productDetails.name.toLowerCase().includes(query) || 
                                productDetails.category.name.toLowerCase().includes(query) || 
                                productDetails.id.toString().includes(query);
                if (!matches) return null;
            }

            return productDetails;
        }

        // --- PRODUCT DETAIL & VARIANT LOGIC ---

        function selectVariant(element, type) {
            const container = document.getElementById(`${type}-selector`);
            
            if (type === 'color') {
                container.querySelectorAll('.color-option').forEach(el => el.style.border = '2px solid transparent');
                element.style.border = '2px solid #FF4081'; // Pink border for selected color
                document.getElementById('selected-color').textContent = element.dataset.colorName;
            } else if (type === 'size') {
                container.querySelectorAll('.size-option').forEach(el => {
                    el.classList.remove('bg-primary-pink', 'text-card-white');
                    el.classList.add('bg-card-white', 'text-primary-black');
                });
                element.classList.add('bg-primary-pink', 'text-card-white');
                element.classList.remove('bg-card-white', 'text-primary-black');
                document.getElementById('selected-size').textContent = element.dataset.sizeName;
            }
            
            // Update state
            window.selectedVariants[type] = element.dataset[`${type}Name`];
        }

        function validateDetails() {
            const address = document.getElementById('delivery-address')?.value.trim();
            const phone = document.getElementById('delivery-phone')?.value.trim();
            const size = window.selectedVariants?.size;
            const color = window.selectedVariants?.color;

            // Only require size/color if there are options other than 'One Size'
            const sizeSelector = document.getElementById('size-selector');
            if (sizeSelector && sizeSelector.querySelectorAll('.size-option').length > 1 && !size) { 
                showCustomMessage('Please select a size.', 'error'); 
                return false; 
            }
            if (!color) { showCustomMessage('Please select a color.', 'error'); return false; }
            if (!address || address.length < 5) { showCustomMessage('Please enter a valid delivery address.', 'error'); return false; }
            if (!phone || !phone.match(/^[0-9]{10}$/)) { showCustomMessage('Please enter a valid 10-digit phone number.', 'error'); return false; }
            
            return true;
        }

        function renderProductDetailModal(productId) {
            const product = generateProductData(productId); 
            if (!product) return; 

            const modalOverlay = document.getElementById('modal-overlay');
            const modalContent = document.getElementById('modal-content');
            
            // Set initial selected variant state
            window.selectedVariants = { 
                color: product.colors[0].name, 
                size: product.sizes[0] // Set to the first available size
            };

            // Generate color options
            const colorOptionsHtml = product.colors.map((color, index) => {
                const isSelected = index === 0;
                return `
                    <div data-color-name="${color.name}" onclick="selectVariant(this, 'color')" 
                         class="color-option w-8 h-8 rounded-full border-2 border-transparent cursor-pointer transition hover:scale-110 ${isSelected ? 'border-primary-pink' : ''}" 
                         style="background-color: ${color.hex}; outline: 2px solid ${color.hex === '#FFFFFF' ? '#121212' : '#FFFFFF'};"
                         title="${color.name}">
                    </div>
                `;
            }).join('');

            // Generate size options (conditionally hidden if only 'One Size' is present)
            const showSizeSelector = product.sizes.length > 1 || product.sizes[0] !== 'One Size';

            const sizeOptionsHtml = product.sizes.map((size, index) => {
                const isSelected = index === 0;
                return `
                    <button data-size-name="${size}" onclick="selectVariant(this, 'size')"
                            class="size-option px-4 py-2 border border-gray-300 rounded-lg text-sm font-medium transition duration-150 ${isSelected ? 'bg-primary-pink text-card-white' : 'hover:border-primary-pink bg-card-white text-primary-black'}">
                        ${size}
                    </button>
                `;
            }).join('');
            
            const sizeSelectorSection = showSizeSelector ? `
                <div id="size-selector">
                    <h3 class="font-semibold text-primary-black mb-2">Size: <span id="selected-size" class="font-normal text-primary-pink">${product.sizes[0]}</span></h3>
                    <div class="flex space-x-3">
                        ${sizeOptionsHtml}
                    </div>
                </div>
            ` : `<p class="font-semibold text-gray-600">Size: One Size Fits All</p><input type="hidden" id="selected-size" value="One Size">`;


            modalContent.innerHTML = `
                <!-- Header -->
                <div class="p-4 flex justify-between items-center border-b border-gray-200 sticky top-0 bg-card-white z-10">
                    <h2 class="text-xl font-bold text-primary-black">${product.category.name} Item</h2>
                    <button onclick="closeModal()" class="text-gray-400 hover:text-primary-pink text-3xl font-light">&times;</button>
                </div>

                <!-- Scrollable Content -->
                <div class="flex-grow overflow-y-auto p-4 space-y-4">
                    <!--
