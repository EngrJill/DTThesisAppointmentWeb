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
    padding-top: 100px;
    
    .landing-subcontainer {
      height: 80%;
      width: 60%;
      background-color: white;
      margin-left: auto;
      margin-right: auto;
      border-radius: 5px;
      

      .txt-container {
        padding-top: 90px;
        margin-left: auto;
        margin-right: auto;
        width: 65%;
        h1 {
          font-size: 4rem;
          text-align: center;
        }
        h5 {
          font-size: 1.3rem;
          font-weight: 300;
          padding-top: 10px;
          padding-left: 30px;
        }
      }

      .placeholder-class {
        padding-top: 120px;
        margin-left: auto;
        margin-right: auto;
        width: 65%;

        .checkbox {
          margin-left: 30px;
          margin-top: 15px;
        }

        p {
          color: #777777;
          padding-left: 30px;
        }
        .name {
          background-color: #F4F1F1;
          border-radius: 10px;
          height: 60px;
          width: 95%;
          margin-top: 10px;
          margin-left: 27px;
          padding-left: 14px;
        }
        button {
          background-color: $primary-color;
          color: white;
          height: 40px;
          width: 30%;
          border-radius: 10px;
          margin-top: 20px;
          margin-left: 33%;
        }
      }

    }

  }
</style>