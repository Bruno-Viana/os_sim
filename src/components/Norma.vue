<template>
  <div class="wrapperNorma">
    <div>
      <h1>Máquina Norma</h1>
      <form v-on:keypress="isNumber($event)">
        Quantidade de registradores: <input type="number" name="CountReg" v-model="CountReg" min="2" max="7"> <br>
        <p>Valor de entrada para A:<input type="text" name="A" v-model="A"></p>
        <p>Valor de entrada para B:<input type="text" name="B" v-model="B"></p>
        <p v-if="CountReg>=3">Valor de entrada para C:<input type="text" name="C" v-model="C"></p>
        <p v-if="CountReg>=4">Valor de entrada para D:<input type="text" name="D" v-model="D"></p>
        <p v-if="CountReg>=5">Valor de entrada para E:<input type="text" name="E" v-model="E"></p>
        <p v-if="CountReg>=6">Valor de entrada para F:<input type="text" name="F" v-model="F"></p>
        <p v-if="CountReg>=7">Valor de entrada para G:<input type="text" name="G" v-model="G"></p>
      
      <p v-if="isLtr" style="color:red; font-size:20">Apenas números não decimais serão aceitos.</p>
      </form>
      <p>Registros inseridos:</p>
      <p>A: {{A}}</p>
      <p>B: {{B}}</p>
      <p v-if="CountReg>=3">C: {{C}}</p>
      <p v-if="CountReg>=4">D: {{D}}</p>
      <p v-if="CountReg>=5">E: {{E}}</p>
      <p v-if="CountReg>=6">F: {{F}}</p>
      <p v-if="CountReg>=7">G: {{G}}</p>
    </div>
    <div class="descBlock">
      <p>INSTRUÇÕES</p><hr>
      <textarea style="text-align:left" v-model="valueInst" placeholder="id: condição/operação variável ex:           1: se zero_b então vá_para9 senão vá_para2"></textarea>
      <label for='uploadBtn'><i class="fas fa-upload"></i>Selecionar um arquivo</label>
      <input id="uploadBtn" type='file' v-on:change="openFile($props)" style="border:none">
      <button v-on:click='testInst()' style="background-color: #ffffff;color: rgb(0, 0, 0);cursor: pointer;padding: 5px 20px;border:none;margin-left:5px">Testa Instruções</button>
      <p>
        1: se zero_b então vá_para9 senão vá_para2 <br>
        2: faça add_a vá_para3 <br>
        3: faça add_a vá_para4 <br>
        4: faça sub_b vá_para1

      </p>
    </div><br>
    <button class="func1Btn" v-on:click='func1()'><i class="far fa-play-circle"></i> Começar</button>
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
          A:null,B:null,C:null,D:null,E:null,F:null,G:null,
          isLtr:null,
          valueInst:null,
          logVal:[],
          index:0,
          CountReg:2,
          Resultado:null,
      }
    },
  methods: {
    openFile(props){
    props = (props) ? props : window.event;
    var reader = new FileReader();
    reader.onload = function(){
      this.Resultado=JSON.stringify(reader.result.substring(0,500))
      //console.log(this.Resultado)
      console.log(reader.result)
      return JSON.stringify(reader.result.substring(0,500));
    };
    reader.readAsText(props.target.files[0]);
    //console.log('Res' + this.Resultado)
    //this.valueInst=Resultado.toString;
    //console.log(this.valueInst)
    },
    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57))) {
        this.isLtr=true;
        evt.preventDefault();
      } else {
        this.isLtr=false;
        return true;
      }
    },
    testInst(){
      console.log(this.valueInst.split("\n"))     
    },
    func1(){
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
  width: fit-content;
  text-align: center;
}
form>p>input, form>input{
  border:1px rgb(100, 59, 59) solid;
  width: 10%;
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