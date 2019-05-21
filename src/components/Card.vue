<template>
  <div class="cards container">
    <div
      v-show="!searchFilter.searching"
      class="card"
      :class="{'editCardClass':p }"
      v-for="(note,index) in SentoCards "
      :key="index"
    >
      <div class="card-body" v-show="!p">
        <div>
          <h5 class="card-title">{{note.title}}</h5>
          <p class="card-text">{{note.text}}</p>
          <a v-if="!p" href="#" class="card-link" @click="Editnote(note,true,index)">
            <i class="fas fa-edit"></i> Edit
          </a>
          <a href="#" class="card-link text-danger" @click="DeleteNote(note)">
            <i class="far fa-trash-alt"></i> Delete
          </a>
        </div>
      </div>

      <div style="padding: 15px;" v-show="note.editing">
        <input type="text" class="form-control" v-model="note.title">
        <textarea class="form-control" v-model="note.text" autofocus></textarea>
        <div>
          <button type="button" class="btn btn-secondary mt-2" @click="Editnote(note,false)">Done</button>
        </div>
      </div>
    </div>
    <!-- search k baad edit delet etc operation huaa h neche -->
    <div v-if="searchFilter.searching">
      <div
        class="card"
        :class="{'editCardClass':p }"
        v-for="(filterData,index) in aftersearched"
        :key="index"
      >
        <div class="card-body" v-show="!p">
          <h4>{{filterData.title}}</h4>
          <p>{{filterData.text}}</p>
          <a v-if="!p" href="#" class="card-link" @click="Editnote(filterData,true,index)">
            <i class="fas fa-edit"></i> Edit
          </a>
          <a href="#" class="card-link text-danger" @click="DeleteNote(filterData)">
            <i class="far fa-trash-alt"></i> Delete
          </a>
        </div>

        <!-- search karne k baad editing k liye -->
        <div style="padding: 15px;" v-show="filterData.editing">
          <input type="text" class="form-control" v-model="filterData.title">
          <textarea class="form-control" v-model="filterData.text" autofocus></textarea>
          <div>
            <button
              type="button"
              class="btn btn-secondary mt-2"
              @click="Editnote(filterData,false)"
            >Done</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "Card",
  props: [
    // app vue se notes ki list ko recive kiya gya h prop se
    "SentoCards",
    "searchFilter"
  ],

  data() {
    return {
      p: false, // ye variable show karta h ki eiding ho rahi h ki nahi,
      aftersearched: ""
    };
  },
  methods: {
    Editnote(item, arg) {
      this.p = true;
      item.editing = arg;
      if (arg === false) {
        this.p = false;
        console.log(item.title);
        this.$emit("Edited", item); //
      }
    },
    DeleteNote(value) {
      console.log(value);
      this.$emit("DeletingData", value);
    }
  },
  mounted() {
    console.log(this.searchFilter);
  },
  watch: {
    searchFilter: function(newVal, oldVal) {
      // watch it
      //console.log('Prop changed: ', newVal, ' | was: ', oldVal)
      if (this.searchFilter.searching) {
        var filterdNote = this.SentoCards.filter(function(value) {
          if (value.title.includes(newVal.searchData)) {
            return value;
          }
        });

        if (filterdNote.length == 0) {
          alert("Not Found");
          this.searchFilter.searching= false
        }
        this.aftersearched = filterdNote;
        console.log(this.aftersearched);
      }
    }
  }
};
</script>

<style scoped>
.container {
  position: relative;
}
.card {
  margin: 12px 1%;
  max-width: 31.33%;
  display: inline-block;
  box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.1);
  position: static;
}
.card:hover {
  transform: scale(1.03);
}
.editCardClass {
  max-width: 100%;
  width: 96%;
  position: absolute;
  left: 1%;
  padding: auto;
  box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.08);
}
.editCardClass:hover {
  transform: none;
}
input:focus {
  box-shadow: none;
  outline: -webkit-focus-ring-color auto 0px !important;
}

textarea:focus {
  box-shadow: none;
  outline: -webkit-focus-ring-color auto 0px !important;
}
</style>
