<template>
  <v-container fluid>
    <h1>Send Email</h1><br/>
    <v-slide-y-transition mode="out-in">
      <v-layout row wrap> 
        <v-flex xs12>
          <v-text-field
            label="Email"
            outline
            v-model="email"
          />
        </v-flex>
        <v-flex xs12>
          <v-text-field
            label="Subject"
            outline
            v-model="subject"
          />
        </v-flex>
        
        <v-flex xs12>
          <v-textarea
            label="Contents"
            outline
            v-model="contents"
          />
        </v-flex>
        <v-flex text-xs-center>
          <v-btn color="primary" @click="sendEmail">Send Email</v-btn>
        </v-flex>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'SendEmail',
    data: () => ({
      email: '',
      subject: '',
      contents: '',
    }),

    methods: {
      sendEmail(){
        console.log(this.email);
        console.log(this.subject);
        console.log(this.contents);

        axios.post('/api/sendEmail', {
          email: this.email,
          subject: this.subject,
          contents: this.contents,
        })
        .then(response => {
          console.log(response);
        })
        .catch(e => {
          console.log(e);
        });

        this.email = '';
        this.subject = '';
        this.contents = '';

      },

    },
  }
</script>