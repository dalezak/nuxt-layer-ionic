<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Reset Password</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" class="ion-padding">
      <ion-grid>
        <ion-row class="ion-justify-content-center">
          <ion-col size="12" size-sm="12" size-md="10" size-lg="8" size-xl="6">
            <ion-card class="ion-margin">
              <ion-card-header>
                <ion-card-title>Change password</ion-card-title>
                <ion-card-subtitle>Enter your new password</ion-card-subtitle>
              </ion-card-header>
              <ion-item lines="inset">
                <ion-label position="floating">Password</ion-label>
                <ion-input aria-label="Password" ref="passwordInput" v-model="password" type="password" required></ion-input>
              </ion-item>
              <ion-card-content>
                <ion-row>
                  <ion-col class="ion-no-padding ion-text-end">
                    <ion-button fill="solid" @click="doUpdate">Update Password</ion-button>
                  </ion-col>
                </ion-row>
              </ion-card-content>
            </ion-card>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup>
definePageMeta({
  middleware: 'auth'
})

let password = ref("");
let passwordInput = ref("");

const userStore = useUserStore();
const { updatePassword } = userStore;

function hasPassword() {
  return hasInput(passwordInput.value, password.value, "Please enter your password");
}

async function doUpdate() {
  if (hasPassword()) {
    try {
      await updatePassword({
        password: password.value
      });
      showAlert("Password Update", "Your password has been updated.");
    }
    catch (error) {
      consoleError("doUpdate", error);
      showAlert("Problem Updating Password", error);
    }
  }
}
</script>

<style scoped lang="scss">

</style>