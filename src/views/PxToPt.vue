<template>
  <v-container fluid>
    <h1>Pixel To Point Convertor</h1><br/>
    <v-slide-y-transition mode="out-in">
      <v-layout row wrap> 
        <v-flex xs12>
          <h2>Pixel To Point</h2><br/>
        </v-flex>
        <v-flex xs7>
          <v-text-field
            label="px"
            outline
            v-model="inputPX"
          />
        </v-flex>
        <v-flex xs1>
          
        </v-flex>
        <v-flex xs4>
          <v-card dark color="secondary">
            <v-card-text>{{outputPT}}pt</v-card-text>
          </v-card>
        </v-flex>
        <v-flex xs12>
          <h2>Pixel To Point(Only height and width)</h2><br/>
        </v-flex>
        <v-flex>
          <v-textarea
            v-on:drop="dropFile"
            label="Input"
            outline
            value=""
            v-model="source"
          />
        </v-flex>
        <v-flex text-xs-center>
          <v-btn large color="primary" @click="convert">Convert</v-btn>
        </v-flex>
        <v-flex>
          <v-textarea
            label="Output"
            outline
            value=""
            v-model="destination"
          />
        </v-flex>
      </v-layout>
    </v-slide-y-transition>
    <div v-html="outputHtml"></div>
  </v-container>
</template>

<script>
  let jsdiff = require('diff');
  let diff2html = require("diff2html").Diff2Html;
  
  export default {
    name: 'PxToPt',
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
      // convertPXtoPT: function(){
      //   this.outputPT = this.inputPX * 0.75;
      // },
      dropFile: function(e){
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
                  
           //console.log(files);
          
        
        
      },
      convert: function(){

        const WIDTH = 'width="';
        const HEIGHT = 'height="';
        let src = this.source;
        let dest = this.convertStyle(src);
        dest = this.convertAttribute(dest, HEIGHT);
        dest = this.convertAttribute(dest, WIDTH);

        this.destination = dest;     
        let strInput = jsdiff.createPatch('diff', this.source, this.destination);
        console.log(strInput);
        this.outputHtml = Diff2Html.getPrettyHtml(strInput, {inputFormat: 'diff', matching: 'lines'});
  
      },
      getFirst: function(src, WIDTH, HEIGHT){
        let tag = WIDTH;
        if (src.indexOf(WIDTH) < 0) {
          tag = HEIGHT;
        }
        else if (src.indexOf(WIDTH) > 0 && src.indexOf(HEIGHT) > 0) {
          tag = src.indexOf(WIDTH) > src.indexOf(HEIGHT) ? HEIGHT : WIDTH;
        }
        
        return tag;
      },
      convertStyle: function(src){
        let dest = '';
        const WIDTH = 'width:';
        const HEIGHT = 'height:';
        const PX = 'px';
        const PT = 'pt';
        
        while(true){
          let vStart = 0;
          let value = 0;
          let vEnd = 0;

          let tag = this.getFirst(src, WIDTH, HEIGHT);
          
          console.log(tag);
        
          if(src.indexOf(tag) < 0){
            break;
          }

          let temp = src.substring(0, src.indexOf(tag) + tag.length);

          console.log(temp);

          console.log(src.substring(src.indexOf(tag)-1, src.indexOf(tag)));

          dest += temp;

          vStart = src.indexOf(tag) + tag.length;
          vEnd =  vStart + src.substring(vStart, src.length).indexOf(PX);
          value = src.substring(vStart, vEnd).trim();
          console.log(value);

          if(src.substring(src.indexOf(tag)-1, src.indexOf(tag)) === "-" || isNaN(value)){   
            src = src.substring(vStart, src.length);
          }else{
            dest += value * 0.75;
            dest += PT;
            src = src.substring(vEnd+2, src.length);
          }

          //console.log(dest);
          //console.log(vStart);
          //console.log(vEnd);
      
        }

        dest += src.substring(0, src.length);

        return dest;
      },
      convertAttribute: function(src, t){
        let dest = '';
        const WIDTH = 'width="';
        const HEIGHT = 'height="';
        const VENDTAG = '"';
        const STARTTAG = '<';
        const ENDTAG = '>';
        const STYLE = 'style="';
        let STYLEWH = 'width:';

        if(t === HEIGHT){
          STYLEWH = 'height:';
        }


        while(true){

          let vStart = 0;
          let value = 0;
          let vEnd = 0;

          let tag = t;//this.getFirst(src, WIDTH, HEIGHT);

          if(src.indexOf(tag) < 0){
            break;
          }


          //let temp = src.substring(0, src.indexOf(tag) + tag.length);

          //console.log(temp);

          //dest += temp;

          vStart = src.indexOf(tag) + tag.length;  
          vEnd =  vStart + src.substring(vStart, src.length).indexOf(VENDTAG);
          value = src.substring(vStart, vEnd).trim();
          console.log(value);

          let vTemp = src.substring(src.substring(0,vEnd).lastIndexOf(STARTTAG), vEnd + src.substring(vEnd, src.length).indexOf(ENDTAG));
        
          if(vTemp.indexOf(STYLEWH) > 0 || isNaN(value) || src.substring(0,vEnd).lastIndexOf(STARTTAG) < 0){
            dest += src.substring(0, vStart);
            src = src.substring(vStart, src.length);
            console.log(src);
          }else{
            if(vTemp.indexOf(STYLE) > 0){
              dest += src.substring(0, src.substring(0,vEnd).lastIndexOf(STARTTAG) + vTemp.indexOf(STYLE) + STYLE.length);
              if(tag === WIDTH)
                dest += 'width:';
              else
                dest += 'height:'
              dest += value * 0.75;
              dest += 'pt;';
              src = src.substring(src.substring(0,vEnd).lastIndexOf(STARTTAG) + vTemp.indexOf(STYLE) + STYLE.length, src.length);
            }else{
              dest += src.substring(0, vEnd + vTemp.indexOf(ENDTAG) + 1 + ENDTAG.length);
              if(tag === WIDTH)
                dest += ' style="width:';
              else
                dest += ' style="height:';
              dest += value * 0.75;
              dest += 'pt;"';
              src = src.substring(vEnd + vTemp.indexOf(ENDTAG) + 1 + ENDTAG.length, src.length);
            }  

            //src = src.substring(vEnd+2, src.length);
          }
        
        }

        dest += src.substring(0, src.length);

        return dest;
      }
    }
  }
</script>