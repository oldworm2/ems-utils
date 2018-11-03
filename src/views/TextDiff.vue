<template>
  <v-container fluid>
    <h1>Text Diff</h1><br/>
    <v-slide-y-transition mode="out-in">
      <v-layout row wrap> 
        
        <v-flex xs5>
          <v-textarea
            v-on:drop="dropFileA"
            label="Input"
            outline
            value=""
            v-model="source"
          />
        </v-flex>
        <v-flex xs2>
         
        </v-flex>
        <v-flex xs5>
          <v-textarea
            v-on:drop="dropFileB"
            label="Output"
            outline
            value=""
            v-model="destination"
          />
        </v-flex>

        <v-flex text-xs-center xs12>
          <v-btn large color="primary" @click="compare">Diff</v-btn>
        </v-flex>

      </v-layout>
    </v-slide-y-transition>
    <div v-html="outputHtml" id="display"></div>
  </v-container>
</template>

<script>
  let jsdiff = require('diff');
  let diff2html = require("diff2html").Diff2Html
  
  export default {
    name: 'TextDiff',
    data: () => ({
      inputPX: '',
      //outputPT: 0,
      source : '',
      destination : '',
      outputHtml: '',
    }),
    computed: {
      outputPT: function(){
        return this.inputPX * 0.75;
      }
    },
    methods: {
      dropFileA: function(e){
        e.preventDefault();
        let files = e.dataTransfer.files;

        let file = files[0];
        let self = this;
        if (file) {
          var reader = new FileReader();
          reader.readAsText(file, "UTF-8");
          reader.onload = function (evt) {
            self.source = evt.target.result;
              //console.log(t.source);
          }
        }
      },
      dropFileB: function(e){
        e.preventDefault();
        let files = e.dataTransfer.files;

        let file = files[0];
        let self = this;
        if (file) {
          var reader = new FileReader();
          reader.readAsText(file, "UTF-8");
          reader.onload = function (evt) {
            self.destination = evt.target.result;
              //console.log(t.source);
          }
        }
      },
      compare: function(){

        let strInput = jsdiff.createPatch('diff', this.source, this.destination);
        this.outputHtml = diff2html.getPrettyHtml(strInput, {inputFormat: 'diff', matching: 'lines', outputFormat: 'line-by-line'});
  

/*
        let diffWords = jsdiff.diffWords(this.source, this.destination);

        //console.log(diffWords);

        let display = document.getElementById('display');
        let fragment = document.createDocumentFragment();
        let color = '';
        let span = null;
        let retStr = '';

        diffWords.forEach(function(part){
          // green for additions, red for deletions
          // grey for common parts
          color = part.added ? 'green' :
            part.removed ? 'red' : 'grey';

          if(color == 'green'){
            retStr += `<ins>${part.value.replace(/</g,'&lt;').replace(/>/g,'&gt;')}</ins>`;
          }
          else if(color == 'red')
            retStr += `<del>${part.value.replace(/</g,'&lt;').replace(/>/g,'&gt;')}</del>`;
          else
            retStr += part.value.replace(/</g,'&lt;').replace(/>/g,'&gt;');

        });

        let cut = retStr.split('\n');



        let htmlStr = '<div class="d2h-file-diff">';
        htmlStr += '<div class="d2h-code-wrapper">';
        htmlStr += '<table class="d2h-diff-table">';
       

        cut.forEach(function(line, i){

  
          
          htmlStr += '<tr>';


          htmlStr += '<td class="d2h-ins d2h-change">';
          htmlStr += `<div class="line-num1">${i + 1}</div>`;
          htmlStr += '</td>';


          htmlStr += '<td class="d2h-code-line-ctn">';
          htmlStr += '<div class="d2h-code-line d2h-ins d2h-change">';
          htmlStr += '<span class="d2h-code-line-ctn">';
          htmlStr += line;
          htmlStr += '</span>';
          htmlStr += '</div>';
          htmlStr += '</td>';
          htmlStr += '</tr>';
          
        });

        htmlStr += '</table>';
        htmlStr += '</div>';
        htmlStr += '</div>';

        span = document.createElement('pre');
        span.appendChild(document
          .createTextNode(retStr));
        fragment.appendChild(span);
*/
        //display.appendChild(fragment);
//this.outputHtml = htmlStr;
        
      },
    }
  }
</script>