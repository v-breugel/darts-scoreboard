<script setup lang="ts">
import { ref, reactive } from 'vue';
import Player from './Player.vue';

interface PlayerType {
    name: string;
    startingScore: number;
    active: boolean;
}

const dialogIsOpen = ref(false);
const newPlayerName = ref('');
const startingScore = 301;
const players: PlayerType[] = reactive([]);

const addPlayer = () => {
    players.push({
        name: newPlayerName.value,
        startingScore,
        active: players.length === 0 ? true : false
    });

    newPlayerName.value = '';
    dialogIsOpen.value = false;
};

const updateActivePlayer = () => {
    if (players.length === 1) {
        return false;
    }

    const activePlayerIndex = players.findIndex((player) => player.active === true);

    if (activePlayerIndex === players.length - 1) {
        players[0].active = true;
    } else {
        players[activePlayerIndex + 1].active = true;
    }

    players[activePlayerIndex].active = false;
};

const openAddPlayerDialog = () => {
    dialogIsOpen.value = true;
};
</script>

<template>
    <q-page padding>
        <div class="row">
            <Player
                v-for="player in players"
                :key="player.name"
                v-bind="player"
                @score-submitted="updateActivePlayer"
            />
            <q-card>
                <q-card-section>
                    <q-btn @click="openAddPlayerDialog">Add player</q-btn></q-card-section
                >
            </q-card>
        </div>
    </q-page>

    <q-dialog v-model="dialogIsOpen">
        <q-card>
            <q-card-section>
                <div class="text-h6">Add new player</div>
            </q-card-section>
            <q-card-section class="q-pt-none">
                <q-form @submit.prevent="addPlayer">
                    <q-input type="text" v-model="newPlayerName" />
                    <q-btn type="submit" :disabled="!newPlayerName">Add Player</q-btn>
                </q-form>
            </q-card-section>
        </q-card>
    </q-dialog>
</template>

<style lang="scss" scoped></style>
