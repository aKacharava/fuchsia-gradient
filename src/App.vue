<template>
  <div id="app" :style="{ background: gradient }">
    <div class="interface-container">
      <transition-group name="flip-list" tag="ul">
        <li v-for="(color,index) in colors" :key="color.id" :class="{ shake : color.last }">
          <div class="moveInput">
            <button class="up" @click="up(index)" v-if="index > 0"></button>
            <button class="down" @click="down(index)" v-if="index < colors.length - 1"></button>
          </div>
          <input v-model.trim="color.hex" maxlength="7" type="text" :class="{ lockedInput : color.disabled }" :style="{ color: color.hex }" :disabled="color.disabled" 
          autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false" placeholder="Type Hexcode">
          <button @click="lockInput(index)" :class="{locked : color.disabled}"></button>
        </li>
      </transition-group>
      <div class="button-group">
        <div>
          <button @click="addInputColor" id="add"></button>
          <transition name="fade">
            <button @click="removeInputColor" id="remove" v-if="colors.length > 2"></button>
          </transition>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return{
      colors:[
        { id: 0, hex: '#002A77', disabled: false },
        { id: 1, hex: '#EE82EE', disabled: false }
      ],
      id: 2
    }
  },
  methods:{
    up(colorIndex){
      if(colorIndex > 0){
        let temp = this.colors[colorIndex];
        let temp2 = this.colors[colorIndex - 1];
        this.colors[colorIndex - 1] = temp;
        this.colors[colorIndex] = temp2;
      }
    },
    down(colorIndex){
      if(colorIndex < this.colors.length - 1){
        let temp = this.colors[colorIndex];
        let temp2 = this.colors[colorIndex + 1]
        this.colors[colorIndex + 1] = temp;
        this.colors[colorIndex] = temp2;
      }
    },
    lockInput(colorIndex){
      this.colors[colorIndex].disabled === true ? 
      (this.colors[colorIndex].disabled = false) :
      (this.colors[colorIndex].disabled = true);
    },
    addInputColor(){
      this.colors.push({ id: this.id, hex: this.randomHexcolor(), disabled: false });
      this.id++;
    },
    removeInputColor(){
      var i;
      for(i = 0; i < this.colors.length; i++){
        if(this.colors[i].disabled === false){
          if(this.colors.length > 2){
            this.colors.splice(i, 1);
            break;
          }
        }
      }
    },
    randomHexcolor(){
      return(
        "#" + Math.random().toString(16).slice(2, 8)
      );
    }
  },
  computed:{
    gradient(){
      let colors = "linear-gradient(135deg";
      this.colors.forEach(function(e){
        colors += "," + e.hex;
      });
      colors += ")";
      return colors;
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  margin: 0;
  height: 100vh;
  font-family: "Lato", sans-serif;
  font-weight: bold;
  font-size: 1.5rem;
  text-transform: uppercase;
}

#app{
  width: 100vw;
  height: 100%;
  display: flex;
  transition: background-color 500ms;
}

.interface-container {
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li{
  margin-bottom: 5px;
  display: flex;
  align-items: center;
  position: relative;
}

li > button {
  border-radius: 0px 10px 10px 0px;
}

li > button,
.button-group button {
  background: #000000;
  color: #ffffff;
  padding: 15px;
  cursor: pointer;
  min-width: 25px;
  text-align: center;
}

/* Move input up/down buttons */
.moveInput button {
  font-size: 1.25rem;
  cursor: pointer;
  height: 100%;
  width: 100%;
  text-align: center;
}

.moveInput button:hover {
  color: #ffffff;
}

.moveInput {
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: absolute;
  right: calc(100%);
  height: 100%;
  width: 35px;
}

li .up::before {
  content: "\f062";
  opacity: 0;
}

li .down::before {
  content: "\f063";
  opacity: 0;
}

li:hover .up::before,
li:hover .down::before {
  opacity: 1;
  transition: opacity 300ms ease-out;
}

.up::before {
  font-family: "FontAwesome";
  content: " ";
}

.down::before {
  font-family: "FontAwesome";
  content: " ";
}
/* End */

input, button{
  all: unset
}

/* Input field */
input{
  display: inline;
  background: #ffffff;
  padding: 15px;
  max-width: 225px;
  border-radius: 10px 0px 0px 10px;
  transition: color 300ms, background 300ms ease-out;
}

.lockedInput {
  background: #000000;
  color: #ffffff;
  animation-duration: 600ms;
}
/* End */

/* Locked/Unlocked button */
li > button::before {
  font-family: "FontAwesome";
  content: "\f09c";
}

.locked::before {
  content: "\f023";
}
/* End */

button {
  transition: 300ms;
}

/* Button group of Add/Remove/Random */
.button-group button {
  border-radius: 10px;
}

.button-group button:hover {
  background: #ffffff;
  color: #000000;
}

#add::before {
  font-family: "FontAwesome";
  content: "\f067";
}

#remove::before {
  font-family: "FontAwesome";
  content: "\f068";
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 300ms;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
/* End */

ul li i {
  content: "#";
  padding: 15px;
  padding-right: 0px;
  background: #ffffff;
  border-radius: 10px 0px 0px 10px;
}

li.shake {
  animation-duration: 0.75s;
  animation-name: shake;
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}

/* Flip input animation */
.flip-list-move {
  transition: transform 300ms;
}

.flip-list-leave-active {
  position: absolute;
  opacity: 0;
}
/* End */
</style>
