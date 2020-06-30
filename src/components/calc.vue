<template>
    <div class="hello">
        <h1>{{ title }}</h1>
        <p>{{ message }}</p>
        <hr>

        <div>
            <div>
                <textarea v-model="fomula" cols="40" rows="5">
                
                </textarea>
            </div>
            <div>
                <button v-on:click = "doAction"><!-- 計算アプリの実行ボタン-->
                    CALC
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Calc',
    props:{
        title:String,
    },

    data:function(){
        return {
            message: 'Enter expression:',
            fomula: '0',
        };
    },

    methods: {
        doAction: function(){
            //fomula text sprit and insert array on indent simbol
            var arr = this.fomula.trim().sprit('\n');
            //Last item in arr of array,insert to last 
            var last = arr.pop();
            var fn = '';

            //Repeat run about each item of array 
            for (var n in arr){
                if(arr[n].trim() != ''){
                    //if get item is not empty,var item make and insert to fn
                    fn += 'var ' + arr[n] + ';';
                }
            }

            //After finish repeat, add to "return last item;"
            fn += 'return ' + last + ';';
            //Making functions put together and run in f
            var exp = 'function f(){' + fn + '} f();';
            //Run function use eval function 
            var answer = eval(exp);
            //Run result answer insert to message 
            this.message = 'answer: ' + answer;
            var re = arr.join(';').trim();
            //if re is not empty,re add ';' and last item
            if(re != ''){
                re += ';';
            }
            re += last;
            //Call result-event on $emit
            this.$emit('result-event',re,answer);
        }
    }
}
</script>