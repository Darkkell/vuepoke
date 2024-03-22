<template>
    <div>
        <label for="base-input"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white sm:max-w-xl md:max-w-6xl w-full mx-auto">
            Busca tu Pokémon por nombre o número
        </label>
        <input
            type="search"
            name="search"
            id="base-input"
            v-model="data"
            @change="searchPokemon"
            placeholder="Pikachu, 25, Mankey"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block  sm:max-w-xl md:max-w-6xl w-full mx-auto p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white  dark:foocus:ring-blue-500 dark:focus:border-blue-500">
    </div>

    <ul class="grid justify-items-center md:max-w-6xl mx-auto">
        <li class="pt-10">
            <p v-if="err" class="block mb-2 text-sm font-medium dark:text-white mx-auto">No existe el Pokémon</p>
            <p class="loader" v-if="!dataLoaded"></p>
            <article class="max-w-[350px]" v-if="dataLoaded">
                <div class="card max-w-[350px] max-h-[516px] fade-enter-active fade-leave-active" :style="typeColor[pokemon?.types?.[0]]?.bgn">
                    <!-- <p>{{ data }}</p> -->
                    <img :src="pokemon.sprite" loading="lazy"> <!-- :alt="pokemon.name" -->
                    <h1 class="name">{{pokemon.name}}</h1>
                    <h2 class="id text-[6rem]">#{{pokemon.id}}</h2>

                    <div class="types">
                        <span :style="typeColor[type].color" v-for="type in pokemon?.types" :key="type.name">
                            <img :src="typeColor[type].icon" alt="type">
                            <p>{{type}}</p>
                        </span>
                    </div>

                    <div class="stats grid grid-cols-3 mx-auto">
                        <span :style="stat.color" v-for="stat in pokemon?.stats" :key="stat.name">
                            <img :src="stat.name" class="svg"/>
                            <p>{{ stat.base_stat }}</p>
                        </span>
                    </div>

                </div>
            </article>
        </li>
    </ul>
</template>

