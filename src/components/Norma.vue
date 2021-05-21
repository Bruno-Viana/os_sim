<template>
  <div class="wrapperNorma">
    <div style="width:80vw;min-width:50vw">
      <h1>Simulador de Processos com paginação</h1>
      <form style="width:fit-content; margin: 0 auto;text-align:right;">
        Quantidade de Memória RAM:<input style="width:80px" type="number" v-model="MemPrincipal" min="3" max="50"><br>
        Número de páginas:<input style="width:80px" type="number" v-model="nPaginas" min="1" max="50"> <br>
      </form>
      <hr>
    </div>
    <div class="descBlock">
        <p style="margin-left: 8px">PROCESSOS</p><hr>
        <textarea style="text-align:left;	-webkit-box-sizing: border-box;-moz-box-sizing: border-box;box-sizing: border-box;width: 50%;" v-model="processo1" placeholder="1; Pid do processo
20;  endereços de memória necessários (tamanho do processo)
l 5; carrega o endereço 5
s 2, 15; armazena o valor 2 no endereço 5"></textarea>
        <textarea style="text-align:left;	-webkit-box-sizing: border-box;-moz-box-sizing: border-box;box-sizing: border-box;width: 50%;" v-model="processo2" placeholder="2; Identificador do processo
30; endereços de memória necessários (tamanho do processo)
l 6; carrega o endereço 6
s 0, 25; armazena valor 0 no endereço 25
s 1, 15; armazena valor 1 no endereço 15"></textarea><br>
        <label for='uploadBtn'><i class="fas fa-upload"></i>Selecionar um arquivo</label>
        <input id="uploadBtn" type="file" ref="myFile" @change="uploadArquivo1">
        <div style="float:right">
        <label for='uploadBtn2'><i class="fas fa-upload"></i>Selecionar um arquivo</label>
        <input id="uploadBtn2" type="file" ref="myFile2" @change="uploadArquivo2">
      </div>
      <p v-if="isTxt==false" style="color:red; font-size:20"><b>Erro:</b> Apenas permitidos arquivos com extensão .txt</p>
      </div><br>
      <button class="func1Btn" v-on:click='startProc()' id='c'><i class="far fa-play-circle"></i> Começar</button>
      <div class="logResultado"><h1 style="font-size:20px">Histórico/Log:</h1>
      <textarea id="a" readonly placeholder="Log..."  v-model="proc1Data"></textarea>
      </div>
  </div>
</template>

