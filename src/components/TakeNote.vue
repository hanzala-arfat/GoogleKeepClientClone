<template>
  <div class="write-text-page">
    <div class="container">
      <div class="input-group" v-if="!makingNote" @click="Enternote(true)">
        <input type="text" class="form-control" placeholder="Write a note here...">
      </div>
      <div class="noteBorder" v-else>
        <input type="text" class="form-control note" placeholder="Title" v-model="notes.title">
        <textarea
          class="form-control note"
          placeholder="Write a note here..."
          v-model="notes.text"
          autofocus
        ></textarea>
        <div>
          <button type="button" class="btn btn-secondary mt-2" @click="SubmitData()">Done</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TakeNote",
  data() {
    return {
      makingNote: false,
      notes: {
        id: 3,
        title: "",
        text:'',
        editing: false
      }
    };
  },

  methods: {
    Enternote(arg) {
      this.makingNote = arg;
    },
    SubmitData(arg) {
      this.makingNote = arg;
      this.notes.id = (this.notes.id + 1);
      this.$emit("UpdateData",this.notes)
      this.notes = {
        title: "",
        text: ""
      };
    }
  }
};
</script>

<style scoped>
.container {
  margin-top: 4rem;
  margin-bottom: 3rem;
}

.input-group {
  box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.1);
}
.note {
  border: none;
}
.noteBorder {
  border: 0px solid;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.1);
}
input:focus {
  box-shadow: none;
  outline: -webkit-focus-ring-color auto 0px;
}

textarea:focus {
  box-shadow: none;
  outline: -webkit-focus-ring-color auto 0px;
}
</style>

 