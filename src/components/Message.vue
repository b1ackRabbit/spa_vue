<template>
    <div class="list-of-messages">
        <div class="message" v-for="(message,index) in messages">
            <a href="#" class="remove-message" v-on:click.prevent="deleteMessage( message.id )">x</a>
             <router-link v-bind:to="'/edit-message/' + message.id">edit</router-link>
            <router-link to="/messages">back to full list</router-link>
            <p class="message-author">{{ message.author }}</p>
            <div class="message-body">{{ message.text }}</div>
            <span>number in list {{ message.id }}</span> <br>
            <span>created {{ message.created_at }}</span> <br>
            <span>updated {{ message.updated_at }}</span>            
            
        </div>
    </div>
</template>

<script>
var url = "http://comments.stage.itsvit.org/api/comments";
export default {
  name: 'Message',
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
     this.getMessage(this.$route.params.id);
    },
    getMessage: function(messageId){
      console.log('fff');
      this.$http.get(url + "/" + messageId).then((response) => { 
        this.messages.push(response.body);

      }, (response) => {
        this.error = response;
      });                    
    }  
  }

}
</script>
