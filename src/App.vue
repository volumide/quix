<template>
    <div class="quiz">
        <div class="box">
            <h1>Quix</h1>
            <p class="count" v-if="!submited">{{currentIndex + 1}} of {{questions.length}}</p>
            <h3 v-if="submited">Score : {{score}}</h3>
            <ul v-if="!submited">
                <p>
                    {{currentQuestion.question}} 
                </p>
                <li 
                    v-for="(option, index) in currentQuestion.options" 
                    :id="option" 
                    :key="index" 
                    :data-answer="`${currentQuestion.answer}`" 
                    @click="getContent"
                    :style="currentAnswer == option
                        ? 'background: #1FFFDA; color: #4F50E4; font-weight:bold' 
                        : 'background: 1E90FF;'"
                >{{option}}</li>
            </ul>
            <button @click="next" v-if="currentIndex < questions.length -1">Next</button>
            <button @click="next" v-if="!submited && currentIndex >= questions.length -1">Submit</button>
            <button @click="startGame" v-if="submited" >Restart</button>
        </div>
        <div class="timer">
            <h4>{{timer}}</h4>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                currentQuestion : [],
                answer: '',
                currentIndex : 0,
                currentAnswer: '',
                submited : false,
                score: 0,
                timer: 60,
                questions: [
                    {
                        id: 1,
                        question : "Who is the pressident of nigeria",
                        answer: "buhari",
                        options: [
                            'jonathan', 'buhari', 'obasanjo', 'adeniyi'
                        ]
                    },
                    {
                        id: 2,
                        question : "Nigeria is in which continent",
                        answer: "africa",
                        options: [
                            'africa', 'europe', 'autralia', 'america'
                        ]
                    },
                    {
                        id: 2,
                        question : "How many colors are there in the Nigeria flag",
                        answer: "2",
                        options: [
                            '3', '1', '2', '8'
                        ]
                    }
                ]
            }
        },

        methods: {
            getContent(e){
                this.answer = e.target.getAttribute('data-answer')
                this.currentAnswer = e.target.getAttribute('id')
            },

            calculateScore(){
                if (this.currentAnswer !== '' && this.answer !== '') {
                    if (this.currentAnswer === this.answer) {
                        this.score += 1
                    }
                }
            },

            // setTimmer(){
            //     setInterval(() => {
            //         this.timer--
            //         // if (this.timer === 0) {
            //         //     this.timer = 0
            //         //     return
            //         // }
            //     }, 1000);
            // },

            next(){
                this.calculateScore()
                if (this.currentIndex >= this.questions.length -1) {
                    this.submited = true
                    this.timer = 0
                    // clearInterval(this.timer)
                    return
                }
                this.timer = 60
                this.currentIndex += 1
                this.getQuestion()
                // this.setTimmer()

            },

            startGame(){
                this.timer = 60
                this.currentIndex = 0
                this.score = 0
                this.submited = false
                this.getQuestion()
            },
            
            getQuestion(){
                this.currentQuestion = this.questions[this.currentIndex]
            }
        },

        created() {
            this.getQuestion()
        },
    }
</script>

<style>
    *{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        /* font-family: 'Pacifico', cursive; */
        font-family: 'Satisfy', cursive;
        font-weight: 600;
        letter-spacing: 1px;
    }

    .quiz{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        width: 100% ;
    }

    .box{
        padding: 20px ;
        /* background: red; */
        width: 500px;
        max-width: 90%;
    }

    .timer{
        /* background: red; */
        padding: 20px;
    }

    h1,h2,h3,h4,h5,h6{
        font-weight: 900;
        /* padding: 10px; */
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
        font-size: 1.4rem;
        font-weight: 300;
    }

</style>
