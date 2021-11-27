<template>
    <div class="based">
        <ul>
            <a href="https://vk.com/alaboet" target="_blank"><li>Связаться с нами</li></a>
            <a href="#"><li>Больше игр</li></a>
            <a href="#"><li>Начальная страница</li></a>
            <a href="https://www.google.com/" target="_blank"><li>Обратно в интернет</li></a>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "header"
    }
</script>

<style scoped>


    .based
    {
        background-color: #42b983;
        width: 70%;
        margin:2rem auto;
        padding: .6rem 0;
        box-shadow: 0 0 10px black;
    }
    a
    {
        text-decoration: none;
        color:white;
    }
    a:hover
    {
        color:mediumpurple;
        transition: .5s;
    }
ul
{

    color: #fff;
    display: flex;
    list-style: none;
    font-size: 22px;
    justify-content: space-evenly;
    padding: 0px;
}
li
{
    font-size: 30px;
    margin:0 1rem;
}
    @media screen and (max-width: 1000px) {
        .based {
            display: none;
        }
    }

</style>