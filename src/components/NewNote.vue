<template>
  <div class="new-note">
    <label><b>Title</b></label>
    <input v-model="note.title" type="text" />
    <div class="prority-wrapper">
      <p><b>Select note priority:</b></p>
      <p>
        <input
          v-model="note.class"
          @click="standartPriority()"
          name="priority"
          type="radio"
          value="standart"
          checked
        />
        Standart
      </p>
      <p>
        <input
          v-model="note.class"
          @click="importantPriority()"
          name="priority"
          type="radio"
          value="important"
        />
        Important
      </p>
      <p>
        <input
          v-model="note.class"
          @click="veryImportantPriority()"
          name="priority"
          type="radio"
          value="very-important"
        />
        Very important
      </p>
    </div>
    <label><b>Description</b></label>
    <textarea v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New note</button>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addNote() {
      this.$emit("addNote", this.note);
    },
    standartPriority() {
      this.$emit("classNote", "standart");
    },
    importantPriority() {
      this.$emit("classNote", "important");
    },
    veryImportantPriority() {
      this.$emit("classNote", "very-important");
    },
  },
};
</script>

<style lang="scss">
.new-note {
  text-align: center;
}
</style>

<style lang="scss" scoped>
button {
  margin-top: 15px;
}
input {
  margin-bottom: 20px;
}
.prority-wrapper {
  display: flex;
  font-size: 14px;
  & div {
    margin: 0 10px;
  }
  & input {
    display: inline-block;
    width: 13px;
    min-height: 13px;
    margin-right: 5px;
    margin-left: 10px;
  }
}
</style>