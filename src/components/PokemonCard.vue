<template>
    <div id="pokemon">
        <div class="card text-bg-dark border-0 my-3">
            <img :src="pokeImagen" class="card-img-top" :class="hidePokemon ? 'blurred' : ''" draggable="false"/>
            <div class="card-body">
                <p class="text-uppercase text-center fw-bold" v-show="!hidePokemon">{{ pokemon.name }}</p>
                <form v-show="hidePokemon">
                    <input type="text" class="form-control" v-model="showPokemon"
                        :class="shakePokemon ? 'shake' : ''" />
                    <div class="d-grid py-2">
                        <button type="submit" class="btn btn-secondary" @click.prevent="enterPokemon">Descubrir</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "PokemonCard",
    props: {
        pokemon: Object,
    },
    data() {
        return {
            infoPokemon: {},
            hidePokemon: true,
            showPokemon: "",
            shakePokemon: false
        };
    },
    computed: {
        pokeImagen() {
            return this.infoPokemon.sprites?.other["official-artwork"].front_default;
        },
    },
    methods: {
        async getInfoPokemon() {
            try {
                let { data } = await axios.get(this.pokemon.url);
                this.infoPokemon = data;
            } catch (error) {
                console.log(error);
            }
        },
        enterPokemon() {
            this.shakePokemon = false
            if (this.showPokemon.toLowerCase() === this.pokemon.name.toLowerCase()) {
                this.hidePokemon = false
                this.$emit("rightAnswer")
            } else {
                this.shakePokemon = true
                this.showPokemon = ""
                setTimeout(() => {
                    this.shakePokemon = false
                }, 1000)
                console.log("incorrecto")

            }
        }
    },
    created() {
        this.getInfoPokemon();
    },
};
</script>

<style scoped>
.blurred {
    filter: blur(10px) grayscale(100%);
}

.shake {
    animation: shake 0.5s;
}

@keyframes shake {
    0% {
        transform: translate(1px, 1px) rotate(0deg);
    }

    10% {
        transform: translate(-1px, -2px) rotate(-1deg);
    }

    20% {
        transform: translate(-3px, 0px) rotate(1deg);
    }

    30% {
        transform: translate(3px, 2px) rotate(0deg);
    }

    40% {
        transform: translate(1px, -1px) rotate(1deg);
    }

    50% {
        transform: translate(-1px, 2px) rotate(-1deg);
    }

    60% {
        transform: translate(-3px, 1px) rotate(0deg);
    }

    70% {
        transform: translate(3px, 1px) rotate(-1deg);
    }

    80% {
        transform: translate(-1px, -1px) rotate(1deg);
    }

    90% {
        transform: translate(1px, 2px) rotate(0deg);
    }

    100% {
        transform: translate(1px, -2px) rotate(-1deg);
    }
}
</style>
