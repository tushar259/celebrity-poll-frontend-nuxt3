<template>
  <div>
      <nav class="navbar navbar-expand-lg navbar-light" style="width: 100% !important">
          <img class="navbar-brand navbar-logo-custom" :src="apiUrl+'/logo/favicon2.png'" alt="logo" @click="gotoHome()">
          
          <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation" @click="collapse = false">
              <span class="navbar-toggler-icon"></span>
          </button>
          <div class="navbar-collapse collapse" :class="{'hide-navbar-now': collapse}" id="navbarNav">
              <ul class="navbar-nav">
                  <li class="nav-item active">
                      <a class="nav-link custom-cursor" href="/" :class="{ 'active': activeLink === 'home' }" @click="gotoHome()">Home</a>
                  </li>
                  
                  <li class="nav-item dropdown">
                      <a class="nav-link dropdown-toggle text-truncate" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                          Industry
                      </a>
                      <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                          <a class="dropdown-item capitalized custom-cursor" v-for="(industryName, index) in allIndustry" :key="index" :class="{ 'active': activeLink === industryName.which_industry }" :href="'/industry/'+industryName.which_industry" @click="transferIndustryTo(industryName.which_industry)">{{industryName.which_industry}}</a>
                          
                      </div>
                  </li>
                  <li class="nav-item active">
                      <a class="nav-link custom-cursor" href="/poll-history" :class="{ 'active': activeLink === 'pollHistory' }" @click="gotoPollHistory()">Poll History</a>
                  </li>
                  <li class="nav-item active">
                      <a class="nav-link custom-cursor" href="/countries" :class="{ 'active': activeLink === 'countries' }" @click="gotoCountries()">Countries</a>
                  </li>
              </ul>
              <hr class="my-1">
              <ul class="navbar-nav ml-auto" :class="{'d-none': foundLoggedinUser}"> <!-- d-none -->
                  <li class="nav-item">
                      <a class="nav-link custom-cursor" :class="{ 'active': activeLink === 'login' }" href="" @click="loginClicked()">Login</a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link custom-cursor" :class="{ 'active': activeLink === 'registration' }" href="" @click="registrationClicked()">Registration</a>
                  </li>
              </ul>
              <ul class="navbar-nav ml-auto" :class="{'d-none': !foundLoggedinUser}">
                  <li class="nav-item dropdown">
                      <a class="nav-link dropdown-toggle text-truncate" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" :title="userEmail">
                          {{userEmail}}
                      </a>
                      <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                          <a class="dropdown-item custom-cursor" href="/change-password" :class="{ 'active': activeLink === 'changePassword' }" @click="gotoChangePassword()">Change password</a>
                          
                          <div class="dropdown-divider"></div>
                          <a href="" class="dropdown-item custom-cursor" @click="logoutClicked()">Logout</a>
                      </div>
                  </li>
              </ul>
          </div>
      </nav>

      <div class="content">
          <!-- <Nuxt/> -->
      </div>
      <div>
          <!-- Sticky footer -->
          <footer class="footer">
              <div class="container text-white">
                  <div class="row">
                      <div class="col-md-7">
                          <h5>Contact Information</h5>
                          <ul>
                              <li>Email: <a href="mailto:contact@example.com">contact@example.com</a></li>
                              <li>Phone: <a href="tel:+11234567890">+1 123-456-7890</a></li>
                          </ul>
                      </div>
                      <div class="col-md-5">
                          <h5>Connect with Us</h5>
                          <ul>
                              <li><a href="https://www.facebook.com/YourWebsiteName">Facebook</a></li>
                              <li><a href="https://twitter.com/YourWebsiteName">Twitter</a></li>
                              <li><a href="https://www.instagram.com/YourWebsiteName">Instagram</a></li>
                          </ul>
                      </div>
                  </div>
                  <div class="row">
                      <div class="col-md-12">
                          <p>
                              Disclaimer: [Your Website Name] is an independent platform and is not affiliated with any particular star or celebrity.
                              All content including text displayed on [Your Website Name] except images, is protected by copyright laws.
                          </p>
                          <p>
                              <a href="/privacy-policy">Privacy Policy</a> | <a href="/terms-and-conditions">Terms and Conditions</a> | <!-- <a href="/copyright">Copyright Notice</a> |--> <a href="/about-us">About us</a> | <a href="/report-problem">Report a Problem</a>
                          </p>
                          <p>&copy; [Year] [Your Website Name]. All rights reserved.

                          </p>
                      </div>
                  </div>
              </div>
          </footer>
      </div>
  </div>
