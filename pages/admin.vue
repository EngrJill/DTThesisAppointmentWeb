<template>
  <div class="main-container-admin">
    <div class="landing-subcontainer">
        <label for="user" >Username</label>
        <input type="text" v-model="user">
        <label for="password">Password</label>
        <input type="password" v-model="pw">
        <div class="newClass" v-if="checkUser">
            <nuxt-link :to="{ name: 'admindash', params:{ access: this.access } }">
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
    data() {
        return {
            user: '',
            pw: '',
            access: null
        }
    },
    computed: {
        checkUser: function() {
            if (this.user == "admin" && this.pw == "secretlover") {
                this.access = true
                return true
            }
            else {
                this.access = false
                return false
            }
        }
    }
}
</script>

<style lang="scss" scoped>
    .main-container-admin {
        background-color:#3598DC;
        height: 100vh;
        width: 100%;
        font-family: Arial, Helvetica, sans-serif;

        .landing-subcontainer{
            width: 280px;
            height: 35%;
            background-color: white;
            padding: 30px 20px;
            position: absolute;
            margin-left: auto;
            margin-right: auto;
            margin-top: 170px;
            left: 0;
            right: 0;
            text-align: left;

            label {
                font-weight: bold;
            }

            .newClass {
                button {
                    cursor: pointer;
                    background-color: #3598DC;
                    color: white;
                    min-height: 5vh;
                    width: 100%;
                    margin-bottom: 15px;
                }
            }
        }

        input {
            border: solid 1px gray;
            margin-bottom: 17px;
            background-color: #F4F1F1;
            height: 30px;
            width: min(800px, 100%);
            margin-top: 6px;
            margin-bottom: 20px;
            padding-left: 10px;
        }
    }
</style>