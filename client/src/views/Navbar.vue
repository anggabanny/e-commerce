<template>
  <div>
  <b-navbar toggleable="sm" class="navbarStore" style="cursor:pointer;background-color:#FFFFFF">
  <router-link to="/">
    <div class="ml-5">
        <img src="../img/logo-store-png-5.png"
        class="ml-5"
        style="height:35px;"
        alt="icon">
    </div>
  </router-link>
      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
          <router-link to="/login">
            <button type="submit" v-if="!isLogin"  class="btn p-0">
                <i class="fas fa-user-circle fa-2x mr-3"></i>
            </button>
          </router-link>
          <button type="submit" v-if="isLogin" @click.prevent="logout" class="btn p-0" >
              <i class="fas fa-address-card fa-2x"></i>
          </button>
          <router-link to="/mycart">
            <button type="submit" v-if="isLogin" v-show="!mycart" :@click="mycart=false" class="btn p-0 ml-2">
              <i class="fas fa-cart-arrow-down fa-2x mr-3"></i>
            </button>
          </router-link>
          <router-link to="/addproduct">
            <button type="submit" v-if="isLogin" class="btn p-0">
              <i class="fas fa-plus-circle fa-2x mr-3"></i>
            </button>
          </router-link>
          <router-link to="/mycart/mylist">
            <button type="submit" v-if="isLogin" class="btn p-0">
              <i class="fas fa-bookmark fa-2x mr-3"></i>
            </button>
          </router-link>
          <!-- <button type="submit" class="btn p-0"> -->
            <!-- <i @click="$router.go(-1)" class="fas fa-arrow-circle-right fa-2x"></i> -->
          <!-- </button> -->
      </b-navbar-nav>
  </b-navbar>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
import { mapState } from 'vuex'
export default {
    data () {
        return {
          mycart: false
        }
    },
    methods: {
      logout(){
        Swal.fire({
          title: "Are you sure to logout ?",
          type: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes"
        })
        .then(result => {
            if(result.value){
                localStorage.removeItem('token')
                localStorage.removeItem('userId')
                this.$store.commit('SET_LOGIN')
                const Toast = Swal.mixin({
                  toast: true,
                  position: 'top-end',
                  showConfirmButton: false,
                  timer: 3000,
                  timerProgressBar: true,
                  onOpen: (toast) => {
                    toast.addEventListener('mouseenter', Swal.stopTimer)
                    toast.addEventListener('mouseleave', Swal.resumeTimer)
                  }
                })
                Toast.fire({
                  icon: 'success',
                  title: 'Logout in successfully'
                })
            }
            this.$router.push('/')
        })
        .catch(err=>{
            Swal.fire({
                icon: "error",
                title: "Oops...",
                text: "Signin Failed!"
            });
        })
      }
    },
    computed: mapState(["isLogin", "back"])
}

// this.$router.push("/cart");
</script>

<style>
.navbarStore{
    box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.082);
}
.fa-address-book{
    transition: 0.3s;
}
.fa-address-book:hover {
    color: #E46F32;
}
</style>
