# Gerenciador-de-Produtos
Para poder criar tabela de produtos e gerenciar itens e preços, com imagem e tambem descrição do produto, podendo tambem editar, exportar e importar a tabela em CSV

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciador de Produtos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            transition: background-color 0.3s ease;
        }
        .card {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            border-radius: 1rem;
            transition: background-color 0.3s ease, box-shadow 0.3s ease;
        }
        .message-box {
            display: none;
            padding: 1rem;
            margin-top: 1rem;
            border-radius: 0.5rem;
            font-weight: 500;
        }
        .message-box.success {
            background-color: #d1fae5;
            color: #065f46;
        }
        .message-box.error {
            background-color: #fee2e2;
            color: #991b1b;
        }
        .message-box.info {
            background-color: #dbeafe;
            color: #1e40af;
        }
        .button-gradient {
            background-image: linear-gradient(to right, #4c51bf, #667eea);
            transition: all 0.3s ease;
        }
        .button-gradient:hover {
            background-image: linear-gradient(to right, #5a67d8, #7f9cf5);
        }
        .button-secondary {
            background-color: #cbd5e0;
            color: #2d3748;
            transition: all 0.3s ease;
        }
        .button-secondary:hover {
            background-color: #e2e8f0;
        }
        .offline-status {
            display: none;
            background-color: #f56565;
            color: white;
            padding: 0.5rem;
            text-align: center;
            border-radius: 0.5rem;
            margin-top: 1rem;
        }
        
        /* Dark Mode styles */
        .dark body {
            background-color: #1a202c;
        }
        .dark .card {
            background-color: #2d3748;
            box-shadow: 0 10px 15px -3px rgba(255, 255, 255, 0.1), 0 4px 6px -2px rgba(255, 255, 255, 0.05);
        }
        .dark h1, .dark h2, .dark .text-gray-800 {
            color: #e2e8f0;
        }
        .dark .text-gray-500, .dark .text-gray-700 {
            color: #cbd5e0;
        }
        .dark .bg-gray-50 {
            background-color: #4a5568;
        }
        .dark .bg-white {
            background-color: #2d3748;
        }
        .dark .bg-gray-100 {
            background-color: #4a5568;
        }
        .dark input, .dark textarea, .dark select {
            background-color: #4a5568;
            color: #e2e8f0;
            border-color: #616e7f;
        }
        .dark table {
            background-color: #2d3748;
        }
        .dark .divide-gray-200 {
            border-color: #4a5568;
        }
        .dark .hover\:bg-gray-50:hover {
            background-color: #4a5568;
        }
        .dark .text-indigo-600 {
            color: #9f7aea;
        }
        .dark .text-red-600 {
            color: #f56565;
        }
    </style>
</head>
<body class="bg-gray-100 p-4 sm:p-8">

    <div class="max-w-5xl mx-auto bg-white rounded-2xl card p-6 sm:p-10 space-y-8">
        <div class="flex justify-between items-center">
            <h1 class="text-4xl font-extrabold text-gray-800 tracking-wide">Gerenciador de Produtos</h1>
            <button id="theme-toggle" class="p-2 rounded-full focus:outline-none transition-transform transform hover:scale-110">
                <svg id="sun-icon" class="w-6 h-6 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
                </svg>
                <svg id="moon-icon" class="w-6 h-6 text-indigo-300 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path>
                </svg>
            </button>
        </div>
        <div id="offline-message" class="offline-status">
            Você está offline. As alterações serão salvas localmente e sincronizadas quando a conexão for restaurada.
        </div>

        <!-- Formulário para adicionar produto -->
        <div class="bg-gray-50 p-6 sm:p-8 rounded-xl space-y-6">
            <h2 class="text-2xl font-semibold text-gray-700">Adicionar Novo Produto</h2>
            <form id="product-form" class="space-y-4">
                <input type="hidden" id="product-id">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label for="product-name" class="block text-sm font-medium text-gray-700">Nome do Produto</label>
                        <input type="text" id="product-name" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                    </div>
                    <div>
                        <label for="product-category" class="block text-sm font-medium text-gray-700">Categoria</label>
                        <input type="text" id="product-category" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                    </div>
                    <div>
                        <label for="purchase-price" class="block text-sm font-medium text-gray-700">Preço de Compra (R$)</label>
                        <input type="number" step="0.01" id="purchase-price" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                    </div>
                    <div>
                        <label for="profit-percentage" class="block text-sm font-medium text-gray-700">Margem de Lucro (%)</label>
                        <input type="number" step="0.01" id="profit-percentage" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                    </div>
                </div>
                <div>
                    <label for="product-image" class="block text-sm font-medium text-gray-700">URL da Imagem</label>
                    <input type="url" id="product-image" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                </div>
                <div>
                    <label for="product-description" class="block text-sm font-medium text-gray-700">
                        Descrição do Produto
                    </label>
                    <textarea id="product-description" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white"></textarea>
                    <button type="button" id="generate-description-btn" class="mt-2 w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white button-gradient hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500">
                        Gerar Descrição ✨
                    </button>
                </div>
                <button type="submit" id="submit-btn" class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white button-gradient focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">Adicionar Produto</button>
            </form>
            
            <!-- Botões de exportação e importação -->
            <div class="flex flex-col sm:flex-row space-y-2 sm:space-y-0 sm:space-x-4 mt-4">
                <button id="save-txt-btn" class="flex-1 flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
                    Salvar como TXT
                </button>
                <button id="save-csv-btn" class="flex-1 flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
                    Salvar como CSV
                </button>
                <label for="import-csv-file" class="flex-1 flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-purple-600 hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-500 cursor-pointer">
                    Importar CSV
                </label>
                <input type="file" id="import-csv-file" accept=".csv" class="hidden">
            </div>
            <div id="status-message" class="message-box"></div>
        </div>

        <!-- Tabela de produtos -->
        <div class="pt-4">
            <h2 class="text-2xl font-semibold text-gray-700">Lista de Produtos</h2>
            <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4 mb-4">
                <div class="flex-1">
                    <label for="search-input" class="sr-only">Pesquisar</label>
                    <input type="text" id="search-input" placeholder="Pesquisar por nome..." class="w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                </div>
                <div class="flex-1">
                    <label for="category-filter" class="sr-only">Filtrar por Categoria</label>
                    <select id="category-filter" class="w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm p-2 border bg-white dark:bg-gray-700 dark:border-gray-600 dark:text-white">
                        <option value="">Filtrar por Categoria</option>
                    </select>
                </div>
            </div>
            <div id="product-table-container" class="overflow-x-auto rounded-lg shadow-md">
                <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-100 dark:bg-gray-700">
                        <tr>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider rounded-tl-lg">Imagem</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Nome</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Categoria</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Descrição</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Preço de Compra</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Margem de Lucro</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Preço de Venda</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider rounded-tr-lg">Ações</th>
                        </tr>
                    </thead>
                    <tbody id="product-table-body" class="bg-white dark:bg-gray-800 divide-y divide-gray-200 dark:divide-gray-700">
                        <!-- Produtos serão inseridos aqui pelo JavaScript -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <script>
        // Lógica para o banco de dados IndexedDB
        const DB_NAME = 'ProductManagerDB';
        const DB_VERSION = 1;
        const STORE_NAME = 'products';
        let db;

        // Função para abrir o banco de dados
        function openDatabase() {
            return new Promise((resolve, reject) => {
                if (db) {
                    resolve(db);
                    return;
                }

                const request = indexedDB.open(DB_NAME, DB_VERSION);

                request.onupgradeneeded = (event) => {
                    const db = event.target.result;
                    if (!db.objectStoreNames.contains(STORE_NAME)) {
                        db.createObjectStore(STORE_NAME, { keyPath: 'id', autoIncrement: true });
                    }
                };

                request.onsuccess = (event) => {
                    db = event.target.result;
                    resolve(db);
                };

                request.onerror = (event) => {
                    console.error("Erro ao abrir o banco de dados IndexedDB:", event.target.error);
                    reject(event.target.error);
                };
            });
        }

        // Adiciona um novo produto
        async function addProduct(product) {
            const database = await openDatabase();
            return new Promise((resolve, reject) => {
                const transaction = database.transaction(STORE_NAME, 'readwrite');
                const store = transaction.objectStore(STORE_NAME);
                const request = store.add(product);
                request.onsuccess = () => resolve(request.result);
                request.onerror = (event) => reject(event.target.error);
            });
        }

        // Obtém todos os produtos
        async function getProducts() {
            const database = await openDatabase();
            return new Promise((resolve, reject) => {
                const transaction = database.transaction(STORE_NAME, 'readonly');
                const store = transaction.objectStore(STORE_NAME);
                const request = store.getAll();
                request.onsuccess = () => resolve(request.result);
                request.onerror = (event) => reject(event.target.error);
            });
        }

        // Atualiza um produto existente
        async function updateProduct(id, updatedProduct) {
            const database = await openDatabase();
            return new Promise((resolve, reject) => {
                const transaction = database.transaction(STORE_NAME, 'readwrite');
                const store = transaction.objectStore(STORE_NAME);
                const request = store.put({ ...updatedProduct, id: Number(id) });
                request.onsuccess = () => resolve(request.result);
                request.onerror = (event) => reject(event.target.error);
            });
        }

        // Exclui um produto
        async function deleteProduct(id) {
            const database = await openDatabase();
            return new Promise((resolve, reject) => {
                const transaction = database.transaction(STORE_NAME, 'readwrite');
                const store = transaction.objectStore(STORE_NAME);
                const request = store.delete(Number(id));
                request.onsuccess = () => resolve();
                request.onerror = (event) => reject(event.target.error);
            });
        }

        // DOM elements and variables
        const productForm = document.getElementById('product-form');
        const productTableBody = document.getElementById('product-table-body');
        const generateDescriptionBtn = document.getElementById('generate-description-btn');
        const saveTxtBtn = document.getElementById('save-txt-btn');
        const saveCsvBtn = document.getElementById('save-csv-btn');
        const importCsvFile = document.getElementById('import-csv-file');
        const productNameInput = document.getElementById('product-name');
        const productCategoryInput = document.getElementById('product-category');
        const productDescriptionInput = document.getElementById('product-description');
        const purchasePriceInput = document.getElementById('purchase-price');
        const profitPercentageInput = document.getElementById('profit-percentage');
        const productImageInput = document.getElementById('product-image');
        const submitBtn = document.getElementById('submit-btn');
        const statusMessage = document.getElementById('status-message');
        const productIdInput = document.getElementById('product-id');
        const searchInput = document.getElementById('search-input');
        const categoryFilter = document.getElementById('category-filter');
        const themeToggleBtn = document.getElementById('theme-toggle');
        const sunIcon = document.getElementById('sun-icon');
        const moonIcon = document.getElementById('moon-icon');
        const offlineMessage = document.getElementById('offline-message');

        let allProducts = [];

        // UI functions
        function showMessage(message, type) {
            statusMessage.textContent = message;
            statusMessage.className = `message-box ${type}`;
            statusMessage.style.display = 'block';
            setTimeout(() => {
                statusMessage.style.display = 'none';
            }, 5000);
        }

        function renderTable(productsToDisplay) {
            productTableBody.innerHTML = '';
            if (productsToDisplay.length === 0) {
                const row = document.createElement('tr');
                row.innerHTML = `<td colspan="8" class="px-6 py-4 text-center text-gray-500 dark:text-gray-400">Nenhum produto encontrado.</td>`;
                productTableBody.appendChild(row);
                return;
            }
            productsToDisplay.forEach(product => {
                const row = document.createElement('tr');
                row.classList.add('hover:bg-gray-50', 'dark:hover:bg-gray-700');
                const sellingPrice = product.purchasePrice * (1 + (product.profitPercentage / 100));

                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap">
                        <img src="${product.imageUrl || 'https://placehold.co/48x48/e2e8f0/64748b?text=Sem+Foto'}" alt="Imagem do Produto" class="h-12 w-12 rounded-full object-cover">
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm font-medium text-gray-900 dark:text-white">${product.name}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm text-gray-500 dark:text-gray-400">${product.category}</div>
                    </td>
                    <td class="px-6 py-4">
                        <div class="text-sm text-gray-500 dark:text-gray-400 max-w-xs overflow-hidden text-ellipsis">${product.description}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm text-gray-500 dark:text-gray-400">R$ ${product.purchasePrice.toFixed(2)}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm text-gray-500 dark:text-gray-400">${product.profitPercentage.toFixed(2)}%</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <div class="text-sm font-bold text-gray-900 dark:text-white">R$ ${sellingPrice.toFixed(2)}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                        <button data-id="${product.id}" class="edit-btn text-indigo-600 dark:text-indigo-400 hover:text-indigo-900 mr-2">Editar</button>
                        <button data-id="${product.id}" class="delete-btn text-red-600 dark:text-red-400 hover:text-red-900">Excluir</button>
                    </td>
                `;
                productTableBody.appendChild(row);
            });
        }

        async function refreshProductList() {
            try {
                allProducts = await getProducts();
                updateTable(allProducts);
                renderCategories(allProducts);
            } catch (error) {
                console.error("Erro ao carregar produtos:", error);
                showMessage("Erro ao carregar os produtos.", 'error');
            }
        }
        
        // Função para atualizar a tabela com base na pesquisa e no filtro
        function updateTable(products) {
            const searchTerm = searchInput.value.toLowerCase();
            const selectedCategory = categoryFilter.value;

            let filteredProducts = products;

            if (selectedCategory) {
                filteredProducts = filteredProducts.filter(p => p.category === selectedCategory);
            }

            if (searchTerm) {
                filteredProducts = filteredProducts.filter(p => p.name.toLowerCase().includes(searchTerm));
            }

            renderTable(filteredProducts);
        }

        // Função para preencher o filtro de categorias
        function renderCategories(products) {
            const categories = [...new Set(products.map(p => p.category))];
            const currentFilter = categoryFilter.value;
            categoryFilter.innerHTML = '<option value="">Filtrar por Categoria</option>';
            categories.forEach(category => {
                const option = document.createElement('option');
                option.value = category;
                option.textContent = category;
                if (category === currentFilter) {
                    option.selected = true;
                }
                categoryFilter.appendChild(option);
            });
        }

        // Function to call the Gemini API
        async function generateDescription(productName, productCategory) {
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

            const systemPrompt = `Você é um especialista em marketing. Crie uma descrição de produto concisa e envolvente.`;
            const userQuery = `Gere uma descrição de produto para um item chamado "${productName}" da categoria "${productCategory}". A descrição deve ser adequada para uma pequena empresa de e-commerce.`;

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                const text = result?.candidates?.[0]?.content?.parts?.[0]?.text;
                if (text) {
                    return text;
                }
            } catch (error) {
                console.error("Erro ao gerar descrição:", error);
                return "Não foi possível gerar a descrição no momento.";
            }
        }
        
        // Function to save data to a file
        function saveFile(filename, data, mimeType) {
            const blob = new Blob([data], { type: mimeType });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = filename;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
        }

        // Function to save as TXT
        async function saveToTxt() {
            const productsToSave = await getProducts();
            const txtHeader = "Lista de Produtos\n\n";
            const txtContent = productsToSave.map(p => 
                `Nome: ${p.name}\n` +
                `Categoria: ${p.category}\n` +
                `Descrição: ${p.description}\n` +
                `Preço de Compra: R$ ${p.purchasePrice.toFixed(2)}\n` +
                `Margem de Lucro: ${p.profitPercentage.toFixed(2)}%\n` +
                `Preço de Venda: R$ ${(p.purchasePrice * (1 + (p.profitPercentage / 100))).toFixed(2)}\n` +
                `URL da Imagem: ${p.imageUrl || 'N/A'}\n`
            ).join('\n----------------------------------------\n');

            const fullTxt = txtHeader + txtContent;
            saveFile("produtos.txt", fullTxt, "text/plain");
        }

        // Function to save as CSV
        async function saveToCsv() {
            const productsToSave = await getProducts();
            const csvHeader = "Nome,Categoria,Descrição,Preço de Compra,Margem de Lucro,Preço de Venda,URL da Imagem\n";
            const csvRows = productsToSave.map(p => {
                const sellingPrice = (p.purchasePrice * (1 + (p.profitPercentage / 100))).toFixed(2);
                const sanitizedDescription = `"${p.description.replace(/"/g, '""')}"`;
                return `${p.name},${p.category},${sanitizedDescription},${p.purchasePrice.toFixed(2)},${p.profitPercentage.toFixed(2)},${sellingPrice},${p.imageUrl || ''}`;
            });
            const fullCsv = csvHeader + csvRows.join('\n');
            saveFile("produtos.csv", fullCsv, "text/csv");
        }
        
        // Function to import data from a CSV file
        function importFromCsv(file) {
            const reader = new FileReader();
            reader.onload = async (e) => {
                const text = e.target.result;
                const lines = text.split('\n').filter(line => line.trim() !== '');
                const importedProducts = [];

                if (lines.length > 0) {
                    lines.shift();
                }

                lines.forEach(line => {
                    const matches = line.match(/(?:[^,"]+|"[^"]*")+/g);
                    if (!matches || matches.length < 6) {
                        return;
                    }
                    
                    const name = matches[0].trim();
                    const category = matches[1].trim();
                    const description = matches[2].replace(/"/g, '').trim();
                    const purchasePrice = parseFloat(matches[3].trim());
                    const profitPercentage = parseFloat(matches[4].trim());
                    const imageUrl = matches[6] ? matches[6].trim() : '';

                    if (name && category && !isNaN(purchasePrice) && !isNaN(profitPercentage)) {
                        importedProducts.push({
                            name: name,
                            category: category,
                            purchasePrice: purchasePrice,
                            profitPercentage: profitPercentage,
                            imageUrl: imageUrl,
                            description: description
                        });
                    }
                });

                for (const product of importedProducts) {
                    await addProduct(product);
                }
                showMessage(`Foram importados ${importedProducts.length} produtos do arquivo CSV.`, 'success');
                refreshProductList();
            };
            reader.onerror = () => {
                showMessage("Erro ao ler o arquivo CSV. Por favor, tente novamente.", 'error');
            };
            reader.readAsText(file);
        }

        // Theme functions
        function setIcon(theme) {
            if (theme === 'dark') {
                sunIcon.classList.add('hidden');
                moonIcon.classList.remove('hidden');
            } else {
                sunIcon.classList.remove('hidden');
                moonIcon.classList.add('hidden');
            }
        }

        function toggleTheme() {
            const root = document.documentElement;
            if (root.classList.contains('dark')) {
                root.classList.remove('dark');
                localStorage.setItem('theme', 'light');
                setIcon('light');
            } else {
                root.classList.add('dark');
                localStorage.setItem('theme', 'dark');
                setIcon('dark');
            }
        }

        function loadTheme() {
            const savedTheme = localStorage.getItem('theme');
            const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
            const root = document.documentElement;

            if (savedTheme === 'dark' || (savedTheme === null && prefersDark)) {
                root.classList.add('dark');
                setIcon('dark');
            } else {
                root.classList.remove('dark');
                setIcon('light');
            }
        }

        // Online/Offline status
        function updateOnlineStatus() {
            if (navigator.onLine) {
                offlineMessage.style.display = 'none';
                showMessage("Conectado. As alterações serão salvas.", 'success');
            } else {
                offlineMessage.style.display = 'block';
                showMessage("Você está offline. As alterações serão salvas localmente.", 'info');
            }
        }

        // Event listeners
        document.addEventListener('DOMContentLoaded', () => {
            loadTheme();
            refreshProductList();
            updateOnlineStatus();

            window.addEventListener('online', updateOnlineStatus);
            window.addEventListener('offline', updateOnlineStatus);

            themeToggleBtn.addEventListener('click', toggleTheme);

            generateDescriptionBtn.addEventListener('click', async () => {
                const productName = productNameInput.value;
                const productCategory = productCategoryInput.value;
                if (!productName || !productCategory) {
                    showMessage("Por favor, preencha o Nome e a Categoria do Produto para gerar a descrição.", 'error');
                    return;
                }
                
                generateDescriptionBtn.textContent = 'Gerando...';
                generateDescriptionBtn.disabled = true;

                const description = await generateDescription(productName, productCategory);
                productDescriptionInput.value = description;

                generateDescriptionBtn.textContent = 'Gerar Descrição ✨';
                generateDescriptionBtn.disabled = false;
            });

            productForm.addEventListener('submit', async (e) => {
                e.preventDefault();

                const productName = productNameInput.value;
                const productCategory = productCategoryInput.value;
                const purchasePrice = parseFloat(purchasePriceInput.value);
                const profitPercentage = parseFloat(profitPercentageInput.value);
                const productImage = productImageInput.value;
                const productDescription = productDescriptionInput.value;
                const productId = productIdInput.value;

                if (!productName || !productCategory || isNaN(purchasePrice) || isNaN(profitPercentage)) {
                    showMessage("Por favor, preencha todos os campos obrigatórios com valores válidos.", 'error');
                    return;
                }

                const productData = {
                    name: productName,
                    category: productCategory,
                    purchasePrice: purchasePrice,
                    profitPercentage: profitPercentage,
                    imageUrl: productImage,
                    description: productDescription
                };

                if (productId) {
                    await updateProduct(productId, productData);
                    showMessage("Produto atualizado com sucesso!", 'success');
                } else {
                    await addProduct(productData);
                    showMessage("Produto adicionado com sucesso!", 'success');
                }
                
                productIdInput.value = '';
                submitBtn.textContent = 'Adicionar Produto';
                submitBtn.classList.remove('bg-green-600', 'hover:bg-green-700', 'focus:ring-green-500');
                submitBtn.classList.add('bg-indigo-600', 'hover:bg-indigo-700', 'focus:ring-indigo-500');
                productForm.reset();
                refreshProductList();
            });

            saveTxtBtn.addEventListener('click', saveToTxt);
            saveCsvBtn.addEventListener('click', saveToCsv);
            importCsvFile.addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    importFromCsv(file);
                }
            });

            productTableBody.addEventListener('click', async (e) => {
                const target = e.target;
                if (target.classList.contains('delete-btn')) {
                    const productId = target.dataset.id;
                    await deleteProduct(productId);
                    showMessage("Produto excluído com sucesso!", 'success');
                    refreshProductList();
                } else if (target.classList.contains('edit-btn')) {
                    // Correção: Converte o ID para número antes de procurar o produto
                    const productId = Number(target.dataset.id);
                    const productToEdit = allProducts.find(p => p.id === productId);
                    
                    if (productToEdit) {
                        productNameInput.value = productToEdit.name;
                        productCategoryInput.value = productToEdit.category;
                        purchasePriceInput.value = productToEdit.purchasePrice;
                        profitPercentageInput.value = productToEdit.profitPercentage;
                        productImageInput.value = productToEdit.imageUrl;
                        productDescriptionInput.value = productToEdit.description;
                        productIdInput.value = productToEdit.id;

                        submitBtn.textContent = 'Atualizar Produto';
                        submitBtn.classList.remove('bg-indigo-600', 'hover:bg-indigo-700', 'focus:ring-indigo-500');
                        submitBtn.classList.add('bg-green-600', 'hover:bg-green-700', 'focus:ring-green-500');

                        showMessage("Você está no modo de edição. Faça suas alterações e clique em 'Atualizar'.", 'info');
                    }
                }
            });

            searchInput.addEventListener('input', () => updateTable(allProducts));
            categoryFilter.addEventListener('change', () => updateTable(allProducts));
        });
    </script>
</body>
</html>


