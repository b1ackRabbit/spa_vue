<template>
    <div>    
        <div class="message">
            <div class="message" v-for="(message,index) in messages">
                <messageShow v-bind:message="message" v-bind:messagesQuantity="message.length"></messageShow>
            </div>
        </div>
    </div>
</template>

<script>


import messageShow from './MessageShow.vue';

export default {
  name: 'Message',
  data () {
    return {
      messages : []
    }
  },
  components: {
    'messageShow' : messageShow
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
      
      this.$http.get("comments/" + messageId).then((response) => { 
        this.messages.push(response.body);

      }, (response) => {
        this.error = response;
      });                    
    }  
  }

}
</script>
