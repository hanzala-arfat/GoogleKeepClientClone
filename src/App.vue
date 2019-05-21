<template>
  <div id="app">
    <!-- navebaar se search kiye gye data ko rececive kiya gya h fir us data ko cards ko  searchFilter ki madad se send kiya gya h  -->
    <NavBar v-on:SearchData="searched($event)"></NavBar>
    <!-- niche takenote se emit kiye hue data ko reccive kiya gya h  -->
    <TakeNote v-on:UpdateData="Updated($event)"></TakeNote>
    <!-- niche notes arry ko Card file ko send kiya gya h wo prop karega -->
    <Cards
      v-bind:SentoCards="notes"
      v-bind:searchFilter="Filternotes"
      v-on:Edited="EditedData($event)"
      v-on:DeletingData="RemoveData($event)"
    ></Cards>
    <!-- uper EitedData() edit kiye Card.vue ke eimit kiye  data ko recive kiya gya h-->
  </div>
</template>

<script>
import axios from "axios";
import NavBar from "./components/NavBar.vue";
import TakeNote from "./components/TakeNote.vue";
import Cards from "./components/Card.vue";
import { error } from "util";

export default {
  name: "app",
  components: {
    //improt kiye gye component ko niche  ke naam se tag banaya gya h
    NavBar,
    TakeNote,
    Cards
  },
  data() {
    return {
      Filternotes:'', 
      notes: [
        // bina axios k liye niche ka write kia gya tha
        {
          id: 1,
          title: "First Note Server par uaplod ye data nahi hai",
          text:
            "Some quick example text to build on the card title and make up the bulk of the card's content.",
          editing: false
        }
        // {
        //   id: 2,
        //   title: "Secand Note",
        //   text:
        //     "Some quick example text to build on the card title and make up the bulk of the card's content.",
        //   editing: false
        // },
      ]
    };
  },
  methods: {
    // bina axios (server) ka data note me write karta h
    // Updated(value) {
    //   this.notes.push(value);
    //   console.log("Appp vue page", value);
    // },

    Updated(value) {
      // new data add by emit k zariye
      axios
        .post("http://localhost:5001/notes", {
          id: value.id,
          title: value.title,
          text: value.text,
          edititing: value.edititing
        })
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    //bina server ka
    // EditedData(cardEditData) {
    //   //this.notes.push(cardEditData);
    //   for (var i = 0; i < this.notes.length; i++) {
    //     if (this.notes[i].id === cardEditData.id) {
    //       this.notes[i] = cardEditData;
    //     }
    //   }
    //   // console.log(cardEditData.id);
    //   console.log(this.notes);
    // },
    EditedData(cardeditedData) {
      for (let note of this.notes) {
        if (note.id === cardeditedData.id) {
          console.log(cardeditedData.text, cardeditedData.title);
          axios
            .post(`http://localhost:5001/notes/edit/${cardeditedData.id}`, {
              id: cardeditedData.id,
              title: cardeditedData.title,
              text: cardeditedData.text
            })

            .then(function(response) {
              console.log(response);
            })
            .catch(function(error) {
              console.log(error);
            });
        }
      }
    },
    // de
    // RemoveData(becomeDel) {
    //   for (var i = 0; i < this.notes.length; i++) {
    //     if (this.notes[i].id === becomeDel.id) {
    //       this.notes.splice([i], 1);
    //     }
    //     console.log(this.notes);
    //   }
    // }

    RemoveData(becomeDel) {
      for (var i = 0; i < this.notes.length; i++) {
        if (this.notes[i].id === becomeDel.id) {
          axios
            .post(`http://localhost:5001/notes/delete/${becomeDel.id}`, {
              id: becomeDel.id,
              text: becomeDel.text,
              title: becomeDel.title,
              editing: becomeDel.title
            })
            .then(function(response) {
              console.log("Deleted: " + response);
            })
            .catch(function(error) {
              console.log(error);
            });
        }
      }
    },
    searched(searchedData) {
      this.Filternotes = searchedData;
      console.log(searchedData);
    }
  },

  mounted() {
    axios
      .get("http://localhost:5001/notes")
      .then(Response => {
        if (Response.status === 200) {
          let listNote = Response.data.keepnote;
          for (let singlenote of listNote) {
            this.notes.push(singlenote);
          }
        }
      })
      .catch(error => {
        console.log("Error is :" + error);
      });
  }
};
</script>