<script>
export default{
    components:{
    },
    data(){
        return{
            dataLoaded: false,
            pokemon: [],
            typeColor: {
                normal: {
                    color: "background:linear-gradient(105deg,#A8A878 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/a/ae/Normal_icon.png/20px-Normal_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#A8A878 36%, #ffffff 36%);"
                },
                fire: {
                    color: "background:linear-gradient(105deg,#F08030 30px,#5A5A5A 31px,#5A5A5A)",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/5/5e/Fire_icon.png/20px-Fire_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#F08030 36%, #ffffff 36%);"
                },
                water: {
                    color: "background:linear-gradient(105deg,#6890F0 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/7/7f/Water_icon.png/20px-Water_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#6890F0 36%, #ffffff 36%);"
                },
                grass: {
                    color: "background:linear-gradient(105deg,#78C850 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/c/cb/Grass_icon.png/20px-Grass_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#78C850 36%, #ffffff 36%);"
                },
                electric: {
                    color: "background:linear-gradient(105deg,#F8D030 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/a/af/Electric_icon.png/20px-Electric_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#F8D030 36%, #ffffff 36%);"
                },
                ice: {
                    color: "background:linear-gradient(105deg,#98D8D8 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/8/83/Ice_icon.png/20px-Ice_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#98D8D8 36%, #ffffff 36%);"
                },
                ground: {
                    color: "background:linear-gradient(105deg,#E0C068 30px,#5A5A5A 31px,#5A5A5A)",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/5/58/Ground_icon.png/20px-Ground_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#E0C068 36%, #ffffff 36%);"
                },
                flying: {
                    color: "background:linear-gradient(105deg,#A890F0 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/f/f0/Flying_icon.png/20px-Flying_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#A890F0 36%, #ffffff 36%);"
                },
                poison: {
                    color: "background:linear-gradient(105deg,#A040A0 30px,#5A5A5A 31px,#5A5A5A)",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/8/84/Poison_icon.png/20px-Poison_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#A040A0 36%, #ffffff 36%);"
                },
                fighting: {
                    color: "background:linear-gradient(105deg,#C03028 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/7/7d/Fighting_icon.png/20px-Fighting_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#C03028 36%, #ffffff 36%);"
                },
                psychic: {
                    color: "background:linear-gradient(105deg,#F85888 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/a/a6/Psychic_icon.png/20px-Psychic_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#F85888 36%, #ffffff 36%);"
                },
                dark: {
                    color: "background:linear-gradient(105deg,#705848 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/3/33/Dark_icon.png/20px-Dark_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#705848 36%, #ffffff 36%);"
                },
                rock: {
                    color: "background:linear-gradient(105deg,#B8A038 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/f/ff/Rock_icon.png/20px-Rock_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#B8A038 36%, #ffffff 36%);"
                },
                bug: {
                    color: "background:linear-gradient(105deg,#A8B820 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/7/79/Bug_icon.png/20px-Bug_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#A8B820 36%, #ffffff 36%);"
                },
                ghost: {
                    color: "background:linear-gradient(105deg,#705898 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/8/82/Ghost_icon.png/20px-Ghost_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#705898 36%, #ffffff 36%);"
                },
                steel: {
                    color: "background:linear-gradient(105deg,#B8B8D0 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/b/b8/Steel_icon.png/20px-Steel_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#B8B8D0 36%, #ffffff 36%);"
                },
                dragon: {
                    color: "background:linear-gradient(105deg,#7038F8 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/9/91/Dragon_icon.png/20px-Dragon_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#7038F8 36%, #ffffff 36%);"
                },
                fairy: {
                    color: "background:linear-gradient(105deg,#EE99AC 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/5/5a/Fairy_icon.png/20px-Fairy_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#EE99AC 36%, #ffffff 36%);"
                },
                stelar: {
                    color: "background:linear-gradient(105deg,#7CC7B2 30px,#5A5A5A 31px,#5A5A5A);",
                    icon: "https://archives.bulbagarden.net/media/upload/thumb/9/9f/Stellar_icon.png/20px-Stellar_icon.png",
                    bgn: "background:radial-gradient(circle at 50% 0%,#7CC7B2 36%, #ffffff 36%);"
                },
            },
            data:'',
            err: false
        }
    },
    beforeMount(){
        this.getPokemon();
    },
    created(){
    },
    methods:{
        async getPokemon(){
            try {
                this.err = false;
                const maxPokemon = 1025;
                const numPokemon = this.data != ''  ? this.data.toLowerCase() : Math.floor(Math.random() * maxPokemon);
                const url = `https://pokeapi.co/api/v2/pokemon/${numPokemon}/`;
                const data = await fetch(url)
                    .then(response => response.json())
                    .then(data => {
                        return data
                    });
                let pokeStructure = {
                    id: pokeId(data.id),
                    name: data.name,
                    sprite: data.sprites.other["official-artwork"].front_default,
                    sprite_dream_world: data.sprites.other.dream_world.front_default,
                    types: data.types.map((item) => {
                        return item.type.name
                    }),
                    moves: data.moves.map((item) => {
                        return item.move.name
                    }),
                    stats: data.stats.map((item) => ({
                        name: statsName(item.stat.name),
                        base_stat: item.base_stat,
                        color: statsColor(item.stat.name)
                    }))
                }

                // console.log(pokeStructure)

                this.pokemon = pokeStructure
                this.dataLoaded = true;
            } catch (error) {
                console.error('sucedio un error');
                this.err = true;
            }

            function pokeId(id) {
                id = id.toString();
                if (id.length === 1) {
                    id = "00" + id;
                }
                if (id.length === 2) {
                    id = "0" + id;
                }
                return id;
            }

            function statsName(name) {
                let names = {
                    attack: './icons/AttackIcon.svg',
                    defense: './icons/DefenseIcon.svg',
                    hp: './icons/HpIcon.svg',
                    "special-attack": './icons/SpecialAttackIcon.svg',
                    "special-defense": './icons/SpecialDefenseIcon.svg',
                    speed: './icons/SpeedIcon.svg',
                }

                return names[name];
            }

            function statsColor(name) {
                let colors = {
                    attack: "background:linear-gradient(105deg,#c0c0c0 30px,#5A5A5A 31px,#5A5A5A);",
                    defense: "background:linear-gradient(105deg,#4682b4 30px,#5A5A5A 31px,#5A5A5A);",
                    hp: "background:linear-gradient(105deg,#e32636 30px,#5A5A5A 31px,#5A5A5A);",
                    "special-attack": "background:linear-gradient(105deg,#c0c0c0 30px,#5A5A5A 31px,#5A5A5A);",
                    "special-defense": "background:linear-gradient(105deg,#4682b4 30px,#5A5A5A 31px,#5A5A5A);",
                    speed: "background:linear-gradient(105deg,#7CC7B2 30px,#5A5A5A 31px,#5A5A5A);",
                }
                return colors[name]
            }
        },
        searchPokemon(){
            this.getPokemon();
        }
    }
}
</script>

