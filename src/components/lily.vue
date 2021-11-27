<template>
    <div class="lily">
        <a href="#" @click.prevent="check"><img id="lilyImg" src="https://pngpress.com/wp-content/uploads/2020/09/uploads_lotus_flower_lotus_flower_PNG28.png" alt="Кувшинка не загрузилась"></a>

    </div>
</template>

<script>
    export default {
        name: "lily",
        props:{
            id:Number,
            lilyName:String,
            isMined:Boolean
        },
        emits:['check'],
        data()
        {
            return{
            gameOver:false,
            }
        },
        methods:{
            check()
            {
                this.$emit('checkIfMined')
            },
        },
        /*computed:{
            imgRequire()
            {
                return require('src/photos/525-5250329_water-lily-png-clip-art-image-transparent-png.png')
            }
        }*/
    }
</script>

<style scoped>
    #lilyImg {
        width:100%;
    }
    #lilyImg:hover
    {
        opacity: 0.6;
        transition: .6s;
    }
    .lily
    {
width: 100%;
    }@media screen and (max-width: 850px) {
        .lily {
            width: 100%;
        }
    }

</style>