<template class="white--text">
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <v-toolbar>
        <v-toolbar-side-icon>
          <v-icon @click="drawer= !drawer">more_vert</v-icon>
        </v-toolbar-side-icon>
        <a class="navbar-brand ml-4" href="#" style="color:#164e87">Notify</a>
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link" href="#" style="color:#164e87">
                Home
                <span class="sr-only">(current)</span>
              </a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#" style="color:#164e87">Features</a>
            </li>
            <li>
              <form class="form-inline">
                <input
                  class="nav-item form-control"
                  type="search"
                  placeholder="Search"
                  aria-label="Search"
                  size="50%"
                />
                <v-btn class="btn my-2 my-sm-0" type="submit" style="color:#164e87">Search</v-btn>
              </form>
            </li>

            <!-- <li class="nav-item dropdown">
            <a
            style="color:#164e87" 
              class="nav-link dropdown-toggle"
              href="#"
              id="navbarDropdown"
              role="button"
              data-target="categories"
              data-toggle="dropdown"
              aria-haspopup="true"
              aria-expanded="false"
            >Categories</a>
            <div class="dropdown-menu" id="categories" aria-labelledby="navbarDropdown">
              <a class="dropdown-item" href="#">Action</a>
              <a class="dropdown-item" href="#">Another action</a>
              <div class="dropdown-divider"></div>
              <a class="dropdown-item" href="#">Something else here</a>
            </div>
            </li>-->
          </ul>
          <v-avatar class="ml-auto" size="45">
            <img :src="`${this.$store.getters.userImage}`" />
          </v-avatar>
          <div class="nav-item dropdown mr-5">
            <a
              class="nav-link dropdown-toggle mr-5"
              href="#"
              id="navbarDropdown"
              role="button"
              data-target="account"
              data-toggle="dropdown"
              aria-haspopup="true"
              aria-expanded="false"
            >{{this.$store.getters.userName}}</a>
            <input type="file" ref="file" style="display: none" @change="file" />
            <div class="dropdown-menu" id="account" aria-labelledby="navbarDropdown">
              <a class="dropdown-item" @click="changepicture" href="#">Change Profile Picture</a>
              <a class="dropdown-item" href="#" @click="changepw">Change Password</a>
              <div class="dropdown-divider"></div>
              <v-btn class="dropdown-item" text @click="logout" href="#">
                <span>Sign Out</span>
                <span class="mdi mdi-logout-variant"></span>
              </v-btn>
            </div>
          </div>
        </div>
      </v-toolbar>
      <!-- <v-toolbar>
      <v-toolbar-title>
        <span>ayklam</span>

        <v-spacer></v-spacer>

        <v-btn flat color="red">
          <span>ayklam</span>
          <v-icon right>exit_to_app</v-icon>
        </v-btn>
      </v-toolbar-title>
      </v-toolbar>-->
      <v-navigation-drawer v-model="drawer" app color="#164e87">
        <v-list>
          <v-list-tile>
            <v-list-tile-action>
              <v-icon class="mr-4"></v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-toolbar-title class="white--text ml-4">All categories</v-toolbar-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
      </v-navigation-drawer>
    </nav>
    <div class="mx-auto">
      <v-carousel class="mx-auto mt-4" style="width:70%" height="800" cycle interval="3000">
        <v-carousel-item
          v-for="(item,i) in items"
          :key="i"
          :src="item.src"
          reverse-transition="fade-transition"
          transition="fade-transition"
        ></v-carousel-item>
      </v-carousel>
    </div>
    <!-- <div>
    <nav class="navbar navbar-light bg-dark">
  <a class="navbar-brand" href="#">
    <img src="" width="100" height="100" alt="">
  </a>
</nav>
    </div>-->
  </div>
</template>
<script>
import api from "../api";
export default {
  data() {
    return {
      drawer: false,
      items: [
        {
          src:
            "https://lh3.googleusercontent.com/proxy/H-K3hoNQBl3q0V65zt-oID5ZMwxwK2eNukxSd0tMD8wullCyyS9M1DdCDHgo3sJtwHtRUg0auBn5JbR31jk5X3ljPfu7NWTCeVW8FOUDbBxKSduiQ3hKBt_cj6rqmQ3tQ8UnEVpOhELl_jaOGbGfT0GwF9ce04Y"
        },
        {
          src:
            "https://s3-eu-west-1.amazonaws.com/wuzzuf/files/company_logo/Souq-com-Egypt-2782-1585140163-og.png"
        },
        {
          src:
            "https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/H%26M-Logo.svg/1280px-H%26M-Logo.svg.png"
        },
        
        {
          src:
            "https://upload.wikimedia.org/wikipedia/commons/5/53/H%26M-Logo.svg"
        },
        {
          src:
            "https://static.dezeen.com/uploads/2019/02/new-zara-logo-sq-1-1024x1024.jpg"
        }
      ]
    };
  },
  methods: {
    file(e) {
      this.$loading(true);
      let frm = new FormData();
      frm.append("profile_picture", e.target.files[0], e.target.files[0].name);
      frm.append("email", this.$store.getters.userEmail);
      api
        .changepicture(frm)
        .then(res => {
          this.$loading(false);
          this.$store.commit("assignpicture", res.data);
          this.$alert("Profile Picture has been changed", null, "success");
        })
        .catch(() => {});
    },
    logout() {
      this.$fire({
        title: "Are you sure you want to sign out?",
        showCancelButton: true,
        focusConfirm: false,
        confirmButtonText: "Sign Out",
        cancelButtonText: "Cancel"
      }).then(() => {
        api.deletetoken();
        this.$router.push("/");
      });
    },
    changepw() {
      this.$fire({
        title: "Enter the details below",
        showCancelButton: true,
        html:
          '<input type="password" id="oldpassword" class="swal2-input" placeholder="Old Password">' +
          '<input type="password" id="password" class="swal2-input" placeholder="Password">' +
          '<input type="password" id="confirmpassword" class="swal2-input" placeholder="Confirm Password">',
        focusConfirm: false,
        confirmButtonText: "Submit",
        cancelButtonText: "Cancel",
        preConfirm: () => {
          return [
            document.getElementById("password").value,
            document.getElementById("confirmpassword").value,
            document.getElementById("oldpassword").value
          ];
        }
      }).then(results => {
        const password = results.value[0];
        const confirmpassword = results.value[1];
        const oldpw = results.value[2];
        if (oldpw.length <= 0)
          return this.$alert(
            "Please enter your old password",
            "Error",
            "error"
          );
        if (password.length < 5)
          return this.$alert(
            "Password must be more than 5 characters",
            "Error",
            "error"
          );
        if (/^(?=.*?[A-Z])(?=.*?[a-z]).{5,}$/.test(password) == false)
          return this.$alert(
            "Password must have at least one upper case and one lower case letters ",
            "Error",
            "error"
          );
        if (password != confirmpassword)
          return this.$alert("Passwords don't match", "Error", "error");
        this.$loading(true);
        let user = {
          email: this.$store.getters.userEmail,
          password: password,
          oldpw: oldpw
        };
        api
          .userChangesPw(user)
          .then(res => {
            this.$loading(false);
            this.$alert(
              `${res.data}, Login to continue.`,
              "Success",
              "success"
            );
            api.deletetoken();
            this.$router.push("/");
          })
          .catch(err => {
            this.$loading(false);
            this.$alert(err.response.data, "Error", "error");
          });
      });
    },
    changepicture() {
      this.$refs.file.click();
    }
  }
};
</script>
<style>
div.relative {
  position: relative;
  bottom: 100%;
}
div.nav-link {
  color: #164e87;
}
</style>