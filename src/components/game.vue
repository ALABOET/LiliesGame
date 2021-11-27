<template>
    <div class="main">

        <app-button text="Правила" @action="rulesRequested=false?rulesRequested:!rulesRequested"></app-button>
        <div v-if="rulesRequested">
        <div class="rules">
            <p>Приветствуем Вас в игре от 1XBET - Лягушки и Кувшинки.</p>

            <p>
             Правила игры максимально просты и заключаются в следующем: перед вами 3 ряда кувшинок по 4 штуки в каждом.
            </p>
            В первом ряду 3 кувшинки обычные, а вот четвертая - с сюрпризом ввиде бомбы, попав на которую игрока ожидает конец игры. Во втором ряду заминированных кувшинки 2, а в 3 ряду - 3.
                <p>
                Заминированные кувшинки ранее остаются в таком же виде, так что игроку придется максимально непросто
                </p>
            Каждый игрок делает ставку и выбирает кувшинку. Если выбранная кувшинка незаминирована, то он проходит к следующему ряду, а коэффициент увеличивается.
                <p>
            После каждого раунда у игрока есть возможность забрать всю выигранную сумму или продолжить играть.</p>
        </div>
        </div>
         <div class="info">
             <p class="roundName" v-if="firstRound && !moneyTaken && !gameIsOver">Первый раунд</p>
             <p class="roundName" v-if="secondRound && !moneyTaken && !gameIsOver">Второй раунд</p>
             <p class="roundName" v-if="thirdRound && !moneyTaken && !gameIsOver">Третий раунд</p>
            <p v-if="secondRound && !gameIsOver">Ваш баланс до второго раунда: {{balance-bet}} руб.</p>
             <p v-if="thirdRound && !gameIsOver">Ваш баланс до третьего раунда: {{balance-bet}} руб.</p>
             <p v-else-if="nullRound">Текущий баланс: {{balance}} рублей.</p>
             <p v-else-if="firstRound && !gameIsOver">Ваш баланс перед первым раундом: {{parseInt(balance) - parseInt(bet)}} руб.</p>
             <p v-else-if="gameIsOver && !congrats && !moneyTaken">Неудача, кувшинка оказалась заминированной! Ваш итоговый баланс: {{moneyTaken?parseInt(balance)-parseInt(bet)+parseInt(totalSum):parseInt(balance)}} руб.</p>
             <p v-else-if=" gameIsOver && congrats">Поздравляю, Вы прошли игру и заслуженно выиграли {{parseInt(totalSum)}} рублей.
             Ваш итоговый баланс: {{parseInt(balance) + parseInt(totalSum)}}</p>
             <p v-if="nullRound">В окне ниже сделайте ставку и игра начнется.</p>
            <p v-if="!nullRound && !gameIsOver">Коэффициент на данный раунд - {{coefficient}}</p>
            <input v-if="!gameStarted && nullRound" type="number" placeholder="Ваша сумма в рублях" v-model="bet">
            <div v-if="bet<=balance">
                <p v-if="!gameIsOver">Ставка: {{bet}} рублей.</p>
                <p v-if="firstRound && !nullRound && !moneyTaken && !gameIsOver">Возможный выигрыш с учетом коэффициента - {{bet*coefficient}} рублей</p>
             <p v-else-if="!firstRound && !nullRound && !moneyTaken && !gameIsOver" >Возможный выигрыш с учетом коэффициента - {{totalSum*coefficient}} рублей. </p>
                <p v-if="!nullRound && !gameIsOver">Итоговая выигранная сумма: {{totalSum}} рублей.</p>
                <p v-if="gameIsOver">Игра закончена</p>
                <app-button text="Сыграть еще раз" v-if="gameIsOver" @action="reloadPage"></app-button>
                <p v-if="moneyTaken && !firstRound">Поздравляем, вы забрали {{totalSum}} рублей</p>
                <p v-else-if="moneyTaken && firstRound">Поздравляем, вы забрали {{bet}} рублей, ваш итоговый баланс - {{parseInt(balance)}} </p>
                <div v-if="!gameStarted" class="notStarted">
            <app-button text="Начать игру" v-if="bet!=0" @action="startTheGame"></app-button></div>
                <div v-else-if="gameStarted && !gameIsOver" class="started">
                    <app-button text="Забрать деньги" @action="finishTheGame"></app-button>
            </div>
            </div>
            <div v-else-if="bet>balance && !gameIsOver"><p>Ставка невозможна, не хватает средств</p></div>
        </div>
        <div class="allHere" v-if="gameStarted && !gameIsOver">
            <div class="row111" v-if="firstRound">
                <p>Первый ряд</p>
        <div class="lilyRow"
             v-for="lily in lilies"
             :key="lily.name">
            <lily @checkIfMined="checkIfMined(lily.isMined)"></lily>
    </div>
            </div>
            <div class="row222" v-if="secondRound">
                <p>Второй ряд</p>
                <div class="lilyRow" v-for="lily in lilies" :key="lily.id">
                    <lily @checkIfMined="checkIfMined(lily.isMined)"></lily>
                    <br>
                </div>
            </div>
            <div class="row333" v-if="thirdRound">
                <p>Третий ряд</p>
                <div class="lilyRow" v-for="lily in lilies"
                     :key="lily.id">
                    <lily @checkIfMined="checkIfMined(lily.isMined)"></lily>
                    <br>
                </div>
            </div>
    </div>
        </div>
