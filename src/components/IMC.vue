<template>
  <div class="md-layout">
    <form novalidate id="imc" class="md-size-15" @submit.prevent="calcularIMC" v-if="!imc">
      <md-card-header>
        <div class="md-title">Cálculo de IMC</div>
      </md-card-header>
      <md-field>
        <label>Peso</label>
        <md-input v-model="peso" type="number"></md-input>
        <span class="md-suffix">kg</span>
      </md-field>
      <md-field>
        <label>Altura</label>
        <md-input v-model="altura" type="number"></md-input>
        <span class="md-suffix">cm</span>
        <span class="md-helper-text">Sem vírgulas, ex.: 170cm</span>
      </md-field>
      <div style="margin-top:10px">
        <md-radio v-model="genero" :value="generos[0]" class="md-primary">Masculino</md-radio>
        <md-radio v-model="genero" :value="generos[1]">Feminino</md-radio>
      </div>
      <div id="submit">
        <md-button type="submit" class="md-raised md-primary" id="submit-button">Calcular IMC</md-button>
      </div>
      <section id="info">
        * Este teste não deve substituir a consulta a um nutricionista.
      </section>
    </form>
    <section style="text-align:center" v-if="imc">
      <p>
        <md-icon class="md-size-2x">linear_scale</md-icon>
      </p>
      <section id="imc-display">
        <h3>Seu IMC</h3>
        <p>{{ imc }}</p>
      </section>
      <section class="imc-values" style="margin-bottom:15px" v-if="imc!=2">
        <md-chip class="md-accent">Necessita atenção!</md-chip>
      </section>
      <section class="imc-values">
        <h3>{{ imcValue[0] }}</h3>
        <p>{{ imcValue[1] }}</p>
      </section>
      <section class="imc-values" style="margin-top:15px">
        <p>Seu peso atual: {{ peso }}kg</p>
        <p>Sua altura: {{ altura }}cm</p>
      </section>
      <section class="imc-values" style="margin-top:15px">
        <h3>Seu peso ideal</h3>
        <p>Entre {{ parseInt(pesoIdeal)-10 }}kg e {{ parseInt(pesoIdeal)+10 }}kg</p>
        <p style="font-size:12px;">* Estes valores podem variar de pessoa para pessoa.</p>
      </section>
      <p>
        <md-button type="button" class="md-raised md-accent" id="submit-button" @click="limparValores">Limpar</md-button>
      </p>
      <p>
        <md-icon class="md-size-2x">linear_scale</md-icon>
      </p>
    </section>
  </div>
</template>

<style scoped>
#submit {
  width: 100%;
  margin: 4px 0 24px;
  padding-top: 16px;
  display: flex;
  position: relative;
  font-family: inherit;
}
#submit-button {
  display: block;
  margin: 25px auto;
}
#imc-display {
  padding: 15px 0;
}
#imc-display h3 {
  margin: 0;
  text-transform: uppercase;
  font-size: 30px;
  font-weight: 100;
}
#imc-display p {
  margin: 20px 0;
  font-size: 60px;
}
.imc-values h3, .imc-values p {
  margin: 0;
}
#info {
  margin: 15px 0;
  text-align: center;
}
</style>

<script>
export default {

  name: 'IMC',

  data: () => {
    return {
      genero: null,
      generos: [
        { title:'Masculino', value:0.90 },
        { title:'Feminino',  value:0.85 }
      ],
      peso      : null,
      pesoIdeal : null,
      altura    : null,
      imc       : 0.0,
      imcValue  : null,
      imcIndex  : null,
      imcValues : [
        // Menos do que 17 -> Muito abaixo do peso
        ['Menos do que 17', 'Muito abaixo do peso'],
        // Entre 17 e 18,49 -> Abaixo do peso
        ['Entre 17 e 18,49', 'Abaixo do peso'],
        // Entre 18,5 e 24,9 -> Peso normal
        ['Entre 18,5 e 24,9', 'Peso normal'],
        // Entre 25 e 29,9 -> Sobrepeso
        ['Entre 25 e 29,9',   'Sobrepeso'],
        // Entre 30 e 34,9 -> Obesidade grau 1
        ['Entre 30 e 34,9',   'Obesidade grau 1'],
        // Entre 35 e 39,9 -> Obesidade grau 2
        ['Entre 35 e 39,9',   'Obesidade grau 2'],
        // Mais do que 40 -> Obesidade grau 3
        ['Mais do que 40',    'Obesidade grau 3 (mórbida)'],
      ]
    }
  },

  created() {
    this.genero    = localStorage['genero'] || this.generos[0];
    this.peso      = localStorage['peso'] || null;
    this.pesoIdeal = localStorage['pesoIdeal'] || null;
    this.altura    = localStorage['altura'] || null;
    this.imc       = localStorage['imc'] || null;
    this.imcIndex  = localStorage['imcIndex'] || null;
    this.imcValue  = this.imcIndex? this.imcValues[this.imcIndex]: null;
  },

  methods: {
    calcularIMC() {
      if(!this.peso || !this.altura) return;
      //
      this.sending = true;
      if(this.altura[1]==',' || this.altura[1]=='.') {
        this.altura = this.altura.substr(0,1) + this.altura.substr(2);
      }
      this.pesoIdeal = (this.altura-100) * this.genero.value;
      localStorage['pesoIdeal'] = this.pesoIdeal;
      localStorage['peso'] = this.peso;
      localStorage['altura'] = this.altura;
      //
      // IMC = Peso ÷ (Altura × Altura)
      this.imc = (this.peso / (this.altura * this.altura)) * 10000;
      this.imc = this.imc.toFixed(1);
      // Menos do que 17
      if(this.imc < 17) this.imcIndex = 0;
      // Entre 17 e 18,49
      if(this.imc >= 17 && this.imc <= 19.49) this.imcIndex = 1;
      // Entre 18,5 e 24,9
      if(this.imc >= 18.5 && this.imc <= 24.9) this.imcIndex = 2;
      // Entre 25 e 29,9
      if(this.imc >= 25 && this.imc <= 29.9) this.imcIndex = 3;
      // Entre 30 e 34,9
      if(this.imc >= 30 && this.imc <= 34.9) this.imcIndex = 4;
      // Entre 35 e 39,9
      if(this.imc >= 35 && this.imc <= 39.9) this.imcIndex = 5;
      // Mais do que 40
      if(this.imc >= 40) this.imcIndex = 6;
      this.imcValue = this.imcValues[this.imcIndex];
      localStorage['imc'] = this.imc;
      localStorage['imcIndex'] = this.imcIndex;
    },

    limparValores() {
      this.peso     = null;
      this.pesoIdeal = null;
      this.altura   = null;
      this.genero   = this.generos[0];
      this.imc      = null;
      this.imcValue = null;
      //
      localStorage.removeItem('peso');
      localStorage.removeItem('pesoIdeal');
      localStorage.removeItem('altura');
      localStorage.removeItem('genero');
      localStorage.removeItem('imc');
      localStorage.removeItem('imcIndex');
    }

  }
}
</script>
