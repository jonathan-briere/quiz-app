<template>
  <div class="game">
    <div class="heading">
      <div>
        <img
          class="responsive"
          src="../assets/logo.jpg"
          width="100"
          height="100"
        />
      </div>
      <div>
        <h1 style="padding-top: 10px">GK Quiz</h1>
      </div>
      <div>
        <img
          class="responsive"
          src="../assets/sharp.jpg"
          width="200"
          height="100"
        />
      </div>
    </div>
    <div class="normal">
      <div class="question">
        <div>
          <p style="font-weight: bold;">Q{{ qno + 1 }}) {{ show.q }}</p>
          <ul class="listQ" style="list-style-type:none">
            <li
              class="innerL"
              id="reset"
              v-for="(option, index) in show.op"
              :key="index"
            >
              <input
                type="radio"
                v-bind:value="index + '.' + option"
                v-model="picked"
              />
              <label v-bind:for="index" v-bind:id="index">{{ option }}</label>
            </li>
          </ul>
        </div>

        <div>
          <button class="customButton" @click="clickHint()">
            <img
              class="responsive"
              src="../assets/hint.jpg"
              title="Hint"
              width="35"
              height="35"
            />
          </button>
          <button class="customButton" @click="result()">
            <img
              style="margin-left:130px"
              class="responsive"
              src="../assets/close.jpg"
              title="Exit"
              width="35"
              height="35"
            />
          </button>
          <br />
          <img
            class="reponsive"
            src="../assets/earth.jpeg"
            width="200"
            height="200"
          />
        </div>
      </div>

      <div class="bottom">
        <div class="listQ" style="padding-top:10px">IQ Level={{ point }}</div>
        <div>
          <button class="next customButton" @click="checkAns()">
            <img src="../assets/next.png" width="40" height="40" />
          </button>
        </div>
        <div style="padding-top:10px">{{ time }}</div>
      </div>
    </div>

    <app-timer @sendTime="updateTime" @stopTimer="getstopTimer" />
    <div v-bind:class="[{ active: isActive }, errorClass]"></div>

  </div>

</template>

