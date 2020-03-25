<template>
  <div class="marjong">
    <h1>{{ msg }}</h1>
    <div id="base">
      <div id="yaku">
        <h3>1.役数を選択(役満は13、ダブル役満は14を選択)</h3>
        <select name="yaku" v-model="yaku">
          <option v-for="yaku in yakus" :key="yaku" name="yaku" :value="yaku">{{ yaku }}</option>
        </select>
        <label>役</label>
      </div>
    </div>
    <div id="low" v-show="yaku < 5">
      <div id="basic">
        <h3>2.固定符かどうかを選択</h3>
        <form>
          <input type="radio" id="other" value="20" v-model="b">
          <label for="other">基本(20符)</label>
          <br>
          <input type="radio" id="chi" value="25" v-model="b">
          <label for="chi">七対子(25符固定)</label>
          <br>
          <input type="radio" id="ptsumo" value="18" v-model="b">
          <label for="ptsumo">平和ツモ(20符固定)</label>
          <br>
          <input type="radio" id="pron" value="30" v-model="b">
          <label for="pron">平和ロン(30符固定)</label>
          <br>
          <div>{{ b }} 符</div>
        </form>
      </div>
      <div id="opt" v-show="this.b==20">
        <div id="agari">
          <h3>3.アガリ符を選択</h3>
          <form>
            <input type="radio" id="other" value="0" v-model="a">
            <label for="other">なし</label>
            <input type="radio" id="tsumo" value="2" v-model="a">
            <label for="tsumo">ツモ(+2符)</label>
            <input type="radio" id="ron" value="10" v-model="a">
            <label for="ron">メンゼンロン(+10符)</label>
            <br>
            <div>{{ a }} 符</div>
          </form>
        </div>
        <div id="atama">
          <h3>4.アタマが役牌の場合は選択</h3>
          <form>
            <input type="radio" id="none" value="0" v-model="m">
            <label for="none">なし</label>
            <input type="radio" id="yaku" value="2" v-model="m">
            <label for="yaku">役牌(+2符)</label>
            <br>
            <div>{{ m }} 符</div>
          </form>
        </div>
        <div id="machi">
          <h3>5.待ち方を選択</h3>
          <form>
            <input type="radio" id="none" value="0" v-model="ma">
            <label for="none">両面</label>
            <input type="radio" id="ryan" value="2" v-model="ma">
            <label for="ryan">両面以外(+2符)</label>
            <br>
            <div>{{ ma }} 符</div>
          </form>
        </div>
        <div id="mentsu">
          <h3>6.面子に刻子や槓子が存在する場合は合計の符を入力</h3>
          <p>1つにつき
            <br>刻子: 2-8→4符 それ以外→8符
            <br>槓子: 2-8→16符 それ以外→32符
            <br>鳴いた場合は1/2にする
          </p>
          <form>
            <input v-model="men" placeholder="合計符数を入力(ない場合は0を入力)">
            <br>
            {{ men }} 符
            <br>
          </form>
        </div>
      </div>
    </div>
    <div id="btn">
      <button @click="cal=!cal">Calculate!</button>
    </div>
    <div id="result" v-if="cal">
      <p>
        <strong>
          {{yaku}}翻{{ce}}符
          <br>
          親: {{totaloya}}点 (ツモ: {{oyatsumo}}オール)
          <br>
          子: {{totalko}}点 (ツモ: 親{{oyapay}} 子{{kopay}})
        </strong>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      yakus: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14],
      yaku: 1,
      b: 20,
      a: 0,
      m: 0,
      ma: 0,
      men: 0,
      cal: false
    };
  },
  computed: {
    sumfu: function() {
      return (
        parseInt(this.b, 10) +
        parseInt(this.a, 10) +
        parseInt(this.m, 10) +
        parseInt(this.ma, 10) +
        parseInt(this.men, 10)
      );
    },
    ce: function() {
      if (this.sumfu === 18) {
        return 20;
      } else if (this.sumfu === 25) {
        return 25;
      } else {
        return Math.ceil(parseInt(this.sumfu, 10) / 10) * 10;
      }
    },
    sumyaku: function() {
      return 2 ** (this.yaku + 2);
    },
    oya: function() {
      switch (this.yaku) {
        case 5:
          return 12000;
        case 6:
        case 7:
          return 18000;
        case 8:
        case 9:
        case 10:
          return 24000;
        case 11:
        case 12:
          return 36000;
        case 13:
          return 48000;
        case 14:
          return 96000;
        default:
          if (
            (this.yaku === 3 && this.ce > 60) ||
            (this.yaku === 4 && this.ce > 30)
          ) {
            return 12000;
          } else return this.ce * this.sumyaku * 6;
      }
    },
    ko: function() {
      if (this.yaku > 4 ||
        (this.yaku === 3 && this.ce > 60) ||
        (this.yaku === 4 && this.ce > 30)       
      ) {
        return this.oya / 1.5;
      } else return this.ce * this.sumyaku * 4;
    },
    totaloya: function() {
      return Math.ceil(this.oya / 100) * 100;
    },
    totalko: function() {
      return Math.ceil(this.ko / 100) * 100;
    },
    oyatsumo: function() {
      return Math.ceil(this.totaloya / 300) * 100;
    },
    oyapay: function() {
      return Math.ceil(this.totalko / 200) * 100;
    },
    kopay: function() {
      return Math.ceil(this.totalko / 400) * 100;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.marjong {
  display: inline-block;
  text-align: center;
}
.btn {
  width: 160pt;
  height: 100pt;
}
</style>
