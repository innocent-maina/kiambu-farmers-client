<script setup lang="ts"></script>

<template>
  <v-app-bar elevation="5" style="background-color: #ecf2ff">
    <div class="d-flex align-center justify-space-between w-100 px-5">
      <div style="min-width: 10rem">
        <div class="pa-5">
          <GuestNavigationLogo />
        </div>
      </div>
      <div>
        <v-btn variant="flat" to="/login" style="background: #5d87ff; color: white">
          Login
        </v-btn>
      </div>
    </div>
  </v-app-bar>
</template>