<script>
import Timer from "../components/timer.vue";
export default {
  components: {
    "app-timer": Timer
  },
  data() {
    return {
      picked: null, //Ans that is picked.. using selection
      point: 0, // Points Show the User Score based on the given answers.
      time: null, //Timer that is being accessed from time.vue component
      stop: null, //Stop function to stop the timer, that is bound to stopTimer in vue
      qno: 0, //Indexing Of Question No
      show: "",
      active: true,
      question: [
        {
          // Q1
          q: "Which of the following is the highest peak in Australia?",
          op: ["Mawson Peak", "Mt. Townsend", "Mt Cook", "Mt. Kosciusko"],
          hint: "I don't know how to cook",
          ans: "Mt Cook"
        },

        {
          // Q2
          q:
            "Who among the following discovered Australia for the first time in 1770?",
          op: [
            "James Cook",
            "Matthew Flinders",
            "Stephen Hawkins",
            "Vasco de Gama"
          ],
          hint: "1770..",
          ans: "James Cook"
        },

        {
          // Q3
          q:
            "What is Australia's largest city, which is famous for its opera house and harbor?",
          op: ["Adelaide", "Melbourne", "Sydney", "Brisbane"],
          hint: "Pretty known city it is..",
          ans: "Sydney"
        },

        {
          // Q4
          q: "Which is the largest reef system in the world?",
          op: [
            "Great Barrier Reef",
            "Great Dividing Range",
            "Great Artesian Basin",
            "Australasia"
          ],
          hint: "Think Dude xD",
          ans: "Great Barrier Reef"
        },

        {
          // Q5
          q:
            "PSL 2020 will be the ______ season of the Pakistan Super League ?",
          op: ["4th", "5th", "6th", "None of these"],
          hint: "It's Coming.. ",
          ans: "5th"
        },

        {
          // Q6
          q:
            "The areas around the North and South poles within the Polar circles is called?",
          op: [
            "The Temperature Zone",
            "The Torrid Zone",
            "The Hardiness Zone",
            "The Frigid Zone"
          ],
          hint: "Fridge",
          ans: "The Frigid Zone"
        },

        {
          // Q7
          q: "ICC was founded in the year?",
          op: ["1910", "1914", "1909", "1905"],
          hint: "It's somewhere in middle",
          ans: "1905"
        },

        {
          // Q8
          q: "The first Persian newspaper was?",
          op: [
            "Mirat-ul-Akhbar",
            "Persian News",
            "AL-Hilal",
            "Jam-i-Jahan Numa"
          ],
          hint: "Don't Know",
          ans: "Mirat-ul-Akhbar"
        },

        {
          // Q9
          q: "The term ‘Computer’ is derived from?",
          op: ["German", "Latin", "French", "Arabic"],
          hint: "Even I Don't Know that",
          ans: "Latin"
        },

        {
          // Q10
          q: "Who is the father of Computer?",
          op: [
            "Allen Turing",
            "Simur Cray",
            "Augusta Adaming",
            "Charles Babbage"
          ],
          hint: "Revolutionized the ERA",
          ans: "Charles Babbage"
        },

        {
          // Q11
          q: "Who is the father of Internet?",
          op: ["Chares Babbage", "Vint Cerf", "Denis Riche", "Martin Cooper"],
          hint: "American Internet pioneer",
          ans: "Vint Cerf"
        },

        {
          // Q12
          q: "If a computer has more than one processor then it is known as?",
          op: [
            "Uni-process",
            "Multiprocessor",
            "Multi-threaded",
            "Multi-programming"
          ],
          hint: "Example, Dual Core, Quad Core.. etc",
          ans: "Multiprocessor"
        },

        {
          // Q13
          q: "WWW stands for?",
          op: [
            "World Whole Web",
            "Wide World Web",
            "Web World Wide",
            "World Wide Web"
          ],
          hint: "Go Google -_-",
          ans: "World Wide Web"
        },

        {
          // Q14
          q: "What type of operating system MS-DOS is?",
          op: [
            "Command Line Interface",
            "Graphical User Interface",
            "Multitasking",
            "Menu Driven Interface"
          ],
          hint: "Ms-DOS, old os",
          ans: "Command Line Interface"
        },

        {
          // Q15
          q: "Which technology is used in compact disks?",
          op: ["Mechanical", "Electrical", "Electro Magnetic", "Laser"],
          hint: "CD Writer use which technology? think",
          ans: "Laser"
        },

        {
          // Q16
          q: "The computer that process both analog and digital is called?",
          op: [
            "Analog computer",
            "Digital computer",
            "Hybrid computer",
            "Mainframe computer"
          ],
          hint: "Hmmmmmm......",
          ans: "Hybrid computer"
        },

        {
          // Q17
          q: "Who is the father of Computer science?",
          op: [
            "Allen Turing",
            "Charles Babbage",
            "Simur Cray",
            "Augusta Adaming"
          ],
          hint: "He worked to break Enigma",
          ans: "Allen Turing"
        },

        {
          // Q18
          q: "You can organize files by storing them in?",
          op: ["archives", "folders", "indexes", "lists"],
          hint: "Hey, why? don't you know that",
          ans: "folders"
        },

        {
          // Q19
          q: "Which device is required for the Internet connection?",
          op: ["Joystick", "Modem", "CD Drive", "NIC Card"],
          hint: "Tricky! right? haha",
          ans: "Modem"
        },

        {
          // Q20
          q: "Junk e-mail is also called?",
          op: ["spam", "spoof", "sniffer script", "spool"],
          hint: "huh!! Last Question",
          ans: "spam"
        }
      ]
    };
  },
  methods: {
    checkAns: function() {
      //Check the Ans
      this.picked = this.picked.split(".");
      if (this.picked[1] == this.show.ans) {
        this.correctAns();
      } else {
        this.wrongAns();
      }
      this.picked = null;
    },
    //Next Function Basically, Brings up the next question.
    next: function() {
      if (this.qno < this.question.length - 1) {
        this.qno++;
        this.show = this.question[this.qno];
      } else {
        this.result();
      }
    },

    correctAns: function() {
      // This Function is run if the ans is Correct.
      var obj = document.getElementById(this.picked[0]);
      obj.style.backgroundColor = "#63E25D";
      this.$fire({
        title: "Correct",
        text: "Answer",
        type: "success",
        timer: 1000
      }).then(() => {
        obj.style.backgroundColor = "#c4c4c4";
        this.point += 2;
        this.next(); //When the Answer is connect. point+2 and next question.
      });
    },
    wrongAns: function() {
      // This Function is run if the ans is Wrong.
      var obj = document.getElementById(this.picked[0]);
      obj.style.backgroundColor = "#C70039";
      var i = 0;
      for (i = 0; i < 4; i++) {
        if (this.show.op[i] == this.show.ans) break;
      }
      var obj2 = document.getElementById(i);
      obj2.style.backgroundColor = "#63E25D";
      this.$fire({
        title: "Wrong",
        text: "Correct Answer is " + this.show.ans,
        type: "error",
        timer: 1000
      }).then(() => {
        obj.style.backgroundColor = "#c4c4c4";
        obj2.style.backgroundColor = "#c4c4c4";
        this.point -= 2;
        this.next(); //When the Answer is connect. point+2 and next question.
      });
    },

    clickHint: function() {
      //Hint Function to Show Hint for Questions
       this.$fire({
        title: "",
        text: this.show.hint,
        type: "info",
        timer: 1000
      });
      // this.$alert(this.show.hint, "", "info");
    },

    result: function() {
      //Result Function Shows the result
      this.stop();
      this.$alert(
        "You Scored " + this.point + " (Time Remain: " + this.time + ")",
        "Result"
      ).then(() => {
        this.$router.push("/");
      });
    },

    //events (Child To Parents)
    updateTime: function(updatedTime) {
      this.time = updatedTime;
    },

    getstopTimer: function(stopFn) {
      this.stop = stopFn;
    }
    //
  },

  //lifecycle
  created() {
    this.show = this.question[this.qno];
  }
};
</script>

<style scoped>
.game {
  border-radius: 15px;
  width: 100%;
  background-color: #e5e5e5;
  padding-bottom: 50px;
  border-radius: 10px;
}

.game .heading {
  padding: 10px 9px 5px 5px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background-color: #c4c4c4;
  text-align: center;
  margin-bottom: 80px;
  border-radius: 10px;
}

.question {
  padding: 4px 9px 4px 9px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  text-align: left;
}

.normal {
  background-color: #c4c4c4;
}

.bottom {
  padding-top: 10px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  text-align: left;
  width: 80%;
  margin: auto;
}

.innerL {
  padding-bottom: 20px;
}

.customButton {
  background: none !important;
  border: none;
  padding: 0 !important;
  /*optional*/
  font-family: arial, sans-serif;
  /*input has OS specific font-family*/
  color: #069;
  text-decoration: underline;
  cursor: pointer;
}

.closebutton {
  margin-left: 150px;
}
</style>
