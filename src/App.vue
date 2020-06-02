<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">開始吧!</router-link> |
      <router-link to="/list">工作清單</router-link> |
      <router-link to="/settings">鬧鈴設置</router-link>
    </div>
    <keep-alive>
      <router-view/>
    </keep-alive>
  </div>
</template>
