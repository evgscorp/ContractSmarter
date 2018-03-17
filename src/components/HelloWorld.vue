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
                      :counter="100"
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
// Import libraries we need.
import { default as Web3} from 'web3';
import { default as contract } from 'truffle-contract'

// Import our contract artifacts and turn them into usable abstractions.
import metacoin_artifacts from '../../build/contracts/MetaCoin.json'

// MetaCoin is our usable abstraction, which we'll use through the code below.
var MetaCoin = contract(metacoin_artifacts);
// The following code is simple to show off interacting with your contracts.
// As your needs grow you will likely need to change its form and structure.
// For application bootstßrapping, check out window.addEventListener below.
var accounts;
var account;

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
        v => v.length <= 100 || "Name must be less than 100 characters"
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

        if (typeof web3 !== 'undefined') {
    console.warn("Using web3 detected from external source. If you find that your accounts don't appear or you have 0 MetaCoin, ensure you've configured that source properly. If using MetaMask, see the following link. Feel free to delete this warning. :) http://truffleframework.com/tutorials/truffle-and-metamask")
    // Use Mist/MetaMask's provider
    window.web3 = new Web3(web3.currentProvider);
  } else {
    console.warn("No web3 detected. Falling back to http://127.0.0.1:9545. You should remove this fallback when you deploy live, as it's inherently insecure. Consider switching to Metamask for development. More info here: http://truffleframework.com/tutorials/truffle-and-metamask");
    // fallback - use your fallback strategy (local node / hosted node + in-dapp id mgmt / fail)
    window.web3 = new Web3(new Web3.providers.HttpProvider("http://127.0.0.1:9545"));
  }
    //    if (this.$refs.form.validate()) {
           // Bootstrap the MetaCoin abstraction for Use.
    MetaCoin.setProvider(web3.currentProvider);

    // Get the initial account balance so it can be displayed.
    web3.eth.getAccounts(function(err, accs) {
      if (err != null) {
        alert("There was an error fetching your accounts.");
        return;
      }

      if (accs.length == 0) {
        alert("Couldn't get any accounts! Make sure your Ethereum client is configured correctly.");
        return;
      }

      accounts = accs;
      account = accounts[0];
    });
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
   // }

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