</template>

<script>
// import axios from 'axios';

export default {
  

  data: () => ({
      apiUrl: null,
      collapse: false,
      foundLoggedinUser: false,
      userEmail: '',
      token: process.client ? localStorage.getItem('token') : '',
      currentPage: '',
      activeLink: '',
      allIndustry: [],
      currentLocation: process.client ? window.location.pathname : '',

  }),


  


    async setup() {
        const allIndustry = ref([]);
        const apiUrl = process.env.API_URL;
        
        try{
            const response = await useFetch(apiUrl+`/api/get-list-of-industries`)
            
            if(response.data.value.success == true){
                allIndustry.value = response.data.value.all_industry;
            }
            else{
                allIndustry.value = [];
            }
        }
        catch(error){
            allIndustry.value = [];
        }
        // console.log("response: ",response.data.value.success);
        return {
            allIndustry,
        };
    },
  

  created(){

      // if(process.client){
      //     this.token = localStorage.getItem('token');
      //     // this.currentLocation = window.location.pathname;
      // }
      
      // console.log("window.location.href: "+window.location.href); // current URL
      // console.log("window.location.pathname: "+window.location.pathname); // path of the current URL
      // console.log("window.location.hash: "+window.location.hash); // anchor part of the current URL
      // console.log("window.location.search: "+window.location.search); // query string part of the current URL
      // this.$nuxt.$on('class-changed', ($event) => this.updateSpecificDivClass($event))
      this.checkIfUserLoggedin();
    //   console.log("Public API URL: "+this.$config.public.API_URL);
    //   console.log("Private API URL: "+this.$config.private_API_URL);
      // this.getListOfIndustries();
      
  },

  mounted(){
      this.getCurrentWindowLocation();
  },

  methods:{
      gotoHome(){
          event.preventDefault();
          this.collapse = true;
          this.activeLink = 'home';
          this.$router.push('/');
          // console.log("this.activeLink: "+this.activeLink);
      },
      
      getCurrentWindowLocation(){
          
          const parts = this.currentLocation.split('/');
          
          if((parts[1] === "" || parts[1] === "polls") && !parts[2]){
              this.activeLink = 'home';
          }
          if(parts[1] === "industry" && parts[2] && parts[2].length > 0){
              this.activeLink = parts[2];
          }
          else if(parts[1] === "countries"){
              this.activeLink = 'countries';
          }
          else if(parts[1] === "change-password"){
              this.activeLink = 'changePassword';
          }
          else if(parts[1] === "login"){
              this.activeLink = 'login';
          }
          else if(parts[1] === "create-account"){
              this.activeLink = 'registration';
          }
          else if(parts[1] === "poll-history"){
              this.activeLink = 'pollHistory';
          }

          // console.log("this.activeLink: "+this.activeLink);
          // else{
          //     this.activeLink = 'home';
          // }
      },

      gotoCountries(){
          event.preventDefault();
          this.collapse = true
          this.activeLink = 'countries';
          this.$router.push('/countries');
      },

      gotoChangePassword(){
          event.preventDefault();
          this.collapse = true;
          this.activeLink = 'changePassword';
          this.$router.push('/change-password');
      },

      gotoPollHistory(){
          event.preventDefault();
          this.collapse = true;
          this.activeLink = 'pollHistory';
          this.$router.push('/poll-history');
      },
      transferIndustryTo(industry){
          event.preventDefault();
          this.collapse = true;
          this.activeLink = industry;
          this.$router.push(`/industry/${industry}`);
          // '/polls/'+industyName
      },
      getListOfIndustries(){
          this.$axios.get('/api/get-list-of-industries')
          .then(response =>{
              // console.log(response);
              if(response.data.success === true){
                  response.data.all_industry.forEach(item =>{
                      this.allIndustry.push(item);
                  })
              }

          })
          .catch(error =>{
              console.log(error); 
          })
      },
      
      setActiveLink(link) {
          this.collapse = true;
          this.activeLink = link;
      },

      updateSpecificDivClass(usersEmailFromComponent){
          // console.log("ifUser: ",usersEmailFromComponent);
          if(usersEmailFromComponent !== ""){
              this.userEmail = usersEmailFromComponent;
              this.foundLoggedinUser = true;
          }
          else{
              this.userEmail = null;
              this.foundLoggedinUser = false;
          }
      },

      checkIfUserLoggedin(){
          if(process.client){
              const formData = new FormData();
              formData.append("token", this.token);
              if(localStorage.getItem('token')){
                  this.$axios.post('/api/auth/check-if-user-logged-in', {
                      // other data you want to send
                  }, {
                      headers: {
                          'Authorization': `Bearer ${this.token}`
                      }
                  })
                  .then(response =>{
                      // console.log(response.data);
                      if(response.data.success === true && response.data.message === "User logged in"){
                          this.foundLoggedinUser = true;
                          this.userEmail = response.data.userInfoFromTk.email;
                          // this.userId = response.data.userInfoFromTk.id;
                      }
                      else{
                          this.foundLoggedinUser = false;
                      }
                  })
                  .catch(error => {
                      console.log(error);
                      this.foundLoggedinUser = false;
                  });
              }
              // else{
              //     //no token means no user logged in
              //     console.log("no token in storage");
              // }
          }
      },

      logoutClicked(){
          event.preventDefault();
          if(process.client){
              this.collapse = true;
              // localStorage.setItem('logout', 'clicked');
              this.currentPage = window.location.pathname;
              // console.log("what is currentpage: "+this.currentPage);
              localStorage.removeItem('token');
              this.userEmail = null;
              this.foundLoggedinUser = false;
              if(this.currentPage === "/login" || this.currentPage === "/create-account" || this.currentPage === "/change-password"){
                  this.currentPage = "/";
                  this.activeLink = 'home';
              }
              this.$router.push(this.currentPage);
              const parts = this.currentPage.split('/');
              if((parts[1] === "" || parts[1] === "polls") && !parts[2]){
                  this.activeLink = 'home';
              }
              else if(parts[1] === "polls" && parts[2] && parts[2].length > 0){
                  this.activeLink = parts[2];
              }
              else if(parts[1] === "countries"){
                  this.activeLink = 'countries';
              }
              else if(parts[1] === "change-password"){
                  this.activeLink = 'changePassword';
              }
              else if(parts[1] === "login"){
                  this.activeLink = 'login';
              }
              else if(parts[1] === "create-account"){
                  this.activeLink = 'registration';
              }
              else if(parts[1] === "poll-history"){
                  this.activeLink = 'pollHistory';
              }
          }
      },

      loginClicked(){
          event.preventDefault();
          if(process.client){
              this.collapse = true;
              this.activeLink = 'login';
              this.currentPage = window.location.pathname;
              if(this.currentPage === "/login" || this.currentPage === "/create-account" || this.currentPage === "/change-password"){
                  this.currentPage = "/";
              }
              localStorage.setItem('load-page', this.currentPage);
              this.$router.push("/login");
          }
      },

      registrationClicked(){
          event.preventDefault();
          if(process.client){
              this.collapse = true;
              this.activeLink = 'registration';
              this.currentPage = window.location.pathname;
              if(this.currentPage === "/login" || this.currentPage === "/create-account" || this.currentPage === "/change-password"){
                  this.currentPage = "/";
              }
              localStorage.setItem('load-page', this.currentPage);
              this.$router.push("/create-account");
          }
      }
  }
}
</script>
