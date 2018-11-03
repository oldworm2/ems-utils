<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout row wrap align-center>

        <v-flex xs4>
          <v-card class="ma-2">

            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">PXToPT</h3>
                <div>You can convert from px to pt <br/> (only width and height)</div>
              </div>
            </v-card-title>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn flat color="orange" to="/pxtopt">GO</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      

        <v-flex xs4>
          <v-card class="ma-2">

            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Send Email</h3>
                <div>Send Email <br/> Using Nodemailer</div>
              </div>
            </v-card-title>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn flat color="orange" to="/sendemail">GO</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>

        <v-flex xs4>
          <v-card class="ma-2">

            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Text Diff</h3>
                <div>Online Text Diff <br>You can see unified diff</div>
              </div>
            </v-card-title>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn flat color="orange" to="/textdiff">GO</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
        
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
