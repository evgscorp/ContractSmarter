<template>
  <v-layout>
    <v-flex xs24 sm12  justify-center  align-center  >
      <v-card style="min-height:300px; max-width:500px; margin-left:auto; margin-right:auto;">
        <v-card-title primary-title>
          <div style="width:100%;">
            <h3  style="margin-top:1rem;"  class="headline">Baller Tournament</h3>
             <v-form v-model="valid">
                    <v-text-field
                      label="Name"
                      v-model="name"
                      :rules="nameRules"
                      :counter="10"
                      required
                    ></v-text-field>
                    <v-text-field
                      label="E-mail"
                      v-model="email"
                      :rules="emailRules"
                      required
                    ></v-text-field>
                     <v-text-field
                      label="Document number"
                      v-model="docnumber"
                      :rules="docnumberRules"
                      :counter="10"
                      required
                    ></v-text-field>

                     <v-checkbox 
                          label="I agree to pay for the participation"
                          v-model="checkbox"
                          :rules="[v => !!v || 'You must agree to continue!']"
                          required
                        ></v-checkbox>

                          <v-btn  color="success" @click="submit"  :disabled="!valid"  >Pay and Register </v-btn>           
                  </v-form>
          </div>
        </v-card-title>
      <!--  <v-card-actions>
         <v-btn @click="submit"  :disabled="!valid"  >Pay and Register </v-btn>
        </v-card-actions>-->
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      valid: false,
      name: "",
      nameRules: [
        v => !!v || "Name is required",
        v => v.length <= 10 || "Name must be less than 10 characters"
      ],
      docnumber:"",
      docnumberRules: [
        v => !!v || "Docnumber is required",
        v => v.length > 10 || "Docnumber must be more than 10 characters"
      ],

      email: "",
      emailRules: [
        v => !!v || "E-mail is required",
        v =>
          /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
          "E-mail must be valid"
      ],  
      checkbox: false,
      
      card_text:
        "Lorem ipsum dolor sit amet, brute iriure accusata ne mea. Eos suavitate referrentur ad, te duo agam libris qualisque, utroque quaestio accommodare no qui. Et percipit laboramus usu, no invidunt verterem nominati mel. Dolorem ancillae an mei, ut putant invenire splendide mel, ea nec propriae adipisci. Ignota salutandi accusamus in sed, et per malis fuisset, qui id ludus appareat."
    };
  }, 
  methods: {
      submit () {
        if (this.$refs.form.validate()) {
          // Native form submission is not yet supported
          axios.post('/api/submit', {
            name: this.name,
            email: this.email,
            docnumber: this.docnumber,
            checkbox: this.checkbox
          })
        }
      },
      clear () {
        this.$refs.form.reset()
      }
    }

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
