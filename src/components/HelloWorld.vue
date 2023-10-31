<template>
  <div class="hello">
    <form @submit.prevent="insert">
      <p><input type="text" name="name" v-model="name"/></p>
      <button>저장</button>
    </form>
    <ul>
      <li v-for="item in data"  :key="item.date">
        {{item.name}} / {{item.count}}
        <button @click="update(item.date, item.count)">좋아용</button>
        <button @click="del(item.date)">삭제</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  mounted(){
    this.select();
  },
  data(){
    return{
      name:``, data: []
    }
  },
  name: `heyheyhey`,
  props: {
    msg: String
  },
  methods:{
    select(){
      axios.get("http://localhost:3050/api")
      .then(res=>{
        this.data = res.data;
      })
    },

    insert(){
      const data = {name:this.name, date:Date.now(), count:0}
      axios.post("http://localhost:3050/api/insert", data)
      .then(res=>{
        this.data = res.data;
      })
    },

    del(date){
      axios.delete(`http://localhost:3050/api/delete?date=${date}`)
      .then(res=>{
        this.data = res.data;
      })
    },

    update(date,count){
      axios.put(`http://localhost:3050/api/update?date=${date}`, {count: count+1})
      .then(res=>{
        this.data = res.data;
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
