<template>
  <div class="container home">
    <div class="row g-0 row-cols-5">
      <div v-for=" energia in energias" :key="energia.index" class="col" @click="atack(energia)"
        :style="{ backgroundImage: `url(${energia.img})`, backgroundRepeat: 'no-repeat', height: '240px', marginBottom: '10px' }">

        <p v-if="energia.type != 7"
          style="color: aliceblue; font-weight: bolder; font-size:xx-large; position: relative; right: 40%;">
          {{ energia.value }}</p>
        <p v-if="energia.type != 7"
          style="color: aliceblue; font-weight: bolder; font-size:xx-large; position: relative; left: 12%;top: 51%;">{{
            energia.value }}</p>

      </div> 
    </div>
  </div>
</template>

<script lang="ts" setup>
import { Options, Vue } from 'vue-class-component';
import { onMounted, onBeforeMount ,nextTick,ref,Ref} from 'vue';
import energy from '@/models/energy'
let energias: Ref<energy[]> = ref<energy[]>([]);
let atackStack: energy[] = [];
//let siguientes:energy[]=[];
const cards: energy[] = [{
  name: "Dragon/Beast",
  value: 0,
  img: "https://wikimon.net/images/f/f9/Dragon_beast_crusader_energy_card.jpg",
  type: 1,
  index: 0
}, {
  name: "Steel/Knight",
  value: 0,
  img: "https://wikimon.net/images/6/63/Steel_knight_crusader_energy_card.jpg",
  type: 2,
  index: 0
}, {
  name: "Nature",
  value: 0,
  img: "https://wikimon.net/images/f/fe/Nature_crusader_energy_card.jpg",
  type: 3,
  index: 0
}, {
  name: "Dark",
  value: 0,
  img: "https://wikimon.net/images/c/c6/Dark_crusader_energy_card.jpg",
  type: 4,
  index: 0
}, {
  name: "Holy",
  value: 0,
  img: "https://wikimon.net/images/d/d1/Holy_crusader_energy_card.jpg",
  type: 5,
  index: 0
}, {
  name: "Support",
  value: 0,
  img: "https://wikimon.net/images/5/51/Support_crusader_energy_card.png",
  type: 6,
  index: 0
}, {
  name: "Wild",
  value: 10,
  img: "https://wikimon.net/images/a/a0/Wild_crusader_energy_card.png",
  type: 7,
  index: 0
}];
function getRandom(max: number) {
  let number = Math.floor(Math.random() * max);
  if (number == 0) {
    number = 1
  }
  return number;
}
function getRandomInt(max: number) {
  return Math.floor(Math.random() * max);
}
const initEnergy = () => {

  for (var i = 1; i <= 10; i++) {
     var indice = getRandomInt(7);
    var card = {...cards[indice]};
    card.index = i;
    card.value = getRandom(10);
    energias.value.push(card);

  }
};

const atack =async (energy: energy) => {

   if (atackStack.length >= 3) {
    console.log("in");
      await removeEnergy(atackStack);
  } {
    if (atackStack.find(x => x == energy)) {
      atackStack = atackStack.filter(x => x != energy);
    } else {
      atackStack.push(energy);
    }
  }

  console.log(atackStack); 
}

const removeEnergy = async (energys:energy[]) =>{
  var energi = [...energias.value]
  if(energys!=null){
    for(var i=0;i<energys.length;i++){
      console.log(i);
      energi = energi.filter(x=>x!=energys[i] );
      console.log(energi,"energia");
    }
    energias.value = energi
    console.log(energias,"energias");
  }
  await nextTick()
}
onBeforeMount(() => {
  initEnergy();
})


</script>
<style>
.home {
  background-color: gray;
}</style>