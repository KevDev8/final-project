<template>
    <div :class="['data-card card p-3 border-2', statusClass]"> <!-- Basic card styling, with borders and padding, data-card for extra customization later,
                                                                     and another dynamically assigned class depending on publishing status -->
        <div class="card-body p-0"> <!-- Prevent excessive padding in the body -->
            <h4 class="mb-1">{{ item.title }}</h4>
            <p class="mb-1"><strong>ID:</strong> {{ item.id }}</p>
            <p class="mb-1"><strong>Year:</strong> {{ item.year }}</p>
            <p class="mb-0"><strong>Published:</strong> {{ isTrue ? 'Yes' : 'No' }}</p> <!-- Turns the Published into Yes and No from true and false -->
        </div>
  </div>
</template>

<script>
export default {
  name: 'DataCard',
  props: { item: { type: Object, required: true } }, // Requires an item object to use
  computed: { // More efficient than methods for dynamic values
    isTrue() {
        if (typeof this.item.published === 'boolean') { return this.item.published }
        else { return false }
    },
    statusClass() { return this.isTrue ? 'true-card' : 'false-card' } // Status dependent on isTrue, which depends on whether an item (book) is published or not
  }
}
</script>

<style>
.data-card {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
.true-card {
    background: linear-gradient(180deg, #e6ffed, #f7fff6);
    border-color: #20c997;
}
.false-card {
    background: linear-gradient(180deg, #fff6f6, #fff9f9);
    border-color: #dc3545;
}
.card-body p {
    margin: 0.2rem 0;
}
</style>
