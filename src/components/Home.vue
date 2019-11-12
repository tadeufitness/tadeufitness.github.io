<template>
  <div>
    <div id="center">
      <section id="top">
        <img src="/static/image/logo-1.png" alt="logo tadeufitness" id="top-image" />
        <h2>Bem vindo!</h2>
        <h3>
          tadeu.fitness
          <small>(beta)</small>
        </h3>
      </section>
      <section id="links">
        <md-button class="md-raised md-accent" id="imc" to="/imc">
          Calcule o seu IMC
        </md-button>
      </section>
      <section id="about">
        <h4>Sobre este app</h4>
        <p>Esta é uma versão <strong>beta</strong>, portanto algumas funcionalidades podem não funcionar da forma esperada, e/ou possuir poucas funcionalidades.</p>
        <p>Em breve teremos novidades por aqui!</p>
        <p id="follow">
          <md-button class="md-raised md-primary" href="https://instagram.com/tadeu.fitness" target="_blank">
            Me acompanhe no Instagram.
          </md-button>
        </p>
      </section>
    </div>
  </div>
</template>

<style scoped>
#center, #center * {
  text-align: center;
  margin: 0;
}
#top-image {
  display: block;
  width: 200px;
  margin: 15px auto;
}
#top h2 {
  text-transform: uppercase;
  font-size: 25px;
}
#top h3 {
  margin-top: 0;
  font-weight: 100;
}
#top h3 small {
  font-weight: bold;
  color: #492bec;
}
#links {
  margin: 30px 0;
}
#about {
  padding-top: 15px;
  margin: 15px 0 30px 0;
  border-top: 1px solid #eee;
}
#about h4 {
  text-transform: uppercase;
}
#follow {
  margin-top: 30px;
}
</style>

<script>
export default {
  name: 'Home'
}
</script>