<style>
.card {
    position: relative;
    border-style: solid;
    border-color: #00b894;
    border-width: 10px;
    box-shadow: 0 20px 30px rgba(0, 0, 0, 0.15);
    border-radius: 10px;
    isolation: isolate;
    img{
        display: block;
        max-width: 180px;
        aspect-ratio: 1/1;
        position: relative;
        margin: 20px auto;
    }
}

.name{
    text-align: center;
    width: 290px;
    height: 24px;
    font-weight: 600;
    text-transform: capitalize;
    color: #404060;
}

.id{
    position: absolute;
    top: 1rem;
    left: 50%;
    transform: translate(-50%,-20%);
    font-size: 6rem;
    font-weight: 800;
    z-index: -1;
    color: #ffffff;
}
.types {
    display: flex;
    text-transform: capitalize;
    justify-content: space-around;
    margin: 20px 0 40px 0;
    span{
        font-size: 12px;
        letter-spacing: 0.4px;
        font-weight: 600;
        padding: 2px 2px 2px 4px;
        border-radius:20px;
        color: #ffffff;
        display:inline-flex;
        align-items: center;
        min-width: 50px;
        img{
            width:20px;
            height:20px;
            margin: 0 auto;
        }
        p{
             margin: 0 5px 0 10px;
        }
    }
}
.stats {
    text-transform: capitalize;
    margin: 20px 0 40px 0;
    justify-content: center;
    span{
        font-size: 12px;
        letter-spacing: 0.4px;
        font-weight: 600;
        padding: 2px 2px 2px 4px;
        border-radius:20px;
        color: #ffffff;
        display:inline-flex;
        align-items: center;
        min-width: 50px;
        margin: 5px;
        .svg{
            width:20px;
            height:20px;
            margin: 0;
            filter: invert(100%);
        }
        p{
             margin: 0 auto;
        }
    }
}
.loader {
    border: 24px solid #FFF;
    border-color: #FF3D00 #FF3D00 #fff #fff;
    border-radius: 50%;
    position: relative;
    animation: rotate 1s linear infinite
}
.loader:before {
    content: '';
    position: absolute;
    top: 50%;
    transform: translate(-50% , -125%);
    left: 50%;
    width: 12px;
    height: 12px;
    background: #fff;
    border-radius: 50%;
    }
@keyframes rotate {
    100%   { transform: rotate(360deg)}
}

.fade-enter-from {
  opacity: 0;
}

.fade-enter-active {
  transition: opacity .5s ease-out;
}

.fade-enter-to {
  opacity: 1;
}

.fade-leave-from {
  opacity: 1;
}

.fade-leave-active {
  transition: opacity .3s ease-in;
}

.fade-leave-to {
  opacity: 0;
}

</style>
