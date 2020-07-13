<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">
        <font-awesome-icon :icon="['fas', 'briefcase']" color="white"></font-awesome-icon>
      </router-link>
      <router-link to="/list"><font-awesome-icon :icon="['fas', 'clipboard-list']" color="white"></font-awesome-icon></router-link>
      <router-link to="/settings"><font-awesome-icon :icon="['fas', 'cog']" color="white"></font-awesome-icon></router-link>
    </div>
    <keep-alive>
      <router-view />
    </keep-alive>
  </div>
</template>
