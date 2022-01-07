<template>
  <div class="main-container">
    <div class="landing-subcontainer">

      <div class="txt-container">
        <h1>Welcome to Advanced Appointment Systems</h1>
        <h5>Your contact-proof appointment gateway</h5>
      </div>

      <div class="placeholder-class">
          <p>
            Name
          </p>

          <input type="text" placeholder="What is your name?" class="name">

          <div class="checkbox">
            <input type="checkbox" name="TandC" id="TandC">
            <label>By proceeding with this Appointment System Form, 
              you agree with the terms and condition of the people behind this technology</label><br>
          </div>

          <nuxt-link to="/appointment">
            <button>
              PROCEED
            </button>
          </nuxt-link>
      </div>

    </div>
  </div>
</template>

<script>
export default {

}
</script>


<style lang="scss" scoped>
$primary-color: #3598DC;
   .main-container {
    font-family: Arial, Helvetica, sans-serif;
    height: 100vh;
    width: 100vw;
    background-color: $primary-color;
    padding: 100px 0px 0px;
    
    .landing-subcontainer {
      height: 80%;
      width: min(800px, 60%);
      background-color: white;
      margin-inline: auto;
      border-radius: 5px;
      

      .txt-container {
        padding-top: 50px;
        margin-inline: auto;
        width: min(500px, 65%);
        h1 {
          font-size: clamp(0.8rem, 3vw, 4rem);
          text-align: center;
        }
        h5 {
          font-size: clamp(0.7rem, 2vw, 1.3rem);
          font-weight: 300;
          padding-top: 10px;
          text-align: center;
        }
      }

      .placeholder-class {
        padding-top: 70px;
        margin-inline: auto;
        width: 65%;

        .checkbox {
          margin-left: 30px;
          margin-top: 15px;
          font-size: clamp(0.5rem, 0.10vw + 0.9vw, 0.96rem);
        }

        p {
          color: #777777;
          text-align: left;
          padding-left: 6%;
          font-size: clamp(0.5rem, 1vw, 0.9rem);
        }
        .name {
          background-color: #F4F1F1;
          border-radius: 10px;
          height: 8vh;
          width:min(500px, 95%);
          margin-top: 10px;
          margin-left: 5%;
          padding-left: 14px;
          font-size: clamp(0.5rem, 1vw, 0.9rem);
        }
        button {
          background-color: $primary-color;
          color: white;
          min-height: 6vh;
          width: min(30vw, 30%);
          border-radius: 10px;
          margin-top: 20px;
          margin-left: 37%;
          font-size: clamp(0.5rem, 1.5vw, 1rem);
        }
      }

    }

  }

</style>