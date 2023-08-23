<template>
    <div>
      <div id="interactive" class="viewport"></div>
      <button class="btn btn-danger w-100" v-on:click="CancelScan">Cancelar</button>
    </div>
  </template>
  
  <script>
  import Quagga from 'quagga';
  
  export default {
    methods: {
      CancelScan () {
        Quagga.stop();
        console.log("Stop");
      }
    },
    mounted() {
      Quagga.init({
        inputStream: {
          name: 'Live',
          type: 'LiveStream',
          target: document.querySelector('#interactive'),
        },
        decoder: {
          readers: ['ean_reader'],
        },
      }, (err) => {
        if (err) {
          console.error('Error:', err);
          return;
        }
        Quagga.start();
      });
        Quagga.onDetected((result) => {
          Quagga.stop();
          alert(result.codeResult.code);
        });
    },
    beforeUnmount() {
      Quagga.stop();
    },
  };
  </script>
  
  <style>
  canvas{
    width: 0;
    height: 0;
  }
  #interactive{
    width: 100%;
  }
  video{
    border: transparent solid 2px;
    max-width: 100%;
    animation: parpadear 2s infinite;
  }
  @keyframes parpadear {
    0%, 100% {
      border-color: red;
    }
    50% {
      border-color: transparent;
    }
  }
  @media (min-width: 768px) {
    video {
      max-width: 50%;
    }
  }
  </style>