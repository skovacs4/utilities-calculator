<script>
    // No script is needed for this layout component
    // You can customize the layout by adding props or slots
</script>

<div>
    <!-- Header -->
    <header class="bg-gray-800 text-white text-center py-4">
        <!-- <h1 class="text-2xl font-bold">Doneaza</h1> -->
    </header>

    <a href="https://revolut.me/stefantb9v" target="_blank" class="donate">Doneaza</a>

    <!-- Main Content (provided as slot) -->
    <main>
        <slot />
    </main>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white text-center py-4">
        <p>&copy; 2024 Nomadic Digital</p>
    </footer>

    <style>
        /* Global styles or overrides can go here */
        main {
            min-height: calc(100vh - 160px); /* Adjust based on header and footer height */
            padding: 20px;
        }

        .donate {
        position: fixed;
        left: 0;
        top: 50%;
        padding: 10px;
        background-color: black;
        color: white;
        transition: background-color 0.3s, color 0.3s; /* Add transition for smooth hover effect */
    }

    /* Add hover styles */
    .donate:hover {
        background-color: white;
        color: black;
    }
    </style>
</div>
