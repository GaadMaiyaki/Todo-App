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
  <div class="w-100 h-100 position-absolute d-flex align-items-center p-3" style="background: rgba(0,0,0, 0.7); z-index: 1" v-if="isEdit">
      
    <div class="container">
      <div class="row">
        <div class="col-sm-12 col-md-12 col-lg-6 col-xl-6 bg-light w-100 mx-auto p-3 my-auto rounded">
          <form @submit.prevent="addData">
              <div class="from-group">
                <label for="name" class="text-muted">Name</label>
                <input class="form-control border-top-0 border-right-0 border-info shadow-sm" v-model.trim="nameE" type="text"/>
              </div>
              <div class="from-group mt-2">
                <label for="task" class="text-muted">Description</label>
                <input class="form-control border-top-0 border-left-0 border-info shadow-sm" v-model.trim="descE" type="text"/>
              </div>
              
              <div class="form-group mt-3 text-center">
                <button type="submit" class="btn btn-primary shadow-md mt-2">Edit</button>
              </div>
            </form>
        </div>
      </div>
    </div>

  </div>

  <div class="container-fluid">
    
    <div class="row justify-content-center align-items-center p-4">
      <div class="col-sm-12 col-md-12 col-lg-6 col-xl-6 p-3 mt-5 bg-white shadow border">
        <div class="text-uppercase mb-4">
          <span class="rounded bg-white shadow-sm p-2 text-muted">to do application
            <small class="text-info ml-3">Using Vue js</small>
          </span>
        </div>
        <form @submit.prevent="addData">
          <div class="from-group">
            <label for="name" class="text-muted">Name</label>
            <input class="form-control border-top-0 border-right-0 border-info shadow-sm" v-model.trim="name" type="text"/>
          </div>
          <div class="from-group mt-2">
            <label for="task" class="text-muted">Description</label>
            <input class="form-control border-top-0 border-left-0 border-info shadow-sm" v-model.trim="desc" type="text"/>
          </div>
          
          <div class="form-group mt-3 text-center">
            <button type="submit" class="btn btn-info shadow-md mt-2">Submit</button>
          </div>
        </form>
        
        <div>
          <div class="mb-4 position-relative">
            <div class="rounded bg-white shadow-sm p-2 text-muted text-uppercase">Your tasks' lists</div>
            
            <div class="position-absolute w-30" style="right: 1.5em; top:0" v-if="isEdited">
              <div class="alert alert-success p-1">
                <small>Successfully Edited!</small>
              </div>
            </div>

            <div class="position-absolute w-30" style="right: 1.5em; top:0" v-if="isDeleted">
              <div class="alert alert-success p-1">
                <small>Successfully Deleted!</small>
              </div>
            </div>

          </div>
          <div class="bg-light text-center mt-3 p-2" v-if="dataPerson.length <= 0">
            <span class="text-muted bg-white shadow shadow-sm p-2 rounded">Currently, you have no todo items</span>
          </div>
          <div v-else-if="dataPerson.length > 0" class="table-responsive">
            <table class="table table-striped table-condensed table-hover text-muted w-100">
              <thead>
                <tr>
                  <th>mDel</th>
                  <th>To-do</th>
                  <th>Desc</th>
                  <th>Options</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(v, i) of dataPerson" :key="i" class="">
                  <td>
                    <input type="checkbox" style="cursor: pointer"/>
                  </td>
                  <td> {{ v.name }} </td>
                  <td> {{ v.desc }} </td>
                  <td class="d-flex">
                      <button class="btn btn-sm btn-primary" @click="edit(i)">
                        <i class="fa fa-edit"></i>
                      </button>
                      <button class="btn btn-sm btn-danger mx-2" @click="del(i)">
                        <i class="fa fa-trash"></i>
                      </button>
                      <button class="btn btn-sm btn-success" v-if="isDone">
                        <i class="fa fa-check"></i>
                      </button>
                      <button class="btn btn-sm btn-warning">
                        <i class="fa fa-close"></i>
                      </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      

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
        index: "",
        isDone: false,
        isDeleted: false,
        isEdited: false
            
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
            this.setEditStatus(false);
            this.setisEdited(true);
          }
        }
        else{
          if(this.name && this.desc){
            this.dataPerson.push({...this.dataPerson, name: this.name, desc: this.desc, date: new Date()});
            this.name = this.desc = "";
            console.log(this.dataPerson)
          }
        }
      },

      setEditStatus(status){
        this.isEdit = status;
      },

      setisEdited(status){
        this.isEdited = status;
        setTimeout(()=>{ this.isEdited = false }, 3000)
      },

      setisDeleted(status){
        this.isDeleted = status;
        setTimeout(()=>{ this.isDeleted = false }, 3000)
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
        this.setisDeleted(true);
      }
    }
  }

</script>

<style>

</style>