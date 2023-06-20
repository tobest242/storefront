<template>
    <div>
      <p>Status: {{ status }}</p>
      <p>Transaction reference: {{ txRef }}</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'Success',
    data() {
      return {
        status: null,
        txRef: null,
      };
    },
    created() {
      axios.get('/api/confirm-payment/', {
          params: {
            status: this.$route.query.status,
            tx_ref: this.$route.query.tx_ref,
          }
        })
        .then(response => {
          this.status = response.data.status;
          this.txRef = response.data.tx_ref;

          if (this.status === 'successful') {
            this.clearCart();
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
  };
  </script>
  