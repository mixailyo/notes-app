<template>
  <div class="notes">
    <div
      class="note"
      :class="[{ full: !grid }, note.class]"
      v-for="(note, index) in notes"
      :key="index"
    >
      <div class="note-header-title">
        <p
          @click="changeTitle(index)"
          class="note-title"
          :class="{ 'display-block-d': clicked }"
        >
          {{ note.title }}
        </p>
        <div
          class="note-input-title display-none"
          :class="({ 'display-block': !clicked }, { 'display-none': clicked })"
        >
          <input
            v-model="note.title"
            @keyup.enter="doneEditTitle(index)"
            @keyup.esc="cancelEditTitle(index)"
            type="text"
          />
          <div class="note-buttons-title">
            <button @click="doneEditTitle(index)" class="btn">Done</button>
            <button @click="cancelEditTitle(index)" class="btn">Cancel</button>
          </div>
        </div>
        <p style="cursor: pointer" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p
          style="cursor: pointer"
          @click="changeDescr(index)"
          class="note-descr"
          :class="{ 'display-block-d': clicked }"
        >
          {{ note.descr }}
        </p>
        <div
          class="note-input-descr display-none"
          :class="({ 'display-block': !clicked }, { 'display-none': clicked })"
        >
          <textarea
            v-model="note.descr"
            @keyup.enter="doneEditDescr(index)"
            @keyup.esc="cancelEditDescr(index)"
            type="text"
          ></textarea>
          <div class="note-buttons-descr">
            <button @click="doneEditDescr(index)" class="btn">Done</button>
            <button @click="cancelEditDescr(index)" class="btn">Cancel</button>
          </div>
        </div>
        <span>{{ note.date }}</span>
        <span>{{ note.dateEdit }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    notes: {
      type: Array,
      required: true,
    },
    grid: {
      type: Boolean,
      default: true,
    },
    noteClass: {
      type: String,
      default: "standart",
    },
    clicked: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    removeNote(index) {
      console.log(`Note id - ${index} removed`);
      this.$emit("remove", index);
    },
    changeTitle(index) {
      this.$emit("changeTitle", index);
    },
    changeDescr(index) {
      this.$emit("changeDescr", index);
    },
    doneEditTitle(index) {
      this.$emit("doneEditTitle", index);
    },
    cancelEditTitle(index) {
      this.$emit("cancelEditTitle", index);
    },
    doneEditDescr(index) {
      this.$emit("doneEditDescr", index);
    },
    cancelEditDescr(index) {
      this.$emit("cancelEditDescr", index);
    },
  },
};
</script>

<style lang="scss">
.notes {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 40px 0;
}
.note {
  width: 46%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
  &.full {
    width: 100%;
  }
  &:hover {
    transform: translateY(-5px);
  }
}
.note-header-title {
  display: flex;
  align-items: center;
  justify-content: space-between;
  h1 {
    font-size: 32px;
  }
  p {
    font-size: 22px;
    color: #402caf;
  }
}
.note-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  h1 {
    font-size: 32px;
  }
  p {
    font-size: 22px;
    color: #402caf;
  }
}
.note-body {
  p {
    margin: 20px 0;
  }
  span {
    font-size: 14px;
    color: #999999;
  }
}
svg {
  transition: 0.3s all;
  margin-right: 12px;
  color: #999999;
  cursor: pointer;
  &:last-child {
    margin-right: 0;
  }
  &:hover {
    color: #402caf;
  }
  &.active {
    color: #402caf;
  }
}
.important {
  & .note-title {
    background-color: yellow;
  }
}
.very-important {
  background-color: #ef2a2a5c;
}
.note-input-title {
  display: flex;
  width: 94%;
  & button {
    margin-left: 15px;
  }
}
.note-input-descr {
  margin: 10px 0;
  display: flex;
  width: 94%;
  & button {
    margin-left: 15px;
  }
}
.display-none {
  display: none;
}
.display-block {
  display: block;
}
.display-block-d {
  display: block;
}
.display-none-d {
  display: none;
}
.note-title {
  cursor: pointer;
}
.note-buttons-title {
  display: flex;
}
.note-buttons-descr {
  display: flex;
  flex-direction: column;
  & button {
    margin-bottom: 5px;
  }
}
textarea {
  font-family: "Montserrat", Helvetica, Arial, sans-serif;
  font: "Montserrat", Helvetica, Arial, sans-serif;
}
@media screen and (max-width: 980px) {
  .note-buttons-title {
    flex-direction: column;
    & button {
      margin-bottom: 5px;
    }
  }
}
@media screen and (max-width: 768px) {
  .prority-wrapper {
    margin-bottom: 10px;
  }
  .note-header {
    flex-direction: column;
    h1 {
      margin-bottom: 15px;
    }
  }
  .icons {
    display: none;
  }
  .notes {
    flex-direction: column;
  }
  .note {
    width: 95%;
  }
}
@media screen and (max-width: 480px) {
  .prority-wrapper {
    flex-direction: column;
    p {
      margin-bottom: 5px;
    }
  }
  .note-input-descr,
  .note-input-title {
    flex-direction: column;
  }
  .note-buttons-descr,
  .note-buttons-title {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    margin-top: 10px;
    .btn {
      padding: 0.5em 1em;
      margin: 0px 0px 0px 0px;
    }
  }
}
</style>