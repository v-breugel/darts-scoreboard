<script setup lang="ts">
import { ref, watch } from 'vue';

const props = defineProps({
    name: {
        type: String,
        required: true,
        default: 'Player'
    },
    startingScore: {
        type: Number,
        required: true,
        default: 501
    },
    active: {
        type: Boolean,
        required: true,
        default: false
    }
});

watch(
    () => props.active,
    (newValue, oldValue) => {
        if (oldValue === false && newValue === true) {
            setPlayerActive();
        }
    }
);

const emit = defineEmits(['scoreSubmitted']);
const remaining = ref(props.startingScore);
const score = ref('');
const turns = ref(0);
const scoreInputElement = ref<HTMLInputElement>();

const updateScore = () => {
    if (remaining.value - parseInt(score.value) > 0) {
        remaining.value = remaining.value - parseInt(score.value);
    }

    score.value = '';
    turns.value++;
    emit('scoreSubmitted');
};

const setPlayerActive = () => {
    scoreInputElement.value?.focus();
};
</script>

<template>
    <div class="card" :class="{ 'is-active': active }">
        <div>Name: {{ name }}</div>
        <div>Remaining: {{ remaining }}</div>
        <div>Turns: {{ turns }}</div>
        <form @submit.prevent="updateScore">
            <input
                type="text"
                placeholder="Enter your score"
                v-model="score"
                ref="scoreInputElement"
            />
            <button type="submit" :disabled="!score || !active">Submit score</button>
        </form>
    </div>
</template>

<style lang="scss" scoped>
.card {
    flex: 1;
    max-width: 300px;
    padding: 20px;
    margin: 20px;
    border-radius: 5px;
    border: 3px solid #ccc;

    > * {
        margin-bottom: 10px;
    }

    &.is-active {
        border-color: black;
    }
}
</style>
