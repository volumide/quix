<template>
    <div class="quiz">
        <h1 class="logo">Quix</h1>
        <div class="box">
            <p class="count" v-if="!submited">Question {{currentIndex + 1}} of {{quest.length}}</p>
            <div class="score">
                <h3 class="score" v-if="submited">score<span> {{score}} </span></h3>
            </div>
            <div class="timer" v-if="time  !== null">
                <h4>{{time}}</h4>
            </div>
            <ul v-if="!submited">
                <p>
                    {{currentQuestion.question}} 
                </p>
                <li 
                    v-for="(option, index) in currentQuestion.options" 
                    :id="option" 
                    :key="index" 
                    :data-answer="`${currentQuestion.answer}`" 
                    @click="getAnswer"
                    :style="currentAnswer == option
                        ? 'background: #1FFFDA; color: #4F50E4; font-weight:bold' 
                        : 'background: 1E90FF;'"
                >{{option}}</li>
            </ul>
            <button @click="next" v-if="currentIndex < quest.length -1">Next</button>
            <button @click="next" v-if="!submited && currentIndex >= quest.length -1">Submit</button>
            <button @click="startGame" v-if="submited" >Restart</button>
        </div>
      
    </div>
</template>

<script>
    import Json from '@/assets/questions.json'
    export default {
        data() {
            return {
                currentQuestion : [],
                answer: '',
                currentIndex : 0,
                currentAnswer: '',
                submited : false,
                score: 0,
                time: 15,
                timer: null,
                quest: Json.data,
                // questions: [
                //     {
                //         id: 1,
                //         question : "Who is the current president of nigeria",
                //         answer: "buhari",
                //         options: [
                //             'jonathan', 'buhari', 'obasanjo', 'adeniyi'
                //         ]
                //     },
                //     {
                //         id: 2,
                //         question : "Nigeria is in which continent",
                //         answer: "africa",
                //         options: [
                //             'africa', 'europe', 'autralia', 'america'
                //         ]
                //     },
                //     {
                //         id: 3,
                //         question : "How many colors are there in the Nigeria flag",
                //         answer: "2",
                //         options: [
                //             '3', '1', '2', '8'
                //         ]
                //     },
                //     {
                //         id: 3,
                //         question : "Abuja is the capital of which of this country?",
                //         answer: "Nigeria",
                //         options: [
                //             'Brazil', 'Crotia', 'Agentina', 'Nigeria'
                //         ]
                //     }
                // ]
            }
        },

        methods: {
            getAnswer(e){
                this.answer = e.target.getAttribute('data-answer')
                this.currentAnswer = e.target.getAttribute('id')
            },

            getQuestion(){
                this.currentQuestion = this.quest[this.currentIndex]
            },
            
            startGame(){
                this.currentIndex = 0
                this.score = 0
                this.time = 15
                this.submited = false
                this.getQuestion()
                this.setTimer()
            },

            calculateScore(){
                if (this.currentAnswer !== '' && this.answer !== '') {
                    if (this.currentAnswer === this.answer) {
                        this.score += 1
                    }
                }
            },
            
            setTimer(){
                this.timer = setInterval(() => {
                    this.time--
                    if (this.time < 0) {
                        this.next()
                    }
                }, 1000);
               
            },

            next(){
                this.calculateScore()
                if (this.currentIndex >= this.quest.length -1) {
                    this.submited = true
                    clearInterval(this.timer)
                    this.time = null
                    return
                }
                this.currentIndex += 1
                this.time = 15
                this.getQuestion()
            },
        },

        async created() {
            this.getQuestion()
            this.setTimer()
            // let value = await this.quest
            // console.log(this.quest[0].id)
        },
    }
</script>

<style>
    *{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
         font-family: 'Nunito', sans-serif;
        font-weight: 600;
        letter-spacing: 1px;
    }

    .logo{
        font-family: 'Satisfy', sans-serif;
        padding: 10px 0;
        font-weight: 900;
        /* color: #1FFFDA; */
        color: #1E90FF;
    }
    .score h3{
        /* border: 1px solid red; */
        padding: 20px 0;
        width: 50%;
        margin: auto;
        margin-bottom: 20px;
    }

    .score h3 span{
        display: block;
        color: #1E90FF;
        /* font-size: 2rem; */
        font-family: 'Satisfy', sans-serif;
        /* font-size: 1.5rem; */
    }

    .quiz{
        align-items: center;
        height: 100vh;
        width: 100% ;
    }

    .box{
        padding: 20px ;
        /* background: red; */
        width: 500px;
        margin: auto;
        max-width: 90%;
    }

    .timer{
        padding: 15px;
        
    }
    .timer h4{
        font-size: 1.6rem;
        font-weight: 700;
        font-family: 'Satisfy', sans-serif;
    }

    h1,h2,h3,h4,h5,h6{
        font-weight: 900;
        text-align: center;
    }

    p{
        display: block;
        text-align: center;
        padding: 5px;
    }

    ul{
        list-style: none;
    }

    li{
        padding: 25px;
        margin: 15px 0;
        cursor: pointer;
        color: #fff;
        text-transform: capitalize;
        border-radius: 5px;
        background: #0783f7;
    }

    

    button{
        padding: 10px 25px;
        border: 1px solid transparent;
        background: #4F50E4;
        border-radius: 3px;
        color: #fff;
        /* margin-right: 10px; */
        cursor: pointer;
        outline: none;
        display: block;
        margin: 10px auto;
        /* font-size: 1.4rem; */
        font-weight: 500;
    }

</style>