<script>
import jQuery from "jquery";      
export default {
  name: 'Norma',
      data()
      {
      return{
          MemPrincipal:null, nPaginas:null,
          processo1:null,processo2:null,
          proc1Data:[], proc2Data:[],
          isLtr:null, isTxt:null, isTxtReg:null, //Validadores de entrada
          valueInst:null,
          logVal:[],
          index:0,
          CountReg:2,terminou:null,
          Resultado:null,
      }
    },
  methods: {
    uploadArquivo1() { //Processo1
      let file = this.$refs.myFile.files[0]; //Apenas 1 arquivo por vez
      if(!file || file.type !== 'text/plain') this.isTxt=false; //Apenas texto/raw
        else{
        this.isTxt=true;  
        let reader = new FileReader();
        reader.readAsText(file, "UTF-8"); //Encoder
        reader.onload =  evt => {
          this.processo1 = evt.target.result; //Conteúdo do arquivo
          let arr=[]
          arr = this.processo1.split("\n")
          let x=0;
            if(arr[x].includes(";")){
              if(arr[x].includes(";")){
              for(let y = 0 ; y<arr.length; y++){
              this.proc1Data.push((arr[x+y].substring(arr[x+y].indexOf(";"),arr[x+y].indexOf("")-1).toUpperCase()))
              }
            }}
        }   
        reader.onerror = evt => {
          console.error(evt); //Não apagar
        }
      }
    },
    uploadArquivo2() { //Processo2
      let file = this.$refs.myFile2.files[0]; //Apenas 1 arquivo por vez
      if(!file || file.type !== 'text/plain') this.isTxt=false; //Apenas texto/raw
        else{
        this.isTxt=true;  
        let reader = new FileReader();
        reader.readAsText(file, "UTF-8"); //Encoder
        reader.onload =  evt => {
          this.processo2 = evt.target.result; //Conteúdo do arquivo
          let arr=[]
          arr = this.processo2.split("\n")
          let x=0;
            if(arr[x].includes(";")){
              for(let y = 0 ; y<arr.length; y++){
              this.proc2Data.push((arr[x+y].substring(arr[x+y].indexOf(";"),arr[x+y].indexOf("")-1).toUpperCase()))
              }
            }
        }   
        reader.onerror = evt => {
          console.error(evt); //Não apagar
        }
      }
    },
    startProc(){
      console.log(this.proc1Data,this.proc2Data)
      for(let x=0;x<this.proc1Data.length;x++){
          if(x==0){
            console.log()
          }
      }
    },
    testInst(){
      //Handling de variáveis
      this.logVal=[]
      this.index=0
      let arr=[]
      //let arraux=[]
      arr = this.processo1.split("\n")
      let tamanho=arr.length;
      console.log(tamanho)
      console.log(arr)
      let x=0;
      do{
        if(arr[x].includes(";")){
          if(arr[x].includes(";")){
            for(let y = 0 ; y<arr.length; y++){
             console.log((arr[x+y].substring(arr[x+y].indexOf(";"),arr[x+y].indexOf("")-1).toUpperCase()))
            }
            
            /*
            let pivot=(arr[x].substring(arr[x].indexOf(";"),arr[x].indexOf("v")-1).toUpperCase())
            console.log(pivot)
            console.log((arr[x+1].substring(arr[x+1].indexOf(";"),arr[x+1].indexOf("v")-1).toUpperCase()))
            console.log((arr[x+2].substring(arr[x+2].indexOf(";"),arr[x+1].indexOf("v")-1).toUpperCase()))
            console.log((arr[x+3].substring(arr[x+3].indexOf(";"),arr[x+1].indexOf("v")-1).toUpperCase()))
            */
            
          }
          else if(arr[x].includes("sub_")){
              let pivot=(arr[x].substring(arr[x].indexOf("sub_")+4,arr[x].indexOf("v")-1).toUpperCase())
              eval(`this.${pivot}--`)
              //console.log("Letra:" + pivot + " Val:" + eval(`this.${pivot}`))
              if(eval(`this.${pivot}`) == 0){
                 
                //this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A: " + this.A + " B: " + this.B + ")" + '\xa0' +  new Date().toLocaleString())
              } else{ //Handler se decrementar o pivot e ele ser 0 printa o último
              if(this.CountReg==2){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B +")" + '\xa0' +  new Date().toLocaleString())
              }
              else if(this.CountReg==3){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C + ")" + '\xa0' +  new Date().toLocaleString())
              }
              else if(this.CountReg==4){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D +")" + '\xa0' +  new Date().toLocaleString())
              }
              else if(this.CountReg==5){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E +")" + '\xa0' +  new Date().toLocaleString())
              }
              else if(this.CountReg==6){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F +")" + '\xa0' +  new Date().toLocaleString())
              }
              else if(this.CountReg==7){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F + " G:" + this.G +")" + '\xa0' +  new Date().toLocaleString())
              }
              }
              if(arr[x].includes("vá_para")){
                x=((arr[x].split(' ').pop().trim().substr(- 1))-1)
              }
          }
        }
        else if(arr[x].includes("se")){
           //console.log(this.valueInst[x].substring(this.valueInst[x].indexOf("zero_")+5,this.valueInst[x].indexOf("então")-1))
           //this.valueInst[x].substring(this.valueInst[x].indexOf("zero_")+ 5,12) //b ou a
          if(arr[x].substring(arr[x].indexOf("zero_")+5,arr[x].indexOf("então")-1)){
            let pivot;
            pivot=(arr[x].substring(arr[x].indexOf("zero_")+5,arr[x].indexOf("então")-1)).toUpperCase()
            this.terminou=false 
              if(eval(`this.${pivot}`) == 0){
                  if(arr[x].includes("vá_para")){
                    //console.log(xd)
                    //x=arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)
                  if(this.CountReg==2){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B +")" + '\xa0' +  new Date().toLocaleString())
                  }
                  else if(this.CountReg==3){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C + ")" + '\xa0' +  new Date().toLocaleString())
                  }
                  else if(this.CountReg==4){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D +")" + '\xa0' +  new Date().toLocaleString())
                  }
                  else if(this.CountReg==5){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E +")" + '\xa0' +  new Date().toLocaleString())
                  }
                  else if(this.CountReg==6){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F +")" + '\xa0' +  new Date().toLocaleString())
                  }
                  else if(this.CountReg==7){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F + " G:" + this.G +")" + '\xa0' +  new Date().toLocaleString())
                  }
                  //this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A: " + eval(`this.${pivot}`) + " " + pivot + " " + eval(`this.${pivot}`)+ ")" + '\xa0' +  new Date().toLocaleString())
                  
                  }
                  this.terminou=true;
              }else{
                  if(arr[x].includes("vá_para")){
                    //console.log(arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) //Final
                    //console.log(arr[x].split(' ').pop().trim().substr(- 1)) // Segundo vai_para após o senão
                    x=(arr[x].split(' ').pop().trim().substr(- 1)-1)
                  }
              }
             
          }//extrai qual valor é testado
        } //fim if
        }while(this.terminou==false)
        //(arr[x].substring(arr[x].indexOf("zero_")+5,arr[x].indexOf("então")-1)).toUpperCase()
    },
    func1(){ //Funções da demo
      do{
        this.func2();
      }while(this.B>0);
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "9," + " " + "A:" + this.A + " " + "B:" + this.B + ")" + '\xa0' +  new Date().toLocaleString());
    },
    func4(){
      this.B--;
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "4," + " " + "A:" + this.A + " " + "B:" + this.B + ")" + '\xa0' +  new Date().toLocaleString())
    },
    func3(){
      this.A++;
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "3," + " " + "A:" + this.A + " " + "B:" + this.B + ")" + '\xa0' +  new Date().toLocaleString())
      this.func4();
    },
    func2(){
      this.A++;
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "2," + " " + "A:" + this.A + " " + "B:" + this.B + ")" + '\xa0' +  new Date().toLocaleString())
      this.func3();
    },
  },
}

