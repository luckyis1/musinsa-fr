<template>
  <div id="musinsa">
    <div class="title-container">
      <span>무신사 과제</span>
    </div>
    <div class="filter-container">
      <button>생존인물만</button>
      <button>여자</button>
      <button>tvSerise없음</button>
      <button>초기화</button>
    </div>
    <div class="list-container">
      <div class="row">
        <div class="txt">
          <div>name: 111, 222, 333, 444, 555</div>
          <div>title: 일본을 무찌르다~</div>
          <div>
            <span>books~~~</span>
            <span>tvSeriese</span>
          </div>
        </div>
        <div class="btn">
          <button>삭제</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Musinsa',
  data() {
    return {
      list: {},
    }
  },
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      console.log('this get list')
    },
  }
}
</script>

<style scoped lang="scss">

#musinsa {
  width:100%;
  height:100%;
  display: flex;
  flex-flow:column;
  padding:10px;

  .title-container{
    background-color:skyblue;
    padding:10px 0;
    span {
      font-size:18px;
    }
  }

  .filter-container{
    display: flex;
    flex-flow: row;
    justify-content: space-around;
    padding: 10px 0;
    background-color: #eeeeee;
    button{
      padding:5px 10px;
      font-size:12px
    }
  }

  .list-container{ 
    display:flex;
    flex-flow:column;
    .row{
      display: flex;
      flex-flow: row;
      align-items: center;
      justify-content: space-around;
      padding: 5px 0;
      margin-top: 10px;
      background-color: lightslategray;
    }
    .txt{
      font-weight: 600;
      text-align: left;
    }
  }
}

</style>