</template>
<script>
    import lily from "@/components/lily"
    import './app-button'
    import AppButton from "@/components/app-button";
    export default {
        name: "game",
        components:{AppButton, lily},
        data()
        {
            return{
                rulesRequested:false,
                gameIsOver:false,
                moneyTaken:false,
                balance:500,
                bet:'',
                congrats:false,
                sumWon:0,
                totalSum:0,
                coefficient:2,
                gameStarted:false,
                lilies:[{name:'lily1', isMined: false,id:1},{name:'lily2', isMined: false,id:2},{name:'lily3', isMined: false,id:3},{name:'lily4', isMined: false,id:4}],
                nullRound:true,
                firstRound:false,
                secondRound:false,
                thirdRound:false,
            }
        },
        methods:{
startTheGame()
{
    this.gameStarted = true;
    this.firstRound = true
    this.nullRound = false
    let randomLily = Math.floor(Math.random() * (3 - 0) + 0);
    this.lilies[randomLily].isMined = true; //заминировал рандомную кувшинку
    console.log(this.lilies)

},
            restartTheGame()
            {
                this.firstRound =false
                this.secondRound = false
                this.thirdRound = false
                this.nullRound = true
                this.gameIsOver = false
this.gameStarted = false
            },
mine()
{
    let randomMine = Math.floor(Math.random() * (3 - 0) + 0);
    this.lilies[randomMine].isMined = true
},
            checkIfMined(currentLily)
            {
                let randomLily = Math.floor(Math.random() * (3 - 0) + 0);
                this.lilies[randomLily].isMined = true;
                if(this.firstRound)
                {
                    if(currentLily) {
                        this.gameIsOver = true
                        this.balance -= this.bet
                    }
                    else if(!currentLily)
                    {
                        this.firstRound = false
                        this.sumWon = this.bet*this.coefficient
                        this.totalSum += this.sumWon
                        this.sumWon = 0
                        console.log(`в данным момент выиграл ${this.sumWon}`)
                        this.secondRound = true
                        console.log('Прошли первый этап')
                        this.coefficient = 3
                    }
                }
                else if(this.secondRound)
                {
                    if(currentLily) {
                        this.gameIsOver = true
                        this.balance -= this.bet
                    }
                    else if(!currentLily)
                    {
                        this.secondRound = false
                        this.sumWon = this.bet*this.coefficient
                        this.totalSum += this.sumWon
                        this.sumWon = 0
                        console.log(`в данным момент выиграл ${this.sumWon}`)
                        this.thirdRound = true
                        console.log('Прошли второй этап этап')
                        this.coefficient = 6
                    }
                }


                else if(this.thirdRound)
                {
                    if(currentLily) {
                        this.gameIsOver = true
                        this.balance -= this.bet
                        console.log('умер на последнем этапе')
                    }
                    else if(!currentLily)
                    {
                        this.thirdRound = false
                        this.sumWon = this.bet*this.coefficient
                        this.totalSum += this.sumWon
                        this.congrats = true
                        this.gameIsOver = true

                    }
                }
                console.log(this.lilies)
            },
            finishTheGame() {
                this.moneyTaken = true
                if(this.nullRound)
                {
                    this.gameIsOver = true
                }
                else if (this.firstRound) {
                    this.gameIsOver = true
                    console.log('забрал перед первым раундом')
                } else if(this.secondRound)
                {
                    this.sumWon = Math.floor(this.bet * this.coefficient)
                    this.gameIsOver = true
                    console.log('забрал перед вторым раундом')
                }
                else if(this.thirdRound)
                {
                    this.sumWon = Math.floor(this.bet * this.coefficient)
                    this.gameIsOver = true
                    console.log('забрал перед третьим раундом')
                }


            },
            reloadPage()
            {
                window.location.reload()
            },
        }

    }
</script>

<style scoped>
.main
{
    font-size: 40px;
}
.row1,.row2,.row3
{
    margin:2rem 0;
}
.lilyImg {
    width: 12rem
}
.lilyImg:hover
{
    opacity: 0.6;
    transition: .6s;
}

.rules
{
    background-color: #42b983;
    width: 90%;
    margin:2rem auto;
    padding: .6rem 1.5rem;
    box-shadow: 0 0 10px black;
    color: white;
}
.allHere
{
    background-color: #42b983;
    width: 60%;
    margin: 2rem auto;
    padding: 0.6rem 1.5rem;
    box-shadow: 0 0 10px black;
    color: white;
}
.row111,.row222,.row333
{
    display: flex;
    width: 90%;
    margin: 0 auto;
}
.lilyRow
{
    margin:auto 1rem
}
    .info
    {
        background-color: #42b983;
        width: 30%;
        margin:2rem auto;
        padding: .6rem 1.5rem;
        box-shadow: 0 0 10px black;
        color: white;
    }

    .roundName
    {
    font-size: 40px;
    }
    input
    {
        font-size: 20px;
        width:50%;
    }

    @media screen and (max-width: 1000px){
        .info
        {
        width: 80%;
        }
        .rules
        {
            width: 80%;
            padding:2rem
        }
        .main
        {
            font-size: 25px;
        }
        .row111,.row222,.row333
        {
            display: block;
        }


    }
</style>