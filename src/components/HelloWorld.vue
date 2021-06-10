<template>
  <div>
    <div class="flex">
      <h1>Participantes</h1>
      <div class="box">
        <input type="text" name="name" id="name" class="input" @keypress.enter="save()" v-model="name">
        <button type="button" @click.prevent="save()" class="btn">Add</button>
      </div>
      <ul>
        <li v-for="user in users" :key="user.id">
          <button type="button" @click.prevent="deleteUser(user.id)">x</button>
          {{ user.name }}
        </li>  
      </ul>
      <button type="button" @click.prevent="shuffle()" class="btn-shuffle">Sortear Equipes</button>
    </div>
    <div class="teams" v-show="teams">
      <h1>Times</h1>
      <ul>
        <li v-for="(team, index) in teams" :key="index">
          <p v-for="user in team" :key="user.id">
            {{ user.name }}
          </p>
        </li>
      </ul>
    </div>   
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data() {
    return {
      users: null,
      name: null,
      teams: null
    }
  },
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      axios
        .get('http://45.32.170.183/list')
        .then(r => this.users = r.data);
    },
    save() {
      axios
        .post('http://45.32.170.183/add', {
          name: this.name
        })
        .then(r => {
          if (r.status == 200) {
            this.getList();
            this.name = null;
          }
        });
    },
    shuffle() {
      axios
        .get('http://45.32.170.183/shuffle')
        .then(r => this.teams = r.data);
    },
    deleteUser(id) {
      axios
        .delete(`http://45.32.170.183/remove/${id}`)
        .then(r => {
          if (r.status == 200) {
            this.getList();
          }
        });
    }
  }
}
</script>

<style scoped>
h1 {
  margin: 40px 0 10px;
}
ul {
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  padding: 0;
  justify-content: center;
}
li {
  position: relative;
  min-width: 240px;
  background-color: #42b983;
  margin: 8px;
  padding: 8px;
  color: #fff;
  font-weight: bold;
  flex: 1
}
li button {
  position: absolute;
  display: flex;
  justify-content: center;
  align-content: center;
  top: -5px;
  right: -5px;
  background-color: #eee;
  width: 20px;
  border-radius: 2px;
  font-weight: bold;
  color: rgb(184, 0, 0);
  cursor: pointer;
  border: none;
}
.box {
  width: 80%;
  display: flex;
  margin: auto;
  margin-bottom: 16px;
}

.box .input {
  flex: 1;
  padding: 8px;
  font-size: 24px;
  margin-right: 8px;
  border: 2px solid #44fa;
  border-radius: 4px;
}
.box .btn {
  padding: 10px 60px;
  margin-left: 8px;
  font-size: 20px;
  background-color: #44fa;
  border: none;
  border-radius: 4px;
  color: #fff;
  font-weight: bold;
  text-transform: uppercase;
  cursor: pointer;
  transition: all .3s;
}
.box .btn:hover {
  background-color: #44fe;
}
.btn-shuffle {
  margin-top: 16px;
  padding: 10px 60px;
  font-size: 20px;
  background-color: #44fa;
  border: none;
  border-radius: 4px;
  color: #fff;
  font-weight: bold;
  text-transform: uppercase;
  cursor: pointer;
  transition: all .3s;
}
.btn-shuffle:hover {
  background-color: #44fe;
}
</style>
