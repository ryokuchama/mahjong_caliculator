<template>
  <div class="marjong">
    <h1>{{ msg }}</h1>
    <div id="base">
      <div id="yaku">
        <h3>1.役数を選択(役満は13、ダブル役満は14を選択)</h3>
        <select name="yaku"  v-model="yaku" v-on:change="modify">
        <option v-for="yaku in yakus" name="yaku" :value="yaku">{{ yaku }}</option>
        </select>
        <label>役</label>
      </div>
      <div id="basic">
        <h3>2.固定符かどうかを選択</h3>
        <form>
            <input type="radio" id="chi" value=25 v-model="b">
            <label for="chi">七対子(25符固定)</label>
            <input type="radio" id="other" value=20 v-model="b">
            <label for="other">それ以外(20符)</label>            
            <br>
            <div>{{ b }} 符 </div><br>
        </form>
      </div>
    </div>
    <div id="opt">
      <div id="agari">
        <h3>3.アガリ符を選択</h3>
        <form>
            <input type="radio" id="other" value=0 v-model="a">
            <label for="other">なし</label>
            <input type="radio" id="tsumo" value=2 v-model="a">
            <label for="tsumo">ツモ(+2符)</label>
            <input type="radio" id="ron" value=10 v-model="a">
            <label for="ron">ロン(+10符)</label>         
            <br>
            <div>{{ a }} 符</div><br>
        </form>
      </div>
      <div id="atama">
        <h3>4.アタマが役牌の場合は選択</h3>
        <form>
            <input type="radio" id="none" value=0 v-model="m">
            <label for="none">なし</label>
            <input type="radio" id="yaku" value=2 v-model="m">
            <label for="yaku">役牌(+2符)</label>         
            <br>
            <div>{{ m }} 符</div><br>
        </form>
      </div>
      <div id="machi">
        <h3>5.待ち方を選択</h3>
        <form>
            <input type="radio" id="none" value=0 v-model="ma">
            <label for="none">両面</label>
            <input type="radio" id="ryan" value=2 v-model="ma">
            <label for="ryan">両面以外(+2符)</label>         
            <br>
            <div>{{ ma }} 符</div><br>
        </form>
      </div>
      <div id="mentsu">
        <h3>6.面子に刻子や槓子が存在する場合は入力</h3>
        <form>
            <input v-model="men" placeholder="合計符数を入力"><br>
            {{ men }} 符
            <br>
        </form>
      </div>
    </div>
    <div id="result">
      親: {{totaloya}}点<br>
      子: {{totalko}}点
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data () {
    return{
      yakus: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14],
      yaku: 1,
      b: 20,
      a: 0,
      m: 0,
      ma: 0,
      men: 0
    }
  },
　computed: {
  sumfu: function() {return this.b + this.a + this.m + this.ma + this.men},
  sumyaku: function() {return 2 ** (this.yaku + 2)},
  totaloya: function() {return this.sumfu * this.sumyaku * 6},
  totalko: function() {return this.sumfu * this.sumyaku * 4}
}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.marjong {
  display: inline-block;
  text-align: center;
}

.base {
  display: inline-block;
  text-align: left;
}

.opt {
  display: inline-block;
  text-align: left;
}
</style>
