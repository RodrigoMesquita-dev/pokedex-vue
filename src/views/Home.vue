<template>
  <div class="container">
    <div class="row mt-2">
      
      <!-- início lado esquerdo -->
      <div class="col mb-2">
        <div class="card palco">
          <div class="card-header"></div>

          <div class="card-body bg-pokebola bg-normal">
            <div class="pokemon">
              <!-- <transition name="pulo" :duration="{ enter: 500, leave: 1000 }"> 
                <img src="@/assets/imgs/pokemons/001.png" v-show="exibir">
              </transition> --> <!--possui 6 classes css para controlar os estados do evento na animação-->   
              <transition
                @after-enter="exibirEvolucoesTransicao"
                @before-leave="ocultarEvolucoesTransicao"
                
                enter-active-class="animate__animated animate__bounce"
                leave-active-class="animate__animated animate__bounce" 
              > <!-- biblioteca animate.style -->
                <img :src="pokemon.imagem ? require(`@/assets/imgs/pokemons/${pokemon.imagem}`) : null" v-show="exibir">
                <!-- <img :src="null" v-show="exibir"> -->
                <!-- <img src="@/assets/imgs/pokemons/001.png" v-show="exibir"> -->
              </transition>
              <div class="evolucoes">
                <transition name="fade" v-for="e in pokemon.evolucoes" :key="e"> <!--possui 6 classes css para controlar os estados do evento na animação-->   
                <img
                  :src="e && require(`@/assets/imgs/pokemons/${e.toString().padStart(3, '0')}.png`)"
                  v-show="exibirEvolucoes">
                </transition>
              </div>
            </div>
          </div>

          <div class="card-footer">
         
          <nav class="nav nav-pills nav-fill">
            <!-- menu de navegação -->
            <router-link
              class="nav-item nav-link text-white"
              :to="{ path: '/sobre' }"
              exact-active-class="active"
            >
            Sobre
            </router-link>
            <router-link
              class="nav-item nav-link text-white"
              :to="{ path: '/status' }"
              exact-active-class="active"
            >
            Status
            </router-link>
            <router-link
              class="nav-item nav-link text-white"
              :to="{ path: '/habilidades' }"
              exact-active-class="active"
            >
            Habilidades
            </router-link>
          </nav>

          <div class="detalhes">
            <!-- exibe dados de acordo com o menu de navegação -->
            
              <router-view 
                v-slot="{ Component }"
                :pokemon="pokemon"
                @adicionarHabilidade="adicionarHabilidade"
                @removerHabilidade="removerHabilidade"
              >
                <transition
                  enter-active-class="animate__animated animate__zoomInDown"
                >
                  <component :is="Component"></component>
                </transition>
              </router-view>
          </div>

          </div>
        </div>
      </div>
      <!-- fim lado esquerdo -->

      <!-- início lado direito -->
      <div class="col mb-2 pokedex">        
        <div class="row">
          <div class="col">
            <h1>Pokédex</h1>
          </div>
        </div>

        <div class="row">
          <div class="col">
            <select class="form-select" v-model="ordenacao">
              <option value="" disabled>Ordenar Pokémon</option>
              <option value="1" >Id crescente</option>
              <option value="2" >Id decrescrente</option>
              <option value="3" >De A - Z</option>
              <option value="4" >De Z - A</option>
            </select>
          </div>
        
          <div class="col">
            <!-- <input
              type="text"
              class="form-control"
              placeholder="Pesquisar pokémon na tecla enter"
              v-model="nomePokemon"
              @keyup.enter="filtrarPokemonsPorNome"
            > -->
            <input
              type="text"
              class="form-control"
              placeholder="Pesquisar pokémon watch"
              v-model="nomePokemon2"
              @keyup.enter="filtrarPokemonsPorNome"
            >
          </div>
        </div>

        <div class="row">
          <div class="pokedex-catalogo">

            <!-- início listagem dinâmica -->
            <transition-group name="ordenacao" >
              <div
                v-for="p in pokemons"
                :key="p.id"
                :class="`cartao-pokemon bg-${p.tipo}`"
                @click="analisarPokemon(p)"
              >
                <h1>{{ p.id }} {{ p.nome }}</h1>
                <span>{{ p.tipo }}</span>
                <div class="cartao-pokemon-img">
                  <transition
                    appear1
                    enter-active-class="animate__animated animate__fadeInDown"
                  >
                  <img :src="require(`@/assets/imgs/pokemons/${p.imagem}`)">
                  
                  </transition>
                </div>
              </div>
            </transition-group>
            <!-- fim listagem dinâmica -->

          </div>
        </div>
      </div>
      <!-- fim lado direito -->

    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data: () => ({
    exibir: false,
    exibirEvolucoes: false,
    pokemon: {},
    pokemons: [],
    ordenacao: '',
    nomePokemon: '',
    nomePokemon2: '',
  }),
  watch: {
    ordenacao(valorNovo) {
      if (valorNovo == 1) {
        this.pokemons.sort((prox, atual) => {
          if (Number(atual.id) < Number(prox.id)) return 1;
          if (Number(atual.id) > Number(prox.id)) return -1;
          return 0
        });
      }
      if (valorNovo == 2) {
        this.pokemons.sort((prox, atual) => {
          if (Number(atual.id) < Number(prox.id)) return -1;
          if (Number(atual.id) > Number(prox.id)) return 1;
          return 0
        });
      }
      if (valorNovo == 3) {
        this.pokemons.sort((prox, atual) => {
          if (atual.nome < prox.nome) return 1;
          if (atual.nome > prox.nome) return -1;
          return 0
        });
      }
      if (valorNovo == 4) {
        this.pokemons.sort((prox, atual) => {
          return atual.nome.localeCompare(prox.nome)
        });
      }
    },
    nomePokemon2(valorNovo) {
      fetch(`http://localhost:3000/pokemons?nome_like=${valorNovo}`)
        .then(response => {
          return response.json()
        })
        .then(data => {
          console.log(data);
          this.pokemons = data;
        })
    }
  },
  created() {
    fetch('http://localhost:3000/pokemons')
      .then(response => {
        return response.json()
      })
      .then(data => {
        this.pokemons = data;
      })
  },
  methods: {
    exibirEvolucoesTransicao() {
      this.exibirEvolucoes = true;
    },
    ocultarEvolucoesTransicao() {
      this.exibirEvolucoes = false;
    },
    analisarPokemon(p) {
      let mudaPokemon = false;
      if (!(this.pokemon && (this.pokemon.id != p.id) && (this.exibir))) {
        this.exibir = !this.exibir;
        this.exibirEvolucoes = !this.exibirEvolucoes;
        mudaPokemon = true;
      }
        this.pokemon = p;

      // this.pokemon = p;
      if(!this.exibir && mudaPokemon) {
        this.pokemon = {}
      }
    },
    adicionarHabilidade(habilidade) {
      this.pokemon.habilidades.push(habilidade);
    },
    removerHabilidade(indice) {
      if(this.pokemon.habilidades[indice]){
        this.pokemon.habilidades.splice(indice, 1)
      }
    },
    filtrarPokemonsPorNome() {
      fetch(`http://localhost:3000/pokemons?nome_like=${this.nomePokemon}`)
        .then(response => {
          return response.json()
        })
        .then(data => {
          console.log(data);
          this.pokemons = data;
        })
    }
  }
}
</script>

