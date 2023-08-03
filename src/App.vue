<template>
  <h2>가위바위보 게임</h2>
  <h4>연승기록 : {{longwin}} </h4>

  <div class="My">
    <img v-if="Myimg == true" src="./assets/question.png" alt="" class="MyPic">
    <img v-else-if=" Myimg == false" :src="require('@/assets/' + mychoice + '.png')" alt="" class="MyPic">
    <br />
    <strong>You</strong>
    <br />
    <img src="./assets/fullheart.png" alt="" class="life"
      v-for="(a,i) in Mylife" :key="i">
    <img src="./assets/heart.png" alt="" class="life"
      v-for="(a,i) in 3-Mylife" :key="i">  
       <br>
      
      <div class="choiceRCP">
        <label @click="Myimg=false" v-for="(a,i) in selects" :key="i">
          <input type="radio" name="choice" :id="selects[i].valuue" v-model="mychoice" :value="selects[i].value"> {{selects[i].name}}
        </label>

      </div>
      
    <div v-if="wait == 1" class="ready" @click="ready"><p>선택 완료!</p></div>
    <div v-if="wait == 2"  class="wait"><p>기다려주세요</p></div>
  </div>

  <div class="count">
    <strong>{{count}}</strong>
  </div>

  <div class="Com">
     <img v-if="Comimg == true" src="./assets/question.png" alt="" class="MyPic">
    <img v-else-if=" Comimg == false" :src="require('@/assets/' + comchoice + '.png')" alt="" class="MyPic">
    <br />
    <strong>Com</strong>
    <br />
    <img src="./assets/fullheart.png" alt="" class="life"
      v-for="(a,i) in Comlife" :key="i"
    >
    <img src="./assets/heart.png" alt="" class="life"
      v-for="(a,i) in 3-Comlife" :key="i">  
  </div>

  <div class="logdiv">
    <ul>
      <li class="log" :class="[logs[i].result == 'win'? 'win-log' : logs[i].result == 'lose'? 'lose-log' : 'same-log' ] "  
      v-for="(a,i) in logs" :key="i"><p>{{logs[i].message}}</p></li>
    </ul>
  </div>
    
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      count: 3,
      Mylife : 3,
      Comlife : 3,
      mychoice : 'question',
      comchoice : 'question',
      wait : 1,
      Myimg : true,
      Comimg : true,
      nowIm : ' ',
      logs : [],
      result : '',
      longwin : 0,
      selects : [
        {name : '가위', value : 'scissor'},
        {name : '바위', value : 'rock'},
        {name : '보', value : 'paper'}
      ]
    };
  },
  components: {},
  methods: {
    ready() {
      if(this.mychoice === 'question') {
        alert('가위,바위,보중 하나를 선택하세요');
      }else if(this.mychoice !== 'question') {
        this.wait = 2;
        this.number();
        
        let countDown = setInterval(() => {
          this.count--;
          this.Comimg = false;
          
          if(this.count === 0) {
            clearInterval(countDown);
            this.wait = 1;
            this.count = 3;
            this.now();
          }
        },1000)
      }
    },
    number() {
      let number = Math.random();
      setTimeout(() => {
        if(number < 0.33) {
        this.comchoice = 'rock'
      }else if (number < 0.66) {
        this.comchoice = 'scissor'
      } else {
        this.comchoice = 'paper'
      }
      }, 3000);
    },
    now() {
      if(this.mychoice === 'rock' && this.comchoice === 'rock') this.nowIm = 'same'
      else if(this.mychoice === 'rock' && this.comchoice === 'scissor') this.nowIm = 'win'
      else if(this.mychoice === 'rock' && this.comchoice === 'paper') this.nowIm = 'lose'
      else if(this.mychoice === 'scissor' && this.comchoice === 'rock') this.nowIm = 'lose'
      else if(this.mychoice === 'scissor' && this.comchoice === 'scissor') this.nowIm = 'same'
      else if(this.mychoice === 'scissor' && this.comchoice === 'paper') this.nowIm = 'win'
      else if(this.mychoice === 'paper' && this.comchoice === 'rock') this.nowIm = 'win'
      else if(this.mychoice === 'paper' && this.comchoice === 'scissor') this.nowIm = 'lose'
      else if(this.mychoice === 'paper' && this.comchoice === 'paper') this.nowIm = 'same'

    if(this.nowIm === 'win') {
      this.Comlife --;
    }else if(this.nowIm === 'lose') {
      this.Mylife --;
    }
    let log = {
      message : `You :  ${this.mychoice}, Computer : ${this.comchoice}`,
      result : this.nowIm 
    }
      this.logs.unshift(log);

      if(this.Mylife === 0) {
        alert('패배');
        this.logs = []
        this.Mylife = 3
        this.Comlife = 3
        this.longwin = 0
        this.mychoice = 'question'
        this.comchoice = 'question'
      }else if(this.Comlife === 0 ) {
        alert('승리');
        this.logs = []
        this.Mylife = 3
        this.Comlife = 3
        this.longwin++;
        this.mychoice = 'question'
        this.comchoice = 'question'
      }
    },
    
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
body {
  margin: 0px;
}
h2 {
  margin-bottom: 30px;
}
.My {
  width: 30%;
  height: 500px;
  border : 1px solid black;
  display: inline-block;
  position: relative;
  right: 100px;
}
.My strong {
  font-size: 50px;
}
.MyPic {
  margin: 30px 0px 10px;
  width: 50%;
  height: 40%;
}
.life {
  margin: 20px 0px 5px;
  width: 15%;
  height: 15%;
}
.Com {
  width: 30%;
  height: 500px;
  border : 1px solid black;
  display: inline-block;
  position: relative;
  left: 100px;
  bottom: 70px;
}
.Com strong {
  font-size: 50px;
}
.count {
  display: inline-block;
  font-size: 80px;
  position: relative;
  bottom: 160px;
}
.ready {
  border: 1px solid rgb(155, 183, 238);
  display: inline-block;
  width: 130px;
  height: 35px;
  background: rgb(155, 183, 238);
}
.ready:hover {
  cursor: pointer;
  border: 1px solid blue;
}
.ready p {
  color: blue;
  margin-top: 6px;
  display: inline-block;
}
.wait {
  border: 1px solid silver;
  display: inline-block;
  width: 130px;
  height: 35px;
  background: silver;
}
.wait p {
  color: black;
  margin-top: 6px;
  display: inline-block;
}
.choiceRCP {
  margin-bottom: 20px;
  font-size: 17px;
}
.choiceRCP input {
  margin: 0px 10px 0px 10px;
}
.log {
  border: 1px solid silver;
  width: 80%;
  height: 35px;
  margin-left: 8.5%;
  margin-bottom: 10px;
  list-style: none;
}
.log p {
  margin-top: 8px;
}
.logdiv {
  position: relative;
  bottom: 55px;
}
.win-log{
  color: green;
  background: #96edc6;
}
.lose-log{
  color: red;
  background: #ffc0c1;
}
.same-log {
  backface-visibility: #f7f7f7;
}
</style>
