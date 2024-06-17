<template>
    <div>
        <div  v-if="!toShow">
            <div class="headline">לומדת פיצוץ מטאורים</div>
            <img class="arrow" @click="next" src="../../src/assets/pictures/arrow.svg" alt="arrow"/>
        </div>

        <div v-if="toShow" class="loading"> 
            <img class="shhh-amongus" src="../../src/assets/pictures/shhh.svg" alt="shhh-amongus"/>
         </div>
    </div>
</template>

<script>
export default {
    name: "open-page",
    data() {
        return {
            toShow: false,
        };
    },
    methods: {
        next() {
            this.toShow = true;
            setTimeout(()=> {
                this.$emit('next');
            }, 3800);    
        }
    }
};
</script>

<style scoped>
    body {
        width: 100vw;
        height: 100vh;
    }

    .headline {
        font-family: bn pixeliom;
        color: white;
        position: absolute;
        top: 45%;
        left:50%;
        transform: translate(-50%,-50%);
        font-size: 10vh;
        text-align: center;
    }

    .arrow {
        width: 20vw;
        height: 9vh;
        position: absolute;
        bottom: 5%;
        left:50%;
        transform: translateX(-50%);
        animation: float 2s linear infinite;
    }

     @keyframes float {
        0% {
            bottom: 5%;
        }
        50% {
            bottom: 8%;
        }
        100% {
            bottom: 5%;
        }
     }

     @-webkit-keyframes float {
        0% {
            bottom: 5%;
        }
        50% {
            bottom: 8%;
        }
        100% {
            bottom: 5%;
        }
     }
    
    .shhh-amongus{
        width: 90%;
        height: 42%;
        position: absolute;
        top: 30%;
        left: 5%;
        animation: spin 2s linear infinite;
    }

    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
     
    @-webkit-keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
</style>