<style>
body {
  background-color: #dee3eb;
}
</style>

<style scoped>

@import '~@/assets/css/animacoes.css';

.pokedex {
  padding: 20px;
  background-color: #ffffff;
  -webkit-box-shadow: 2px 2px 10px rgba(200, 200, 200, 0.77);
  -moz-box-shadow: 2px 2px 10px rgba(200, 200, 200, 0.77);
  box-shadow: 2px 2px 10px rgba(200, 200, 200, 0.77);
  border-radius: 10px;
}

.pokedex-catalogo {
  overflow: auto;
  display: flex;
  flex-wrap: wrap;
  height: 400px;
  width: 98%;
  margin-top: 10px;
}

.cartao-pokemon {
  position: relative;
  margin: 5px;
  width: 150px;
  height: 115px;
  cursor: pointer;
  border-radius: 5px;
  -webkit-box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
  -moz-box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
  box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
}

.cartao-pokemon h1{
  color:#fff;
  font-size: 14px;
  margin: 5px 0px 0px 5px;
  padding: 0px;
}

.cartao-pokemon span{
  color:#fff;
  position: absolute;
  background: rgba(255, 255, 255, 0.3);
  font-size: 12px;
  margin: 10px 0px 0px 5px;
  padding: 5px 10px 5px 10px;
  border-radius: 25px;
}

.cartao-pokemon img {
    max-width:60%;
    max-height:60%;
    float: right;
}

.bg-grama {
  background-color: #2d8f78;
}

.bg-fogo {
  background-color: #e47373
}

.bg-agua {
  background-color: #5a9ed2
}

.bg-inseto {
  background-color: #26d3ab
}

.bg-normal {
  background-color: #cecece
}

.bg-pokebola {
  background-image: url("~@/assets/imgs/pokebola.png");
  background-repeat: no-repeat;
  background-position: bottom right;
}

.palco {
  color: #fff;
  background-color: #333;
  -webkit-box-shadow: 2px 2px 10px rgba(230, 223, 223, 0.77);
  -moz-box-shadow: 2px 2px 10px rgba(230, 223, 223, 0.77);
  box-shadow: 2px 2px 10px rgba(230, 223, 223, 0.77);
  border-radius: 10px;
}

.pokemon {
  display: block;
  text-align: center;
  height: 215px;
}

.detalhes {
  margin: 20px 30px 20px 30px;
}

.evolucoes {
  position: absolute;
  top: 0px;
  right: 0px;
  height: 70px;
}

.evolucoes img {
  cursor: pointer;
  max-width: 100%;
  max-height: 100%;
}

</style>