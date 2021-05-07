<template>
  <div class="wrapperNorma">
    <div style="width:80vw;min-width:50vw">
      <h1>Máquina Norma</h1>
      <form v-on:keypress="isNumber($event)">
        Quantidade de registradores: <input type="number" name="CountReg" v-model="CountReg" min="2" max="7"> <br>
        <br><label for='uploadBtnReg'><i class="fas fa-upload"></i>Selecionar um arquivo</label>
        <input id="uploadBtnReg" type="file" ref="myFileReg" @change="uploadArquivoReg"> <i style="font-size:20px;cursor:help" @mouseover="showHelp=true" @mouseleave="showHelp=false" class="far fa-question-circle"></i>
        <div class="tooltipReg" v-if="showHelp==true">
          <h1 style="font-size:20px;">Exemplos de arquivos txt válidos:</h1>
          reg=7 //Numero de reg<br>
          A=2 //Atribuição ao registrador A<br>
          B=3<br>
          C=3<br>
          D=1<br>
          G=4<br>
          Se nenhum valor foi setado e o número de registradores cobre o registrador ele iniciará como 0, como é o caso do E e do F neste exemplo.
        </div>
        <p v-if="isTxtReg==false" style="color:red; font-size:20"><b>Erro:</b> Apenas permitidos arquivos com extensão .txt</p>
        <p>Valor de entrada para A:<input type="text" name="A" v-model="Aalt"></p>
        <p>Valor de entrada para B:<input type="text" name="B" v-model="Balt"></p>
        <p v-if="CountReg>=3">Valor de entrada para C:<input type="text" name="C" v-model="Calt"></p>
        <p v-if="CountReg>=4">Valor de entrada para D:<input type="text" name="D" v-model="Dalt"></p>
        <p v-if="CountReg>=5">Valor de entrada para E:<input type="text" name="E" v-model="Ealt"></p>
        <p v-if="CountReg>=6">Valor de entrada para F:<input type="text" name="F" v-model="Falt"></p>
        <p v-if="CountReg>=7">Valor de entrada para G:<input type="text" name="G" v-model="Galt"></p>
      
      <p v-if="isLtr" style="color:red; font-size:20"><b>Erro:</b> Apenas números não decimais serão aceitos.</p>
      </form>
      <hr>
      <p><b>Registros inseridos:</b></p>
      <p>A: {{Aalt}}</p>
      <p>B: {{Balt}}</p>
      <p v-if="CountReg>=3">C: {{Calt}}</p>
      <p v-if="CountReg>=4">D: {{Dalt}}</p>
      <p v-if="CountReg>=5">E: {{Ealt}}</p>
      <p v-if="CountReg>=6">F: {{Falt}}</p>
      <p v-if="CountReg>=7">G: {{Galt}}</p>
    </div>
    <div class="descBlock">
      <p style="margin-left: 8px">INSTRUÇÕES</p><hr>
      <textarea style="text-align:left" v-model="valueInst" placeholder="id: condição/operação variável ex:           1: se zero_b então vá_para9 senão vá_para2"></textarea><br>
      <label for='uploadBtn'><i class="fas fa-upload"></i>Selecionar um arquivo</label>
      <input id="uploadBtn" type="file" ref="myFile" @change="uploadArquivo">
      <p v-if="isTxt==false" style="color:red; font-size:20"><b>Erro:</b> Apenas permitidos arquivos com extensão .txt</p>
      <p style="background-color:#1f1f1f"><b>Digitar no layout: </b><br>
        1: se zero_b então vá_para9 senão vá_para2 <br>
        2: faça ad_a vá_para3 <br>
        3: faça ad_a vá_para4 <br>
        4: faça sub_b vá_para1

      </p>
    </div><br>
    <button class="func1Btn" v-on:click='testInst()'><i class="far fa-play-circle"></i> Começar</button>
    <div class="logResultado"><h1 style="font-size:20px">Histórico/Log:</h1>
      <textarea readonly placeholder="Log..."  v-model="logVal"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Norma',
      data()
      {
      return{
          A:0,B:0,C:0,D:0,E:0,F:0,G:0, //Registradores
          Aalt:0, Balt:0, Calt:0, Dalt:0, Ealt:0, Falt:0, Galt:0,
          showHelp:false,
          handleReg:[],
          isLtr:null, isTxt:null, isTxtReg:null, //Validadores de entrada
          valueInst:null,
          logVal:[],
          index:0,
          CountReg:2,
          Resultado:null,
      }
    },
  methods: {
    uploadArquivoReg(){ //Macros de input
      let file = this.$refs.myFileReg.files[0]; //Apenas 1 arquivo por vez
      if(!file || file.type !== 'text/plain') this.isTxtReg=false; //Apenas texto/raw
        else{
        this.isTxtReg=true;  
        let reader = new FileReader();
        reader.readAsText(file, "UTF-8"); //Encoder
        reader.onload =  evt => {
          this.handleReg=evt.target.result;
          let arr=[]
          arr = this.handleReg.split("\n")
          console.log(arr)
          for(let x=0; x<arr.length;x++){
            if(arr[x].includes("reg=")){
              this.CountReg=(arr[x].substring(arr[x].indexOf("reg=")+4));
            }
            if(arr[x].includes("A=")){
              this.Aalt=(arr[x].substring(arr[x].indexOf("A=")+2))
            }
            if(arr[x].includes("B=")){
              this.Balt=0
              this.Balt=(arr[x].substring(arr[x].indexOf("B=")+2))
            }
            if(arr[x].includes("C=")){
              this.Calt=(arr[x].substring(arr[x].indexOf("C=")+2))
            }
            if(arr[x].includes("D=")){
              this.Dalt=(arr[x].substring(arr[x].indexOf("D=")+2))
            }
            if(arr[x].includes("E=")){
              this.Ealt=(arr[x].substring(arr[x].indexOf("E=")+2))
            }
            if(arr[x].includes("F=")){
              this.Falt=(arr[x].substring(arr[x].indexOf("F=")+2))
            }
            if(arr[x].includes("G=")){
              this.Galt=(arr[x].substring(arr[x].indexOf("G=")+2))
            }
          }

        }
        reader.onerror = evt => {
          console.error(evt); //Não apagar
        }
      }
    },
    uploadArquivo() { //Instruções
      let file = this.$refs.myFile.files[0]; //Apenas 1 arquivo por vez
      if(!file || file.type !== 'text/plain') this.isTxt=false; //Apenas texto/raw
        else{
        this.isTxt=true;  
        let reader = new FileReader();
        reader.readAsText(file, "UTF-8"); //Encoder
        reader.onload =  evt => {
          this.valueInst = evt.target.result; //Conteúdo do arquivo
        }
        reader.onerror = evt => {
          console.error(evt); //Não apagar
        }
      }
    },
    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57))==45) {
        this.isLtr=true;
        evt.preventDefault();
      } else {
        this.isLtr=false;
      }
    },
    testInst(){
      //Handling de variáveis
      this.A=this.Aalt; this.B=this.Balt, this.C=this.Calt ,this.D=this.Dalt, this.E=this.Ealt, this.F=this.Falt, this.G=this.Galt
      this.logVal=[]
      this.index=0
      let arr=[]
      arr = this.valueInst.split("\n")
      let x=0;
      do{
        if(arr[x].includes("faça")){
          if(arr[x].includes("ad_")){
            let pivot=(arr[x].substring(arr[x].indexOf("ad_")+3,arr[x].indexOf("v")-1).toUpperCase())
            console.log(eval(`this.${pivot}++`))
            console.log("Letra:" + pivot + " Val:" + eval(`this.${pivot}`))
            if(this.CountReg==2){
              this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B +")")
            }
            else if(this.CountReg==3){
              this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C + ")")
            }
            else if(this.CountReg==4){
              this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D +")")
            }
            else if(this.CountReg==5){
              this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E +")")
            }
            else if(this.CountReg==6){
              this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F +")")
            }
            else if(this.CountReg==7){
              this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F + " G:" + this.G +")")
            }
              if(arr[x].includes("vá_para")){
                x=((arr[x].split(' ').pop().trim().substr(- 1))-1)
              }
          }
          else if(arr[x].includes("sub_")){
              let pivot=(arr[x].substring(arr[x].indexOf("sub_")+4,arr[x].indexOf("v")-1).toUpperCase())
              console.log(eval(`this.${pivot}--`))
              console.log("Letra:" + pivot + " Val:" + eval(`this.${pivot}`))
              if(eval(`this.${pivot}`) == 0){
                  if(this.CountReg==2){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A:" + this.A + " B:" + this.B +")")
                  }
                  else if(this.CountReg==3){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A:" + this.A + " B:" + this.B + " C:" + this.C + ")")
                  }
                  else if(this.CountReg==4){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D +")")
                  }
                  else if(this.CountReg==5){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E +")")
                  }
                  else if(this.CountReg==6){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final"+ " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F +")")
                  }
                  else if(this.CountReg==7){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F + " G:" + this.G +")")
                  }
                //this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + "Final" + " A: " + this.A + " B: " + this.B + ")")
              } else{ //Handler se decrementar o pivot e ele ser 0 printa o último
              if(this.CountReg==2){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B +")")
              }
              else if(this.CountReg==3){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C + ")")
              }
              else if(this.CountReg==4){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D +")")
              }
              else if(this.CountReg==5){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E +")")
              }
              else if(this.CountReg==6){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F +")")
              }
              else if(this.CountReg==7){
                this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (x+1) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F + " G:" + this.G +")")
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
              if(eval(`this.${pivot}`) == 0){
                  if(arr[x].includes("vá_para")){
                  if(this.CountReg==2){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B +")")
                  }
                  else if(this.CountReg==3){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C + ")")
                  }
                  else if(this.CountReg==4){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D +")")
                  }
                  else if(this.CountReg==5){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E +")")
                  }
                  else if(this.CountReg==6){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F +")")
                  }
                  else if(this.CountReg==7){
                    this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A:" + this.A + " B:" + this.B + " C:" + this.C +  " D:" + this.D + " E:" + this.E + " F:" + this.F + " G:" + this.G +")")
                  }
                  //this.logVal[this.index++] = ("\n" + "(" +this.index + ", ID:" + (arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) + " A: " + eval(`this.${pivot}`) + " " + pivot + " " + eval(`this.${pivot}`)+ ")")

                  }
              }else{
                  if(arr[x].includes("vá_para")){
                    //console.log(arr[x].substring(arr[x].indexOf("vá_para")+7,arr[x].indexOf("senão")-1)) //Final
                    //console.log(arr[x].split(' ').pop().trim().substr(- 1)) // Segundo vai_para após o senão
                    x=(arr[x].split(' ').pop().trim().substr(- 1)-1)
                  }
              }
             
          }//extrai qual valor é testado
        } //fim if
        }while(this.B!=0)
    },
    func1(){ //Funções da demo
      do{
        this.func2();
      }while(this.B>0);
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "9," + " " + "A:" + this.A + " " + "B:" + this.B + ")");
    },
    func4(){
      this.B--;
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "4," + " " + "A:" + this.A + " " + "B:" + this.B + ")")
    },
    func3(){
      this.A++;
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "3," + " " + "A:" + this.A + " " + "B:" + this.B + ")")
      this.func4();
    },
    func2(){
      this.A++;
      this.logVal[this.index++] = ("\n" +this.index + ": " + "(" + "2," + " " + "A:" + this.A + " " + "B:" + this.B + ")")
      this.func3();
    },
  },
}


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