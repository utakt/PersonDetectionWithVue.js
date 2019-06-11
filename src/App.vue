<template>
  <div id="app">
    <div>
      <b-jumbotron header="物体検知アプリ" lead="using TensorFlow Object Detection API">
        <p>画像を選択して、判定ボタンをクリック！</p>
      </b-jumbotron>
    </div>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<style>
</style>
