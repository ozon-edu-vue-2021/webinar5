<template>
    <div id="app">
        <h1>Vue-компоненты. Практика</h1>

        <div class="components-wrapper">
            <div>
                <h2>Input: {{ text }}</h2>
                <base-input
                    v-model="text"
                    placeholder="Введите значение"
                    autofocus
                />
            </div>

            <div>
                <h2>NumberInput: {{ numtext }}</h2>
                <number-input
                    v-model="numtext"
                    placeholder="Введите число"
                    :max="501"
                />
            </div>

            <div>
                <h2>MultiSelect</h2>
                <multi-select
                    :items="items"
                    :selected="selectedItems"
                    prop-name="name"
                    prop-value="id"
                    @selected="updateSelectedItems"
                />

                <div class="component-item">
                    <chip
                        v-for="(item, i) in selectedItems"
                        :key="`${item.id}-${i}`"
                        class="component-chip-item"
                        :item="item"
                        prop-name="name"
                        prop-value="id"
                        @remove="removeFromSelected"
                    />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import BaseInput from './components/Input/BaseInput.vue';
import NumberInput from './components/Input/NumberInput.vue';
import MultiSelect from './components/MultiSelect/MultiSelect.vue';
import Chip from './components/Chip/Chip.vue';
import items from './utils/data';

export default {
    name: 'App',
    components: {
        BaseInput,
        NumberInput,
        MultiSelect,
        Chip
    },
    data() {
        return {
            text: '',
            numtext: '',
            items,
            selectedItems: [items[0], items[3]]
        }
    },
    methods: {
        addToSelected(item) {
            this.selectedItems.push(item);
        },
        removeFromSelected(item) {
            this.selectedItems = this.selectedItems.filter(selected => selected.id !== item.id);
        },
        updateSelectedItems(item) {
            const foundIndex = this.selectedItems.findIndex(selected => selected.id === item.id);
            foundIndex > -1
                ? this.removeFromSelected(item)
                : this.addToSelected(item);
        }
    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>

<style scoped>
.components-wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 60px;
    margin: 0 auto;
    width: 1000px;
}

.component-item {
    display: flex;
    margin-top: 30px;
}

.component-chip-item:not(:last-child) {
    margin-right: 8px;
}
</style>
