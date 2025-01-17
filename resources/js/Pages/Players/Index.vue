<template>
    <div>
        <div class="prose mb-12">
            <h1>
                Players
            </h1>
            <p>
                Search players by their identifier or name.
            </p>
        </div>

        <!-- Search -->
        <form class="w-full bg-gray-100 p-8 mb-6" @submit.prevent>
            <label class="block font-semibold mb-4" for="name">
                Search by name
            </label>
            <input class="w-full bg-gray-200 px-4 py-2" id="name" name="name" placeholder="Marius Truckster" v-model="filters.query">
        </form>

        <div class="bg-gray-100 rounded shadow overflow-x-auto">
            <table class="w-full whitespace-no-wrap">
                <tr class="text-left font-semibold">
                    <th class="px-6 py-4">Identifier</th>
                    <th class="px-6 py-4">Name</th>
                    <th class="px-6 py-4">Playtime</th>
                    <th class="px-6 py-4">Warnings</th>
                    <th class="px-6 py-4 w-64">Banned?</th>
                    <th class="px-6 py-4 w-24"></th>
                </tr>
                <tr class="hover:bg-gray-100" v-for="player in players.data" v-bind:key="player.id">
                    <td class="px-6 py-3 border-t">{{ player.steamIdentifier }}</td>
                    <td class="px-6 py-3 border-t">{{ player.playerName }}</td>
                    <td class="px-6 py-3 border-t">{{ player.playTime }} minutes</td>
                    <td class="px-6 py-3 border-t">{{ player.warnings }}</td>
                    <td class="px-6 py-3 border-t text-center">
                        <span class="block bg-red-500 text-white rounded px-4 py-2" v-if="player.isBanned">
                            Banned
                        </span>
                        <span class="block bg-green-500 text-white rounded px-4 py-2" v-else>
                            Not banned
                        </span>
                    </td>
                    <td class="px-6 py-3 border-t">
                        <inertia-link class="block bg-indigo-600 font-semibold text-white text-center rounded px-4 py-2" v-bind:href="'/players/' + player.steamIdentifier">
                            <i class="fas fa-chevron-right"></i>
                        </inertia-link>
                    </td>
                </tr>
                <tr v-if="players.data.length === 0">
                    <td class="px-6 py-6 border-t text-center" colspan="100%">
                        No players was found.
                    </td>
                </tr>
            </table>
        </div>
        <pagination v-bind:links="players.links" v-bind:meta="players.meta" />
    </div>
</template>

<script>
import Layout from './../../Layouts/App';
import Pagination from './../../Components/Pagination';
import throttle from 'lodash/throttle';

export default {
    layout: Layout,
    components: {
        Pagination,
    },
    props: {
        players: {
            type: Object,
            required: true,
        },
        filters: {
            query: String,
        },
    },
    watch: {
        filters: {
            handler: throttle(function () {
                this.$inertia.replace('/players', {
                    data: this.filters,
                    preserveState: true,
                    preserveScroll: true,
                    only: [ 'players' ],
                });
            }, 150),
            deep: true,
        },
    },
};
</script>
