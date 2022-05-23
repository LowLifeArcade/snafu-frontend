<template>
  <div class="game">
    <h1>The Game</h1>
    <input v-model="input" class="shadow border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" type="text"/>
  </div>
</template>

<script>
export default {
  data(){
    return {
      ws: null,
      input: null,
      user: null,
      gameData: []
    }
  },
  mounted() {
    if (!localStorage.getItem('user')) {
      this.user = Math.random().toString()
      console.log('user',this.user)
      localStorage.setItem('user', this.user)
    } else {
      this.user = localStorage.getItem('user')
    }
    console.log('user', this.user)
    this.ws = new WebSocket("ws://localhost:8080")
    this.ws.addEventListener("open", () => {
      console.log("We are connected");
      this.ws.send("How are you?");
    });

    this.ws.onmessage = function (event) {
      const eventData = JSON.parse(event.data)
      // this.gameData[eventData.user] = eventData.input
      console.log(JSON.parse(event.data));
      console.log('gameData', this.gameData)
    }
    // this.ws.addEventListener("message", function (event) {
    //   console.log(event.data);
    // });
    this.user = localStorage.getItem('user')
  },
  watch: {
    input() {
      this.ws.send(JSON.stringify({user: this.user, input: this.input}))
    }
  }
};
</script>
