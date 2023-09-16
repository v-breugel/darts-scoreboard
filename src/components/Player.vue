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
    <q-card class="player-card" :class="{ 'is-active': active }" bordered>
        <q-card-section>
            <div>Name: {{ name }}</div>
            <div>Remaining: {{ remaining }}</div>
            <div>Turns: {{ turns }}</div>
            <q-form @submit.prevent="updateScore">
                <q-input
                    type="text"
                    outlined
                    label="Score"
                    placeholder="Enter your score"
                    v-model="score"
                    ref="scoreInputElement"
                />
            </q-form>
        </q-card-section>
        <q-card-actions align="right">
            <q-btn type="submit" @click="updateScore" :disabled="!score || !active" color="primary" label="Submit score"/>
        </q-card-actions>
    </q-card>
</template>

<style lang="scss" scoped>
.player-card {

}
</style>
