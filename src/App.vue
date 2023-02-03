<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { OAuthProvider, signInWithPopup } from "firebase/auth";
import {auth } from "../src/firebase/init";
import Sidebar from "../src/components/Sidebar.vue"


import {ref} from 'vue';

//Handle the sign-in flow with the Firebase SDK
//1. Create an instance of an OAuthProvider using the provider ID you got in the Firebase console
const provider = new OAuthProvider('oidc.oauthtest');

// 2. Authenticate with Firebase using the OAuth provider object.

const handleLogIn = () => {
  console.log("sucess");
  signInWithPopup(auth, provider)
      .then((result) => {
        //setUser(result.user)
        //const user = result.user
        // User is signed in.
        // IdP data available using getAdditionalUserInfo(result)

      // Get the OAuth access token and ID Token
      const credential = OAuthProvider.credentialFromResult(result);
      const accessToken = credential.accessToken;
      const idToken = credential.idToken;
      console.log("credential", credential, "accessToken", accessToken, "idToken", idToken);
    })
    .catch((error) => {
      // Handle error.
      const errorCode = error.code;
      const errorMessage = error.message;
      // the email of the user's account used.
      const email = error.customData.email;
      window.alert(error)
      console.log("error code", errorCode, "error message", errorMessage, "email", email );
      console.error(`Error loggin in: ${error.message}`);
    });
};

</script>

<template>
  <div class="flex w-full h-full items-center justify-center relative">
    <div class="flex w-full h-full justify-center flex-col p-2">
      <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

      <div class="sm:flex-row flex flex-col w-full items-center justify-center">
        <button @click="handleLogIn" class="flex w-25 h-11 items-center justify-center border border-gray-300 rounded-lg bg-gray-50 hover:bg-gray-100">
          LogIn
        </button>
      </div>
    
    </div>

    <Sidebar/>
  </div>
  
</template>

<style scoped>

.logo {
  display: block;
  margin: 0 auto 2rem;
}

button {
  width: 100px;
  height: 40px;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

/* @media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
} */
</style>
