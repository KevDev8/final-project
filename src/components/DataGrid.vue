<template>
    <div>
        <div class="d-flex justify-content-between align-items-center mb-3"> <!-- Basic alignment, centering and spacing -->
            <h3>Fetched Books</h3>
            <div>
                <button class="btn btn-sm btn-outline-primary me-2" @click="fetchData">Refresh</button> <!-- Calls fetchData again when the Refresh button is clicked -->
                <button class="btn btn-sm btn-outline-secondary" @click="logout">Logout</button> <!-- Calls logout when the Logout button is clicked to reload the home page -->
            </div>
        </div>

        <div v-if="loading" class="text-center py-5">Loading...</div> <!-- Shows up while the data is being fetched -->
        <div v-if="fetchError" class="alert alert-danger">{{ fetchError }}</div> <!-- Warns if there was any error when fetching the data -->

        <div v-if="items.length" class="data-grid"> <!-- Checks if there is any data to display -->
            <div class="grid-container">
                <DataCard v-for="item in items" :key="item.id" :item="item"/> <!-- Loads one DataCard component for each item fetched iterating through the list of them -->
            </div>
        </div>
        <div v-else-if="!loading" class="alert alert-info">No data to display.</div> <!-- Warns if the API doesn't return any data -->
    </div>
</template>

<script>
import DataCard from './DataCard.vue'

export default {
    name: 'DataGrid',
    props: { password: { type: String, default: 'cardinals' } }, // Inherits the password passed from App.vue and defaults to cardinals if it fails to receive it
    components: { DataCard },
    data() {
        return {
            loading: false,
            fetchError: null,
            items: [],
        }
    },
    mounted() { this.fetchData() }, // Waits for the data grid to load before fetching the data
    methods: {
        logout() {
            window.location.reload() // Reload the page to show login again
        },
        async fetchData() {
            this.loading = true
            this.fetchError = null
            this.items = []

            try {
                const res = await fetch("https://cis255.vercel.app/api", {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({ password: this.password })
                }) // Basic authenticated request to fetch the data from the API

                if (!res.ok) throw new Error(`HTTP ${res.status}`) // Throws an error if the data couldn't be fetched, showing the HTTP error code being received

                let data = await res.json()
                this.items = Array.isArray(data.books) ? data.books : []

            }
            catch (err) { this.fetchError = 'Failed to fetch data: ' + err.message } // Catches the thrown http error and escalates it to the responsible alert
            finally { this.loading = false } // Sets loading to false, hiding the "Loading..." text after the fetching was completed (successfully or not)
        }
    }
}
</script>

<style>
.grid-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /** Three equal sized columns per row */
    gap: 1rem; /** Adds some spacing */
}
</style>
