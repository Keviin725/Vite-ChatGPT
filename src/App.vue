<script setup>
import bot from './assets/bot.svg'
import user from './assets/user.svg'
const form = document.querySelector('form')
const chatContainer = document.querySelector('#chat_container')

let loadInterval

// Loader while await the response
function loader(element) {
  element.textContent = ''
  loadInterval = setInterval(()=>{
    element.textContent += '.'

    if(element.textContent === '....'){
      element.textContent= ''
    }
  },300)
}
// Type text letter by letter
function typeText(element, text){
  let index = 0
  let interval = setInterval(()=>{
    if(index < text.length){
      element.innerHTML += text.charAt(index)
      index++
    }else{
      clearInterval(interval)
    }
  },20)

  function generateUniqueId(){
    const timestamp = Date.now()
    const randomNumber = Math.random()
    const hexadecimalString = randomNumber.toString(16)
    return `id-${timestamp}-${hexadecimalString}`
  }

  function chatStripe(isAi, value, uniqueId){
    return(
      `
      <div class= "wrapper ${isAi && 'ai'}">
        <div class="chat">
          <div className="profile">
            <img src="${isAi? bot: user}" alt="${isAi? 'bot': 'user'}"/>
          </div>
          <div class="message" id=${uniqueId}> ${value}</div>
        </div>
      </div>
      `
    )
  }

  const handleSubmit = async(e) =>{
    e.preventDefault();
    const data = new FormData(form)
   
    //user chatStripe
    chatContainer.innerHTML+= chatStripe(false, data.get('prompt'))
    form.reset()

    //bot chatStripe
    const uniqueId = generateUniqueId()
    chatContainer.innerHTML+= chatStripe(true, " ", uniqueId)
    
    //scroll the view while the bot's typing
    chatContainer.scrollTop = chatContainer.scrollHeight

    const messageDiv = document.getElementById(uniqueId)


  }
}
</script>






<template>
  <div id="app">
    <div id="chat_container"></div>
    <form action="">
      <textarea name="prompt" id="" cols="1" rows="1" placeholder="Ask Something..." ></textarea>
      <button type="submit"><img src="./assets/send.svg" alt=""></button>
    </form>

  </div>

</template>

<style scoped>
  
</style>
