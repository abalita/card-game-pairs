<template>
    <v-layout row wrap>
        <v-flex xs6 pa-5 offset-xs3>
            <v-card>
                <v-toolbar dark color="primary">
                    <span class="title font-weight-light">Login</span>
                </v-toolbar>
                <v-card-text>
                  <v-text-field
                    name="name"
                    label="Username"
                    id="id"
                    v-model="username"
                  ></v-text-field>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="primary" @click="join">Join</v-btn>
                </v-card-actions>
            </v-card>
        </v-flex>
    </v-layout>
</template>

<script>
export default {
  data(){
    return{
      username: ''
    }
  },
  methods:{
    join(){
      this.$http.get("https://ccci-cards.herokuapp.com/api/users/" + this.username)
      .then(response => {
        // alert(JSON.stringify(response.data));
        this.$store.commit('JOIN', response.data)
        this.$router.push('/game')
      })
    }
  }
}
</script>

<style>

</style>

