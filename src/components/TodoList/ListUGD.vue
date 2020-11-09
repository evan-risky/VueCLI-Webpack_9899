<template>
 <v-main class="list">
   <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

   <v-card>
     <v-card-title>
       <v-text-field
         v-model="search"
         append-icon="mdi-magnify"
         label="Search"
         single-line
         hide-details
       ></v-text-field>

       <v-spacer></v-spacer>
       <div>
         <v-select
          v-model="searchPriotity"
          label="Priority"
          :items="['All Priority','Penting', 'Biasa', 'Tidak penting']"
          dense
          outlined
          ></v-select>
        </div>
       <v-spacer></v-spacer>
       
       <v-btn color="success" dark @click="dialog = true">
         Tambah
       </v-btn>

     </v-card-title>
     <v-data-table :headers="headers" :items="filteredItems" :search="search">
       <template v-slot:[`item.actions`]="{ item }">
         <v-btn small class="mr-2" @click="editItem(item)">
           edit
         </v-btn>
         <v-btn small @click="deleteItem(item)">
           delete
         </v-btn>
       </template>
     </v-data-table>
   </v-card>

   <v-dialog v-model="dialog" persistent max-width="600px">
     <v-card>
       <v-card-title>
         <span class="headline" v-if="add == true">
           Form Add
          </span>
          <span class="headline" v-else>
           Form Edit
          </span>
       </v-card-title>
       <v-card-text>
         <v-container>
           <v-text-field
             v-model="formTodo.task"
             label="Task"
             required
             autofocus
           ></v-text-field>

           <v-select
             v-model="formTodo.priority"
             :items="['Penting', 'Biasa', 'Tidak penting']"
             label="Priority"
             required
           ></v-select>

           <v-textarea
             v-model="formTodo.note"
             label="Note"
             required
           ></v-textarea>
         </v-container>
       </v-card-text>
       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancel">
           Cancel
         </v-btn>

         <v-btn v-if="add == true" color="blue darken-1" text @click="save">
            Save
         </v-btn>

         <v-btn v-else color="blue darken-1" text @click="edit(formTodo)">
           Save
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>

   <v-dialog v-model="dialog1" persistent max-width="300px">
     <v-card>
       <v-card-title>
         <span class="headline">Yakin Ingin Hapus?</span>
       </v-card-title>
       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancel">
           Cancel
         </v-btn>
         <v-btn color="blue darken-1" text @click="deleteNow">
           Delete
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>
 </v-main>

 
</template>
<script>
export default {
 name: "List",
 data() {
   return {
     search: null,
     searchPriotity: "All Priority",
     add: true,
     edititem:null,
     dialog: false,
     dialog1: false,

     filters: {
        search: '',
        priority: '',
      },

     headers: [
       {
         text: "Task",
         align: "start",
         sortable: true,
         value: "task",
       },
       { text: "Priority",field: "priority", value: "priority" },
       { text: "Note",  field: "note",value: "note" },
       { text: "Actions", value: "actions" },

     ],
     todos: [
       {
         task: "bernafas",
         priority: "Penting",
         note: "huffttt",
       },
       {
         task: "nongkrong",
         priority: "Tidak penting",
         note: "bersama tman2",
       },
       {
         task: "masak",
         priority: "Biasa",
         note: "masak air 500ml",
       },
     ],
     formTodo: {
       task: null,
       priority: null,
       note: null,
     },
   };
 },
 methods: {
   save() {
     this.todos.push(this.formTodo);
     this.cancel();
   },
   deleteItem() {
     this.dialog1 = true;
     this.edititem = item;
   },
   cancel() {
     this.resetForm();
     this.dialog = false;
     this.edititem = null;
     this.add = true;
     this.dialog1 = false;
   },
   resetForm() {
     this.formTodo = {
       task: null,
       priority: null,
       note: null,
     };
   },
   deleteNow(){
     this.todos.splice(this.todos.indexOf(this.edititem), 1);
     this.dialog1 = false;
   },
   editItem(item) {
     this.add = false;
     this.formTodo = {
        task: item.task,
        priority: item.priority,
        note: item.note,
      };
     this.dialog = true;
     this.edititem = item;
    },

    edit(formTodo) {
     this.edititem.task = formTodo.task;
     this.edititem.priority = formTodo.priority;
     this.edititem.note = formTodo.note;
     this.cancel();
    },
 },
 computed: {
    filteredItems() {
      return this.todos.filter((i) => {
        if(this.searchPriotity !== "All Priority") {
          return !this.searchPriotity || (i.priority === this.searchPriotity);
        } else {
          return this.todos;
        }
      })
    }
  }
};
</script>