jQuery(function ($) {
  $("#c").on("click", function () {
      var avalue = $('#a').val();
      var newVal = avalue.replace(/^\s*[\r\n]/gm, '');
      //var finalResults = newVal.replace("\n", "");
      $('#a').val(newVal);
  });
})

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapperNorma{
  color:white;
  background-color: rgba(0, 0, 0, 0.164);
  margin: 0 auto;
  max-width: fit-content;
  text-align: center;
}
.tooltipReg{
  position: absolute;
  background-color:black;
  width:30%;
  margin-left: 25%
}
form>p>input, form>input{
  border:1px rgb(100, 59, 59) solid;
  width: 5%;
  overflow: auto;
}

.descBlock{
  text-align: left;
  border:1px white solid;
}
.descBlock>textarea{
  width: 98.5%;
  height: 100px
}
.descBlock>p.descBlock>span{
  margin-left: 3%;
}
.func1Btn{
  width: 100%;
  font-size: 24px;
  background: transparent;
  border: .6px white solid;
  color: white;
  cursor: pointer;
}

.logResultado>textarea{
  min-height: 25vh;
  max-height: fit-content;
  min-width: 98.5%
}

input[type='file'] {
  display: none
}

label {
  background-color: #ffffff;
  color: rgb(0, 0, 0);
  cursor: pointer;
  padding: 4px 20px
}
</style>