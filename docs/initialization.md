James, as an architect, you know that **Browser Security Models** differ fundamentally from Backend environments. A standard browser (Chrome/Safari) cannot natively read a `.env` file from your hard drive because it has no access to your file system (to prevent malicious websites from stealing your data).

To make the `API_KEY` come from a `.env` file while maintaining your "Humble Store" simplicity, the industry standard is to use a **Minimalist Build Tool** like **Vite**. 

This aligns perfectly with your "SmartRecs Hub" tech stack and creates a professional development workflow.

---

### Step 1: The "Humble Store" Repository Setup

Follow these steps in your WSL terminal to create a standalone project that handles `.env` variables correctly.

```bash
# 1. Create and enter the directory
mkdir simple-shopping-cart
cd simple-shopping-cart

# 2. Initialize the project
npm init -y

# 3. Install Vite (The lightweight dev tool you're already using for the Hub)
npm install vite --save-dev

# 4. Create the necessary files
touch index.html .env .gitignore
```

---

### Step 2: The Infrastructure Configuration

#### 1. The `.env` file
Add your sensitive key here.
```text
VITE_SMARTRECS_API_KEY=sr_live_xxx
```

#### 2. The `.gitignore`
**Critical Architect Step:** This ensures your key is never public on GitHub.
```text
node_modules
.env
dist
```

---

### Step 3: Updating the Code (`index.html`)

Vite uses `import.meta.env` to inject variables. I have updated the script block to pull from the environment instead of a hardcoded string.

```html
<!-- Inside index.html - Replace your <script> section with this -->
<script type="module">
    // ==========================================
    // CONFIGURATION (Vite-Powered Environment)
    // ==========================================
    const API_URL = "http://127.0.0.1:8001/recommend";
    
    // FETCHING KEY FROM .env
    const API_KEY = import.meta.env.VITE_SMARTRECS_API_KEY;

    if (!API_KEY) {
        console.error("Architect Warning: API_KEY is missing. Check your .env file.");
    }

    const inventoryItems = ["hot dog", "coke", "chips", "charcoal", "detergent"];
    let cart = [];

    // ... (Your existing addToCart, updateUI functions stay here) ...

    async function getRecommendations() {
        const statusEl = document.getElementById('api-status');
        const recDiv = document.getElementById('recommendations');
        
        const params = new URLSearchParams();
        cart.forEach(item => params.append('items', item));

        try {
            const response = await fetch(`${API_URL}?${params.toString()}`, {
                method: 'GET',
                headers: {
                    'X-API-KEY': API_KEY, // Variable injected here
                    'Accept': 'application/json'
                }
            });

            if (response.ok) {
                const data = await response.json();
                statusEl.innerText = `Online (${data.latency_ms}ms)`;
                statusEl.className = "text-green-500 font-bold";
                renderRecommendations(data.recommendations);
            } else {
                statusEl.innerText = "Auth Failed";
            }
        } catch (error) {
            statusEl.innerText = "Offline (Connect FastAPI)";
        }
    }
    
    // IMPORTANT: Make sure to attach functions to the window object 
    // because Vite modules are scoped.
    window.addToCart = (item) => {
        if (!cart.includes(item)) {
            cart.push(item);
            updateUI();
            getRecommendations();
        }
    };
    window.clearCart = () => {
        cart = [];
        updateUI();
        document.getElementById('recommendations').innerHTML = '<p class="text-xs text-gray-500 italic">Add items...</p>';
    };
    
    // ... (Your renderRecommendations function stays here) ...
</script>
```

---

### Step 4: The GitHub Deployment SOP

To map your local project to a new repository:

1.  **Create the Remote:** Go to [GitHub](https://github.com/new) and create a repository named `simple-shopping-cart`. **Leave it empty** (no README).
2.  **Initialize Git locally:**
    ```bash
    git init
    git add .
    git commit -m "feat: initial store integration with environment variable safety"
    ```
3.  **Map and Push:**
    ```bash
    git branch -M main
    git remote add origin https://github.com/YOUR_USERNAME/simple-shopping-cart.git
    git push -u origin main
    ```

---

