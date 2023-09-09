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
    <div class="container">
        <button @click="openAddPlayerDialog">Add player</button>
        <div class="player-list">
            <Player
                v-for="player in players"
                :key="player.name"
                v-bind="player"
                @score-submitted="updateActivePlayer"
            />
        </div>
    </div>

    <dialog :open="dialogIsOpen">
        <p>Add new player</p>
        <form @submit.prevent="addPlayer">
            <input type="text" v-model="newPlayerName" />
            <button type="submit" :disabled="!newPlayerName">Add Player</button>
        </form>
    </dialog>
</template>

<style lang="scss" scoped>
.container {
    padding: 20px;
}

.player-list {
    display: flex;
    flex-wrap: wrap;
}
</style>
