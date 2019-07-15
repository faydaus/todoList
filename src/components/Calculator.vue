<template>
    <div class="calculator">
       <div class="display">{{current || '0'}}</div>
       <div class="btn" @click="clear()">AC</div>
       <div class="btn" @click="sign()">+/-</div>
       <div class="btn" @click="percent()">%</div>
       <div class="btn operator" @click="divide()">÷</div>
       <div class="btn" @click="append('7')">7</div>
       <div class="btn" @click="append('8')">8</div>
       <div class="btn" @click="append('9')">9</div>
       <div class="btn operator" @click="time()">x</div>
       <div class="btn" @click="append('4')">4</div>
       <div class="btn" @click="append('5')">5</div>
       <div class="btn" @click="append('6')">6</div>
       <div class="btn operator" @click="subtract()">-</div>
       <div class="btn" @click="append('1')">1</div>
       <div class="btn" @click="append('2')">2</div>
       <div class="btn" @click="append('3')">3</div>
       <div class="btn operator" @click="add()">+</div>
       <div class="btn zero" @click="append('0')">0</div>
       <div class="btn" @click="dot('.')">.</div>
       <div class="btn operator" @click="equal()">=</div>
    </div>
</template>

<script>
export default {
    name: 'Calculator',
    data(){
        return{
            current: '',
            previous: '',
            operator: null,
            operatorClicked: false
        }
    },
    methods:{
        clear(){
            this.current = '';
        },
        sign(){
            this.current = this.current.charAt(0) === '-'?
            //append - sign infront of current number
                this.current.slice(1): `-${this.current}`;
        },
        percent(){
            //change string value to float
             this.current = `${parseFloat(this.current/100)}`;
        },
        append(number){
            if(this.operatorClicked){
                this.current = '';
                this.operatorClicked = false;
            }
            this.current = `${this.current}${number}`;
        },
        dot(){
            if(this.current.indexOf('.') === -1){
                this.append('.');
            }
        },
        setPrevious(){
            this.previous = this.current;
            this.operatorClicked = true;
        },
        divide(){
            this.operator = (a,b) => a / b;
            this.setPrevious();
        },
        time(){
            this.operator = (a,b) => a * b;
            this.setPrevious();
        },
        subtract(){
            this.operator = (a,b) => a - b;
            this.setPrevious();
        },
        add(){
            this.operator = (a,b) => a + b;
            this.setPrevious();
        },
        equal(){
            this.current = `${this.operator(parseFloat(this.current), parseFloat(this.previous))}`;
            this.previous = '';
        }
    }
}
</script>

<style scoped>
.calculator{
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
    width: 500px;
}

.display{
    grid-column: 1 / 5;
    background: rgb(97, 96, 96);
    text-align: center;
    color: white;
}
.zero{
    grid-column: 1/3;
    text-align: center;
}
.btn{
    background-color: #eeee;
    text-align: center;
    border: 1px solid #333;
}
.operator{
    background-color: orange;
    color: white;
}
</style>
