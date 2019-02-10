<template>
  <main v-if="getSupportMessages.isLoaded"
        class="display-flex">
    <div class="caption flex-12">
      <h1 class="mt-15">Messages</h1>
    </div>
    <div @click="sendMessage"
         class="button button__primary mt-10 ml-10">
        Send
    </div>
    <alert v-if="getAlert.id === 'fail'" />
  </main>
</template>

<script>
import Alert from '../../elements/Alert';


export default {
  data() {
    return {
      userMessage: '',
      files: [],
      removeFile: false,
    };
  },
  components: {
    Alert,
  },
  ...
  computed: {
    ...mapGetters(['getSupportMessages', 'getAlert']),
  },
  methods: {
    ...mapActions(['loadSupportMessages', 'showAlert']),
    sendSupportMessage() {
      this.$http
        .post(`${api}api/v1/messages`, formData)
        .then(() => {
          this.userMessage = '';
          this.files = [];
          this.$router.go(0);
          this.scrollToEnd();
        })
        .catch(({ response: { data } }) => {
          if (data.length === 1) {
            this.showAlert({
              color: 'danger',
              text: `${data[0].message}`,
              id: 'fail',
            });
          } else if (data.length === 2) {
            this.showAlert({
              color: 'danger',
              text: `${data[0].message} ${data[1].message}`,
              id: 'fail',
            });
          }
        });
    },
  },
};
</script>
