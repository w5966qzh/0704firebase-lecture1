# 0704firebase-lecture1

HTML
```
<div id="app">
  <ul is="transition-group">
    <li v-for="user in users" class="user" :key="user.id">
      <span>{{user.name}} - {{user.email}}</span>
      <button v-on:click="removeUser(user)">X</button>
    </li>
  </ul>
  <form id="form" v-on:submit.prevent="addUser">
    <input v-model="newUser.name">
    <input v-model="newUser.email">
    <input type="submit" value="Add User">
  </form>
</div>

    <!-- vue -->
<!--     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script> -->

    <!-- firebase -->
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase.js"></script>
```

JavaScript
```
var config = {

}
firebase.initializeApp(config)
console.log("firebase",firebase)

const db = firebase.firestore();
var usersRef = db.collection('users')
console.log("usersRef",usersRef)

// create Vue app
var app = new Vue({
  data: {
    newUser: {
      name: '',
      email: ''
    },
    users:[]
  },
  // computed property for form validation state
  created(){
    db.collection("users").onSnapshot(snapshot=>{
      snapshot.forEach(doc=>{
        console.log("doc.id",doc.id)
        console.log("doc.data()",doc.data())
        const user = {
          id:doc.id,
          name:doc.data().name,
          email:doc.data().email
        }
        this.users.push(user)
      })
    })
  },
  // methods
  methods: {
    addUser: function () {
      const data = {
        name : this.newUser.name,
        email: this.newUser.email
      }
      db.collection("users").set(data)
    },
    removeUser(user){
    
    }
  }
}).$mount('#app')

```
