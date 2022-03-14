<template>
    <div class="quiz">
        <div class="container">
            <h1>Quiz</h1>
            <div v-if="fin == false" class="quizcontent">
                <div class="score">Score : {{score}} / {{Quizjson.length}}</div>
                <div class="cardQuestion">
                    <div class="laquestion">{{AfficheQuestion(NumeroQuestion)}}</div>
                    <div class="listReponse">
                        <ul>
                            <li v-for="item in TableauReponse" v-bind:key="item.index" @click="selectReponse"
                                :id="'index' + idReponse(item)">
                                {{item}}
                            </li>
                        </ul>
                    </div>
                </div>
                <button v-if="questionValid" @click="suivante">Question suivante</button>
            </div>
            <div v-else>
                <div class="result">
                    <div class="ttreresultat">
                        <div v-if="score >= Quizjson.length/2">
                            Bien joué
                        </div>
                        <div v-else>
                            C'est pas top
                        </div>
                    </div>
                    <div class="scoreresultat">
                        Vous avez {{score}} <span v-if="score <= 1">bonne reponse</span><span v-else>bonnes
                            reponses</span> pour {{Quizjson.length}} questions
                    </div>
                </div>
                <button @click="recommence">Recommencer ?</button>
            </div>
        </div>
    </div>
</template>

<script>
    import json from '@/json/quiz.json'

    export default {
        name: 'Quiz',
        data: function () {
            return {
                Quizjson: json,
                NumeroQuestion: 0,
                TableauReponse: [],
                questionAffiche: true,
                questionValid: false,
                reponse: '',
                verif: '',
                score: 0,
                lettreReponse: ['A', 'B', 'C', 'D'],
                idquestion: 0,
                fin: false
            }
        },
        methods: {
            AfficheQuestion: function (num) {
                if (this.questionAffiche == true) {
                    this.AfficheReponse(num);
                }
                this.questionAffiche = false
                return this.Quizjson[num].question

            },
            AfficheReponse: function (num) {
                this.Quizjson[num].tableauReponse.map(detail => {
                    this.TableauReponse.push(detail)
                })
                this.reponse = this.Quizjson[num].resultat
            },
            selectReponse: function (e) {


                if (this.questionValid == false) {
                    if (e.target.innerHTML === this.reponse) {
                        this.verif = true;
                        this.score++;
                        e.target.classList.toggle('vrai')

                    } else {
                        this.verif = false
                        e.target.classList.toggle('faux')
                    }
                }
                this.questionValid = true

            },
            suivante: function () {
                if (this.NumeroQuestion < this.Quizjson.length - 1) {
                    const listreponse = document.querySelectorAll('.listReponse li')
                    listreponse.forEach(reponse => {
                        return reponse.classList.remove('vrai', 'faux')
                    })
                    this.NumeroQuestion++
                    this.TableauReponse = []
                    this.questionValid = false
                    this.questionAffiche = true
                } else {
                    this.fin = true
                }

            },
            idReponse: function (reponse) {
                return this.TableauReponse.indexOf(reponse)
            },
            recommence: function () {
                this.fin = false
                this.NumeroQuestion = 0
                this.TableauReponse = []
                this.AfficheReponse(0)
                this.score = 0
                this.questionValid = false
            }
        }
    }
</script>

<style scoped>
    .quizcontent {
        max-width: 450px;
        margin: auto;
    }
    .cardQuestion {
        background: rgb(230, 230, 230);
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.4);
        padding: 25px;
        color: rgb(5, 5, 17);
    }
    .score {
        margin: 50px 0 0 0;
        padding: 25px 5px;
        text-align: right;
        background: #ffbb23;
        color: rgb(5, 5, 17);
    }

    .laquestion {
        font-size: 1.2rem;
        padding: 25px;
    }

    .cardQuestion ul {
        list-style: none;
        padding: 0;
        margin: 0;
    }

    .cardQuestion li {
        width: 90%;
        padding: 10px;
        border: 1px solid rgb(182, 182, 182);
        cursor: pointer;
        margin: 10px;
        border-radius: 30px;
        transition: 0.3 ease-in
    }

    .cardQuestion li:hover {
        background: rgb(40, 3, 139);
        color: #fff;
    }

    .vrai,
    .cardQuestion li.vrai:hover {
        background: rgb(6, 126, 66);
        color: #fff;
    }

    .faux,
    .cardQuestion li.faux:hover {
        background: rgb(80, 1, 1);
        color: #fff;
    }

    .quiz button {
        margin: 25px auto;
        border-radius: 30px;
        background: #ffbb23;
        padding: 10px 20px;
        border: none;
        font-size: 1.1rem;
        cursor: pointer;
        box-shadow: 0 3px 6px #020202;
    }

    .quiz button:hover {
        box-shadow: 0 1px 3px #020202;
    }
    .result{
         background: rgb(230, 230, 230);
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.4);
        padding: 25px;
        color: rgb(5, 5, 17);
        max-width: 450px;
        margin: auto;
    }
    .ttreresultat{
        font-size: 2rem;
        padding: 25px;
        font-weight: 700;
    }
</style>