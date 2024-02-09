<template>
  <div class="panel">
    <select class="panel-select" v-model="selected">
      <option value="plus">Сложение</option>
      <option value="minus">Вычитание</option>
      <option value="multiplication">Умножение</option>
      <option value="division">Деление</option>
      <option value="multiplication">Степень</option>
      <option value="sqrt">Корень</option>
      <option value="calcFromString">Строка</option>
    </select>
    <div class="panel-row" v-if="selected !== 'sqrt' && selected !== 'calcFromString'">
      <input v-model="num1" class="panel-row__input"/>
      <input v-model="num2" class="panel-row__input"/>
      <span class="panel-row__symbol">=</span>
      <input :value="result" disabled class="panel-row__input"/>
    </div>

    <div class="panel-row" v-else>
      <input v-model="num1" class="panel-row__input"/>
      <span class="panel-row__symbol">=</span>
      <input :value="result" disabled class="panel-row__input"/>
    </div>
    <button class="panel-btn" @click="Calc">Посчитать</button>
  </div>
</template>

<script>
const baseURL = "http://localhost:5183/api/calc";
export default {
  name: "Calc",

  data() {
    return {
      selected: 'plus',
      result: '',
      num1: '',
      num2: '',
    }
  },

  methods: {
    async Calc() {
      try {
        let requestOptions
        if (this.selected === 'calcFromString'){
          requestOptions = {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify(
                {
                  str: this.num1,
                })
          };
        }
        else{
          requestOptions = {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify(
                {
                  num1: this.num1,
                  num2: this.num2
                })
          };
        }

        const res = await fetch(`${baseURL}/${this.selected}`, requestOptions);

        if (!res.ok) {
          const message = `An error has occured: ${res.status} - ${res.statusText}`;
          alert(message)
        }

        this.result = await res.json();
      } catch (err) {
        alert(err.message);
      }
    }
  }
}
</script>

<style scoped lang="scss">
.panel {
  text-align: start;
  margin-right: auto;
  margin-left: auto;
  max-width: 500px;
  width: 100%;
  color: black;
  padding: 20px;
  background: whitesmoke;
  box-shadow: 4px 12px 18px 13px rgba(34, 60, 80, 0.2);
  border-radius: 25px;
  &-select{
    height: 40px;
    font-size: 20px;
    margin-bottom: 20px;
    width: 100%;
  }
  &-btn{
    height: 40px;
    font-size: 20px;
    margin-bottom: 20px;
    width: 100%;
  }
  &-row {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
    :last-child{
      margin-right: 0;
    }

    &__input {
      padding-right: 10px;
      padding-left: 10px;
      margin-right: 10px;
      font-size: 20px;
      height: 40px;
      width: 100%;
    }

    &__text {
      margin-bottom: 5px;
    }

    &__symbol {
      margin-right: 10px;
      font-size: 20px;
    }
  }
}
</style>