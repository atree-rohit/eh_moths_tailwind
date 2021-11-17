<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.app{
    margin-top:60px;
}
.family-row{
    margin:  10px 0;
    padding: 10px 5px;
    transition: all .25s;
}
.family-row:hover{
    cursor: pointer;
    background: #6ce;
    color: #ff0;
}
.genus-row{
    /*display: flex;
    justify-content: space-between;
    flex-wrap: wrap;*/
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
    grid-gap: 5px;
    text-align: center;
}
    /*padding: 5px 0 !important;*/
.species-card{
    transition: all .3s;
}
.species-card:hover{
    background: #ffa;
    cursor: pointer;
}
.species-card:hover{
    transform: scale(1.05)
}
img{
    height:auto;
    width:auto;
    max-height:calc(17.5vw / 3 * 2);
    max-width: 17.5vw;
    object-fit: cover;
    transition: all .3s;
}

.modal {
  display: block;
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

.modal-content p{
    text-indent: 50px;
    color: #555;
    margin: 10px 0;
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>

<template>
    <div class="app">
        <w-card v-for="(superfamilyData, superfamily) in tree" :key="superfamily">
            <template v-for="(familyData, family) in superfamilyData" :key="family">
                <div class="family-row title2 info-light2--bg"
                v-text="cardFamilyTitle(superfamily, family)"
                @click="showFamilyModal(family)"
                >
                </div>
                <div class="genus-row">
                    <template v-for="(subfamilyData, subfamily) in familyData" :key="subfamily">
                        <template v-for="(tribeData, tribe) in subfamilyData" :key="tribe">
                            <template v-for="(genusData, genus) in tribeData" :key="genus">
                                <w-card v-for="species in genusData" class="species-card" :key="species[0]" @click='showSpeciesModal(species[1])'>
                                    <div>
                                        <img :src="require('@/assets/book_data/medium/'+species[1].img)" alt="">
                                    </div>
                                    <div class=""
                                    v-text="species[0]"
                                    ></div>
                                    <div>
                                        <!-- {{species[1]}} -->
                                    </div>
                                </w-card>
                            </template>
                        </template>
                </template>
            </div>
        </template>
    </w-card>
</div>

<div class="modal" v-if="familyModalDisplay">
    <!-- Modal content -->
    <div class="modal-content">
        <span class="close" @click="hideFamilyModal">&times;</span>
        <h1 class="title1">Family {{selectedFamily}}</h1>
        <p v-for="(p, k) in familyModalText" v-text="p" :key="k"></p>
    </div>
</div>
<div class="modal" v-if="speciesModalDisplay">
    <!-- Modal content -->
    <div class="modal-content" style="text-align:center;">
        <span class="close" @click="hideSpeciesModal">&times;</span>
        <h1 class="title1">{{taxonomy_data[selectedSpecies.taxa_id].species}}</h1>
        <img :src="require('@/assets/book_data/medium/'+selectedSpecies.img)" alt="">
        <h3 class="title2">Observed by: {{user_data[selectedSpecies.user_id][0]}}</h3>
        <h3 class="title2">Photo Date: {{selectedSpecies.observed_on}}</h3>
        <w-button class="ma1" bg-color="success" color="white" lg @click="gotoSpeciesPage(selectedSpecies)">Go to Species Page</w-button>
    </div>
</div>
</template>

<script>
import book_data from '../assets/book_data/book_data.json'
import taxonomy_data from '../assets/book_data/taxonomy_data.json'
import user_data from '../assets/book_data/user_data.json'
import species_descriptions from '../assets/book_data/descriptions.json'

export default {
    name: 'AllSpecies',
    data() {
        return {
            book_data: book_data,
            user_data: user_data,
            taxonomy_data: taxonomy_data,
            descriptions: species_descriptions,
            data:{},
            tree:{},
            levels: ["superfamily", "family", "subfamily", "tribe", "genus", "species"],
            selected:{},
            familyModalDisplay: false,
            speciesModalDisplay: false,
            selectedFamily:"",
            selectedSpecies: "",
        }
    },
    created() {
        this.init()
    },
    computed:{
        familyModalText(){
            let op = ""
            if(this.selectedFamily != ""){
                op = this.descriptionParagraphs(this.selectedFamily)
            }
            return op
        }
    },
    methods: {
        init() {
            this.levels.forEach(l => {
                this.selected[l] = ''
            })
            this.book_data.forEach(o => {
                let taxa = this.taxonomy_data[o.taxa_id]
                if(this.tree[taxa.superfamily] == undefined){
                    this.tree[taxa.superfamily] = {}
                }
                if(this.tree[taxa.superfamily][taxa.family] == undefined){
                    this.tree[taxa.superfamily][taxa.family] = {}
                }
                if(this.tree[taxa.superfamily][taxa.family][taxa.subfamily] == undefined){
                    this.tree[taxa.superfamily][taxa.family][taxa.subfamily] = {}
                }
                if(this.tree[taxa.superfamily][taxa.family][taxa.subfamily][taxa.tribe] == undefined){
                    this.tree[taxa.superfamily][taxa.family][taxa.subfamily][taxa.tribe] = {}
                }
                if(this.tree[taxa.superfamily][taxa.family][taxa.subfamily][taxa.tribe][taxa.genus] == undefined){
                    this.tree[taxa.superfamily][taxa.family][taxa.subfamily][taxa.tribe][taxa.genus] = []
                }
                this.tree[taxa.superfamily][taxa.family][taxa.subfamily][taxa.tribe][taxa.genus].push([taxa.species, o])
            })
        },
        cardFamilyTitle (superFamily, family){
            return superFamily + " > " + family
        },
        cardTitle (subfamily, tribe){
            let op = ""
            if(subfamily != 'none'){
                op += subfamily
            }
            if(tribe != 'none'){
                op += " > " + tribe
            }
            return op
        },
        select_taxa(l, t){
            if(this.selected[l] == t){
                this.selected[l] = ''
            } else {
                this.selected[l] = t
            }
        },
        descriptionParagraphs(family) {
            let op = [""]
            if(this.descriptions[family] != undefined)
                op = this.descriptions[family].split('<br>')
            return op
        },
        gotoSpeciesPage(o){
            let url = "https://www.inaturalist.org/taxa/" + o.taxa_id
            window.open(url, '_blank').focus();
        },
        showFamilyModal(f){
            this.selectedFamily = f
            this.familyModalDisplay = true
        },
        hideFamilyModal(){
            this.familyModalDisplay = false
        },
        showSpeciesModal(s){
            this.selectedSpecies = s
            this.speciesModalDisplay = true
        },
        hideSpeciesModal(){
            this.speciesModalDisplay = false
        }
    }
}
</script>
