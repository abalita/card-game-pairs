<template>
    <v-layout row wrap>
        <v-flex xs12 pa-4>
            <v-card>
                <v-toolbar dark color="primary">
                    <span class="title font-weight-thin">Cards on Hand</span>
                </v-toolbar>
                <v-card-text>
                    <v-layout align-center justify-center row wrap>
                        <v-flex xs1 v-for="card in cards" :key="card.code">
                             <v-img :src="card.image_url" @click="getCard(card)"></v-img>
                        </v-flex>
                    </v-layout>                   
                </v-card-text>
                <v-divider></v-divider>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="success" @click="init">Refresh</v-btn>
                    <v-btn  color="primary" @click="pair()">Pair</v-btn>
                </v-card-actions>
            </v-card>
        </v-flex>
        <v-dialog
            v-model="dialog"
             :overlay="true"
             :scrollable="false"
            transition="dialog-transition"
            width="215"
        >       
         <v-card>
             <v-card-title primary-title>
                 {{selected_card.card}} of {{selected_card.sign}}
             </v-card-title>
             <v-card-text>
                 <!-- <v-img :src="selected_card.image_url"></v-img> -->
                 <v-layout row wrap>
                        <v-flex xs12>
                             <v-img :src="selected_card.image_url"></v-img>
                        </v-flex>
                    </v-layout>
             </v-card-text>
             <v-divider></v-divider>
             <v-card-actions>
                 
                 <v-spacer></v-spacer>
                 
                 <v-btn outline color="success" @click="play">PLay</v-btn>
             </v-card-actions>             
         </v-card> 
        </v-dialog>
    </v-layout>
</template>

<script>
export default {
    data(){
        return{
            user:{},
            cards:[], 
            dialog:false,
            selected_card:{}
        }
        
    },
    created(){
        this.user = this.$store.state.user
        // alert(JSON.stringify(this.$store.state))
        this.init()
    },
    methods:{
        init(){            
            this.$http.get("http://ccci-cards.herokuapp.com/api/cards/" + this.user._id)
            .then(response => {
                console.log(JSON.stringify(response.data))
                this.cards = response.data.cards
            })
        },
        getCard(card){
            this.selected_card = card;
            this.dialog = true;
            
        },
        play(){
            // alert(this.selected_card._id)
            this.$http.post("http://ccci-cards.herokuapp.com/api/play/"+ this.user._id,{"card": this.selected_card._id})
            .then(response => {
                alert(JSON.stringify(response.data.message))
                return this.$http.get("http://ccci-cards.herokuapp.com/api/cards/" + this.user._id)
            })
            .then(response => {
                // console.log(JSON.stringify(response.data))
                this.cards = response.data.cards
                this.dialog = false;
            })
            .catch(err=>{
                alert(err)
            })
        },
        pair(){
            this.$http.get("http://ccci-cards.herokuapp.com/api/cards/pair/" + this.user._id)
            .then(response => {
                return this.$http.get("http://ccci-cards.herokuapp.com/api/cards/" + this.user._id)
            })
            .then(response => {
                console.log(JSON.stringify(response.data))
                this.cards = response.data.cards
            })
        }
    }

}
</script>

<style>

</style>

