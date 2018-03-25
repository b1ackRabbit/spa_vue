<template>
    <div>
        <div class="">
            <router-link to="/edit-message">Add New Message</router-link>
        </div>

        <div class="list-of-messages">
            <div class="message" v-for="(message,index) in messages">
                <router-link v-bind:to="'/edit-message/' + message.id">edit</router-link>
                <a href="#" class="remove-message" v-on:click.prevent="deleteMessage( message.id )">x</a>
                <router-link v-bind:to="$route.path + '/' + message.id">show detailes (load one message)</router-link>
                <p class="message-author">{{ message.author }}</p>
                <div class="message-body">{{ message.text }}</div>
                
            </div>
        </div>
    </div>
</template>

<script>

export default {
  name: 'ListOfMessages',
  data () {
    return {
      messages : []
    }
  },
  created: function(){
    // this.prepareToSaveMessage();
    this.init();
  },
  methods: {
    init: function(){

      this.getMessages();
    },
    getMessages: function() { 

      this.$http.get('comments').then((response) => { 
        if(!!response.body) {

          var answer = response.body;
          for (var messageNumber in answer){
            if (answer.hasOwnProperty(messageNumber)) {
              if(typeof messageNumber === "number" || !isNaN(parseInt(messageNumber)) && typeof parseInt(messageNumber) === "number"){
                this.messages.push(answer[messageNumber]);
              }
            }
          }

        }
      }, (response) => {
        this.error = response;
      });
    }   
  }

}
</script>
