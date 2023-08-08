# Kviz
my project

index.html

<!DOCTYPE html>
<html>
  <head>
     <meta charset="UTF-8">
     <title> Quiz </title>
  </head>
      <body>

          <div class = "container start">
              <h3 class ='container_h3'></h3>
              <div class = 'start-btn'> Начать </div>
          </div>

                   <h3 class="container_h3"> </div>
          <div class="container main">
          <div class="question">23 - 7</div>
          <div class="ans-row">
                 <div class="answer">2</div>
                  <div class="answer">4</div>
                   <div class="answer">16</div>
                    <div class="answer">20</div>
                     <div class="answer">19</div>
          </div>
          </div>

 <script src='https://cdn.jsdelivr.net/npm/animejs@3.2.1/lib/anime.min.js'></script>

       </body>
</html>

style.css

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300&display=swap');

body {
    font-family: 'Montserrat', sans-serif;

}
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column; 
}
.container_h3 {
    margin-bottom: 10px;
}
.question {
    font-size: 100px;
}

.main {
    display:none;
}

.ans-row {
    display: flex;
    box-sizing: border-box;
}
.answer {
    width: 60px;
    height: 60px;
    border: 1px solid black;
    border-radius: 5px;
    margin: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

.start-btn {
    border: 1px solid black;
    border-color: green;
    cursor: pointer;
    border-radius: 20px;
    padding: 10px;
    font-size: 30px;
    color: green;
}

script.js

let question_field = document.querySelector('.question')
 let answer_buttons = document.querySelectorAll('.answer')
  let container_h3 = document.querySelector('.container_h3')
   let start_button = document.querySelector('.start-btn')
    let container_start = document.querySelector('.start')
     let container_main = document.querySelector('.main')


 let cookie = false
 let cookies = document.cookie.split('; ')
   for (let i = 0; i < cookies.length; i += 1) {
       if (cookies[i].split('=')[0] == 'score') {
           cookie = cookies[i].split('=')[1]
             break
    }
}

 if (cookie) {
    let data = cookie.split('/')
    container_h3.innerHTML = `<h3>В прошлый раз вы дали ${data[1]} правильных ответов из ${data[0]}. Точность - ${Math.round(data[1] * 100 / data[0])}%.</h3>`
}

 function randint(min, max) {
    return Math.round(Math.random() * (max - min) + min)
}

 let signs = ['+', '-', '*', '/']
     function getRandomSign() {
         return signs[randint(0, 3)]
}

 function shuffle(array) {
   let currentIndex = array.length,  randomIndex;

  while (currentIndex != 0) {
    randomIndex = Math.floor(Math.random() * currentIndex);currentIndex--;
    [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
  }
  return array;
}

class Question {
    constructor() {
          let a = randint(1, 30)
           let b = randint(1, 30)
             let sign = getRandomSign()

            this.question = `${a} ${sign} ${b}`

        if (sign == "+") {this.correct = a + b}
         else if (sign == "-") {this.correct = a - b}
          else if (sign == "*") {this.correct = a * b}
           else if (sign == "/") {this.correct = Math.round(a / b)}

        this.answers = [
             randint(this.correct - 15, this.correct - 1),
              randint(this.correct - 15, this.correct - 1),
              this.correct,
             randint(this.correct + 1, this.correct + 15),
              randint(this.correct + 1, this.correct + 15),  
        ]
     shuffle(this.answers)
    }

    display () {
        question_field.innerHTML = this.question
         for (let i = 0; i < this.answers.length; i += 1) {
             answer_buttons[i].innerHTML = this.answers[i]
        }
    }
}

     let correct_answers_given = 0
      let total_answers_given = 0
       let current_question

 start_button.addEventListener('click', function() {
     container_start.style.display = 'none'
     container_main.style.display = 'flex'

       current_question = new Question()
       current_question.display()

      correct_answers_given = 0
      total_answers_given = 0

   setTimeout(function() {
       let new_cookie = `score=${total_answers_given}/${correct_answers_given}; max-age=10000000000`
       document.cookie = new_cookie
         container_start.style.display = 'flex'
         container_main.style.display = 'none'

       container_h3.innerHTML = 
      `Вы дали ${correct_answers_given} правильных ответов из ${total_answers_given}. 
      Точность - ${Math.round(correct_answers_given * 100 / total_answers_given)}%.` 
      }, 10000)

})


 for (let i = 0; i < answer_buttons.length; i += 1) {
    answer_buttons[i].addEventListener('click', function() {
        if (answer_buttons[i].innerHTML == current_question.correct) {
            correct_answers_given += 1
            answer_buttons[i].style.background = "#30E55A"
              anime({
                 targets: answer_buttons[i],
                 background: "#FFFFFF",
                 duration: 500,
                 delay: 100,
                 easing: 'linear'
            })
        } else {
           answer_buttons[i].style.background = "#EE3535"
             anime({
                targets: answer_buttons[i],
                background: "#FFFFFF",
                duration: 500,
                delay: 100,
                easing: 'linear'
            })
        }

         total_answers_given += 1
        
         current_question = new Question()
         current_question.display()
 
    })
}
