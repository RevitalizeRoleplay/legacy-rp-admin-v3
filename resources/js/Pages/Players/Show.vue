<template>
    <div>
        <div class="flex flex-grow items-start justify-between mb-12">
            <div class="prose max-w-full">
                <div class="flex items-start space-x-10">
                    <h1>
                        {{ player.playerName }}
                    </h1>
                    <div class="flex items-center space-x-5">
                        <div class="px-5 py-1 rounded bg-red-100 border-2 border-red-200" v-if="player.isBanned">
                            <span class="font-semibold">Banned</span>
                        </div>
                        <div class="px-5 py-1 rounded bg-green-100 border-2 border-green-200" v-if="player.isStaff">
                            <span class="font-semibold">Staff Member</span>
                        </div>
                        <div class="px-5 py-1 rounded bg-gray-100 border-2 border-gray-200">
                            <span class="font-semibold">{{ player.playTime }}</span> minutes played
                        </div>
                    </div>
                </div>
                <p>
                    Viewing player profile.
                </p>
            </div>
            <div>
                <!-- Unbanning -->
                <inertia-link class="rounded bg-green-500 hover:bg-green-600 font-semibold text-white py-2 px-5" method="DELETE" v-bind:href="'/players/' + player.steamIdentifier + '/bans/' + player.ban.id" v-if="player.isBanned">
                    <i class="fas fa-lock-open mr-1"></i>
                    Unban
                </inertia-link>
                <!-- Banning -->
                <button class="rounded bg-red-500 hover:bg-red-600 font-semibold text-white py-2 px-5" @click="isBanning = true" v-else>
                    <i class="fas fa-gavel mr-1"></i>
                    Issue ban
                </button>
            </div>
        </div>

        <!-- Ban -->
        <div>
            <!-- Viewing -->
            <div class="rounded bg-red-500 text-white p-4 mb-10" v-if="player.isBanned">
                <div class="flex items-center justify-between mb-2">
                    <h2 class="text-lg font-semibold">
                        Banned by <span class="italic">{{ player.ban.issuer }}</span> <span class="italic" v-if="player.ban.expire">until {{ $moment(player.ban.expireAt).format('lll') }}</span>
                    </h2>
                    <div class="font-semibold">
                        {{ $moment(player.ban.timestamp).format('lll') }}
                    </div>
                </div>
                <p class="text-gray-100">
                    {{ player.ban.reason }}
                </p>
            </div>
            <!-- Issuing -->
            <div class="rounded bg-gray-100 p-8 mb-10" v-if="isBanning">
                <div class="space-y-5 mb-8">
                    <h2 class="text-2xl font-semibold">
                        Issuing ban
                    </h2>
                    <p class="text-gray-900">
                        You are now issuing a ban for this player. Make sure you are <span class="font-semibold">well within reason</span> to do this. It's never a bad idea to double check with an additional staff member!
                    </p>
                </div>
                <form class="space-y-6" @submit.prevent="submitBan">
                    <!-- Deciding if ban is temporary -->
                    <div class="flex items-center space-x-3">
                        <input class="block shadow rounded bg-gray-200 p-3" type="checkbox" id="tempban" name="tempban" v-model="isTempBanning">
                        <label class="font-semibold italic" for="tempban">
                            This is a temporary ban
                        </label>
                    </div>

                    <!-- Expiration -->
                    <div v-if="isTempBanning">
                        <label class="font-semibold italic">
                            Expiration
                        </label>
                        <div class="flex items-center">
                            <input class="block shadow rounded bg-gray-200 p-3" type="date" id="expireDate" name="expireDate" step="any" :min="$moment().format('YYYY-MM-DD')" v-model="form.ban.expireDate" required>
                            <input class="block shadow rounded bg-gray-200 p-3" type="time" id="expireTime" name="expireTime" step="any" :min="$moment().format('HH:mm')" v-model="form.ban.expireTime" required>
                        </div>
                    </div>

                    <!-- Reason -->
                    <div>
                        <label class="font-semibold italic" for="reason">
                            Reason
                        </label>
                        <textarea class="block w-full shadow rounded bg-gray-200 p-5" id="reason" name="reason" rows="5" :placeholder="player.playerName + ' did a big oopsie.'" v-model="form.ban.reason" required></textarea>
                    </div>

                    <!-- Buttons -->
                    <div class="flex items-center space-x-3">
                        <button class="rounded bg-red-500 hover:bg-red-600 font-semibold text-white py-2 px-5" type="submit">
                            <i class="fas fa-gavel mr-1"></i>
                            Ban player
                        </button>
                        <button class="rounded hover:bg-gray-200 py-2 px-5" type="button" @click="creatingBan = false">
                            Cancel
                        </button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Useful links -->
        <div class="rounded bg-gray-100 p-8 mb-8">
            <div class="flex flex-wrap items-center text-center">
                <inertia-link class="flex-1 m-2 bg-indigo-600 font-semibold text-white rounded block p-5" v-bind:href="'/logs?identifier=' + player.steamIdentifier">
                    <i class="fas fa-toilet-paper mr-1"></i>
                    Check player's logs
                </inertia-link>
                <a class="flex-1 m-2 bg-gray-800 font-semibold text-white rounded block p-5" target="_blank" v-bind:href="player.steamProfileUrl">
                    <i class="fab fa-steam mr-1"></i>
                    Open Steam profile
                </a>
            </div>
        </div>

        <!-- Characters -->
        <div class="rounded bg-gray-100 p-8 mb-8">
            <h2 class="text-2xl font-semibold mx-3 mb-3">
                Characters
            </h2>
            <div class="grid grid-cols-1 lg:grid-cols-2 xl:grid-cols-3">
                <div class="flex flex-col bg-gray-200 shadow rounded p-8 m-3" v-for="character in characters" v-bind:key="character.id">
                    <div class="flex-grow mb-10">
                        <div class="text-center border-b-2 border-gray-400 mb-8 pb-8">
                            <h1 class="text-xl font-semibold mb-2">
                                {{ character.name }} (#{{ character.id }})
                            </h1>
                            <h3 class="text-indigo-500">
                                <span class="font-semibold">Date of birth:</span> {{ $moment(character.dateOfBirth).format('l') }}
                            </h3>
                        </div>
                        <p class="text-gray-800">
                            {{ character.backstory }}
                        </p>
                    </div>
                    <inertia-link class="bg-indigo-600 hover:bg-orange-500 text-white text-center rounded block px-4 py-3" v-bind:href="'/players/' + player.steamIdentifier + '/characters/' + character.id + '/edit'">
                        View
                    </inertia-link>
                </div>
            </div>
            <p class="px-4 py-6" v-if="characters.length === 0">
                This player has not created any characters yet.
            </p>
        </div>

        <!-- Warnings -->
        <div class="rounded bg-gray-100 p-8">
            <h2 class="text-2xl font-semibold mb-5">
                Warnings ({{ player.warnings }})
            </h2>
            <div class="mb-8">
                <div class="flex-grow bg-gray-200 rounded p-5 mb-5" v-for="warning in warnings" v-bind:key="warning.id">
                    <div class="flex items-center justify-between border-b-2 border-gray-400 mb-5 pb-5">
                        <h1 class="text-lg font-semibold">
                            {{ warning.issuer.playerName }}
                        </h1>
                        <div class="flex items-center">
                            <span>{{ $moment(warning.createdAt).format('lll') }}</span>
                            <inertia-link class="bg-red-500 hover:bg-red-600 font-semibold text-white text-sm rounded py-1 px-4 ml-4" method="DELETE" v-bind:href="'/players/' + warning.player.steamIdentifier + '/warnings/' + warning.id">
                                <i class="fas fa-trash mr-1"></i>
                                Remove
                            </inertia-link>
                        </div>
                    </div>
                    <p class="text-gray-800">
                        {{ warning.message }}
                    </p>
                </div>
                <p v-if="warnings.length === 0">
                    This player has not received any warnings.
                </p>
            </div>
            <form @submit.prevent="submitWarning">
                <label for="message"></label>
                <textarea class="w-full shadow rounded bg-gray-200 p-5 mb-5" id="message" name="message" rows="5" v-bind:placeholder="player.playerName + ' did an oopsie.'" v-model="form.warning.message" required></textarea>

                <button class="rounded bg-indigo-600 font-semibold text-white py-2 px-5" type="submit">
                    <i class="fas fa-exclamation mr-1"></i>
                    Warn player
                </button>
            </form>
        </div>
    </div>
</template>

<script>
import Layout from './../../Layouts/App';

export default {
    layout: Layout,
    props: {
        player: {
            type: Object,
            required: true,
        },
        characters: {
            type: Array,
            required: true,
        },
        warnings: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            isBanning: false,
            isTempBanning: false,
            form: {
                ban: {
                    reason: null,
                    expire: null,
                    expireDate: null,
                    expireTime: null,
                },
                warning: {
                    message: null,
                },
            },
        };
    },
    methods: {
        async submitBan () {
            // Default expiration.
            let expire = null;

            // Calculate expire relative to now in seconds if temp ban.
            if (this.isTempBanning) {
                const nowUnix    = this.$moment().unix();
                const expireUnix = this.$moment(this.form.ban.expireDate + ' ' + this.form.ban.expireTime).unix();
                expire           = expireUnix - nowUnix;
            }

            // Send request.
            await this.$inertia.post('/players/' + this.player.steamIdentifier + '/bans', { ...this.form.ban, expire });

            // Reset.
            this.isBanning = false;
            this.form.ban.reason = null;
            this.form.ban.expire = null;
            this.form.ban.expireDate = null;
            this.form.ban.expireTime = null;
        },
        async submitWarning () {
            // Send request.
            await this.$inertia.post('/players/' + this.player.steamIdentifier + '/warnings', this.form.warning);

            // Reset.
            this.form.warning.message = null;
        },
    },
};
</script>
