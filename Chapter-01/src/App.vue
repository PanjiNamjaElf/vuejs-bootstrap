<template>
 <div id="app" class="jumbotron">
  <div class="container">
   <h1>Hello! Nice to meet you!</h1>

   <hr/>

   <form @submit="addMessage">
    <div class="form-group">
     <input class="form-control" maxlength="40" autofocus
            v-model="newMessage.title"
            placeholder="Please introduce yourself :)"/>
    </div>

    <div class="form-group">
     <textarea class="form-control"
               v-model="newMessage.text"
               placeholder="Leave your message!"
               rows="3"></textarea>
    </div>

    <button class="btn btn-primary" type="submit">Send</button>
   </form>

   <hr/>

   <div class="card-columns">
    <card class="card-outline-success" :title="'Hello!'"
          :text="'This is our fixed card!'"
          :footer="'Added on ' + dateToString(Date.now())"></card>

    <card v-for="message in messages"
          :title="message.title"
          :text="message.text"
          :footer="'Added on ' + dateToString(message.timestamp)"></card>
   </div>
  </div>
 </div>
</template>

<script>
  import Firebase from 'firebase'
  import { dateToString } from './utils/utils'
  import Card from './components/Card'

  let config = {
    apiKey: 'AIzaSyAFFTjsJ6NvowcBM5JjtxNfVwat3ii7uTU',
    authDomain: 'sadaharu-4543a.firebaseapp.com',
    databaseURL: 'https://sadaharu-4543a.firebaseio.com',
    projectId: 'sadaharu-4543a',
    storageBucket: 'sadaharu-4543a.appspot.com',
    messagingSenderId: '676872735374'
  }

  let app = Firebase.initializeApp(config)
  let db = app.database()
  let messagesRef = db.ref('messages')

  export default {
    name: 'app',

    data () {
      return {
        newMessage: {
          title: '',
          text: '',
          timestamp: null
        }
      }
    },

    firebase: {
      messages: messagesRef
    },

    components: {
      Card
    },

    methods: {
      addMessage (e) {
        e.preventDefault()

        if (this.newMessage.title === '') {
          return
        }

        this.newMessage.timestamp = Date.now()
        messagesRef.push(this.newMessage)

        this.newMessage.text = ''
        this.newMessage.title = ''
        this.newMessage.timestamp = null
      },

      dateToString: dateToString
    }
  }
</script>
