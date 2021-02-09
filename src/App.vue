<template>
  <!-- <div v-once v-text="count"></div> -->
    <!-- <ol>
      the binding is used for binding an element as to make each its own unique
      <li v-for="(v, key) in dataPerson" v-bind:key="key">{{ v.name }}</li>
    </ol> -->

    <!-- <input :[attribute]="newDataPerson"/> -->
    <!-- the v-model has been used to updaye the state of newdataperson here -->
    <!-- note that v-model has many properties like lazy (similar to an unfocusout event), trim, number and many others -->
    
    <!-- <input v-model.trim="newDataPerson"/>
    <button @click="addData">click me</button> -->

    <!-- using the v-bind or an ordinary colon will make us bind a declared relay with any type of event we want to handle -->
    <!-- <button :disabled="isShow">add new</button> -->
  <div class="w-100 h-100 position-absolute d-flex align-items-center" style="background: rgba(0,0,0, 0.7); z-index: 1" v-if="isEdit">
    
    <div></div>
    <div class="bg-light w-50 mx-auto p-3 my-auto rounded" id="modal">
      <form @submit.prevent="addData">
        <div class="from-group">
          <label for="name">Name</label>
          <input class="form-control" v-model="nameE" type="text"/>
        </div>
        <div class="from-group">
          <label for="task">Task</label>
          <input class="form-control" v-model="descE" type="text"/>
        </div>
        
        <div class="form-group mt-3">
          <button class="btn btn-success">Edit</button>
        </div>
      </form>
    </div>
    <div></div>

  </div>

  <div class="container">
    <div class="row">
      <div></div>
      
      <div class="col-8 mx-auto wow slideInRight border p-3">
        <form @submit.prevent="addData">
          <div class="from-group">
            <label for="name">Name</label>
            <input class="form-control" v-model="name" type="text"/>
          </div>
          <div class="from-group">
            <label for="task">Task</label>
            <input class="form-control" v-model="desc" type="text"/>
          </div>
          
          <div class="form-group mt-3">
            <button type="submit" class="btn btn-dark">submit</button>
          </div>
        </form>
        
        <div>
          <h5 class="text-danger text-center">Your tasks' lists</h5>
          <div class="bg-light text-center mt-3" v-if="dataPerson.length <= 0">
            <small>You have no do item</small>
          </div>
          <div v-else-if="dataPerson.length > 0">
            <table class="table table-striped table-hover">
              <thead>
                <tr>
                  <th></th>
                  <th>S/n</th>
                   <th>To-do</th>
                  <th>Description</th>
                  <th>Edit/Delete</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(v, i) of dataPerson" :key="i">
                  <td>
                    <input type="checkbox" style="cursor: pointer"/>
                  </td>
                  <td> {{ i + 1 }} </td>
                  <td> {{ v.name }} </td>
                  <td> {{ v.desc }} </td>
                  <td>
                      <button class="btn btn-sm btn-warning mr-2" @click="edit(i)">Edit</button>
                      <button class="btn btn-sm btn-danger" @click="del(i)">Delete</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      
      <div></div>

    </div>
  </div>
</template>

<script>
  export default{
    data(){
      return {
        
        attribute: "value",
        // name: "<h1>My name is Peter Maiyaki</h1>",
        // isShow: false,
        // isDo: false,
        isEdit: false,
        count: 0,
        name: "",
        desc: "",
        nameE: "",
        descE: "",
        dataPerson: [],
        tempPerson: [],
        index: ""
            
      }
    },
    
    
    mounted(){
      let interval = setInterval(
        ()=>{ 
          if(this.count < 10) this.count++;
          else clearInterval(interval);
        }, 
      1000);

      return interval;
    },

    methods: {

      addData(){
        
        if(this.isEdit){
          if(this.nameE && this.descE){
            this.dataPerson[this.index] = {name: this.nameE, desc: this.descE};
          }

          this.setEditStatus(false);
        }
        else{
          if(this.name && this.desc){
            this.dataPerson.push({...this.dataPerson, name: this.name, desc: this.desc});
            this.name = this.desc = "";
          }
        }
      },

      setEditStatus(status){
        this.isEdit = status;
      },

      getTask(taskIndex){
        console.log(taskIndex);
        return this.dataPerson.filter((v, i) => i === taskIndex );
      },

      edit(i){
        this.setEditStatus(true);
        const {name, desc} = this.getTask(i)[0];
        this.nameE = name;
        this.descE = desc;
        this.index = i;
      },

      del(i){
        this.dataPerson.splice(i, 1);
      }

    }
  }

</script>

<style>

</style>