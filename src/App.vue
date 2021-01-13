<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <!-- message  -->
          <message v-if="message" :message="message" />

          <!-- new note  -->
          <div>
            <newNote :note="note" @addNote="addNote" @classNote="goClassNote" />
          </div>

          <div class="note-header">
            <!-- title  -->
            <h1>{{ title }}</h1>
            <!-- search  -->
            <search
              :value="search"
              placeholder="Find your note"
              @search="search = $event"
            />
            <!-- icons controls  -->
            <div class="icons">
              <svg
                :class="{ active: grid }"
                @click="grid = true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg
                :class="{ active: !grid }"
                @click="grid = false"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>

          <!-- note list  -->
          <notes
            :notes="notesFilter"
            :grid="grid"
            :clicked="clicked"
            @remove="removeNote"
            @changeTitle="changeTitle"
            @changeDescr="changeDescr"
            @doneEditTitle="doneEditTitle"
            @cancelEditTitle="cancelEditTitle"
            @doneEditDescr="doneEditDescr"
            @cancelEditDescr="cancelEditDescr"
            :noteClass="noteClass"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from "@/components/Message.vue";
import newNote from "@/components/NewNote.vue";
import notes from "@/components/Notes.vue";
import search from "@/components/Search.vue";
export default {
  components: {
    message,
    newNote,
    notes,
    search,
  },
  data() {
    return {
      title: "Notes App",
      search: "",
      noteClass: "standart",
      clicked: false,
      message: null,
      grid: true,
      note: {
        startTitle: "",
        startDescr: "",
        title: "",
        descr: "",
        class: "standart",
      },
      notes: [
        {
          startTitle: "First Note",
          startDescr: "Description for first note",
          title: "First Note",
          descr: "Description for first note",
          date: "Created: " + new Date(Date.now()).toLocaleString(),
          class: "standart",
          dateEdit: "",
        },
        {
          startTitle: "Second Note",
          startDescr: "Description for second note",
          title: "Second Note",
          descr: "Description for second note",
          date: "Created: " + new Date(Date.now()).toLocaleString(),
          class: "important",
          dateEdit: "",
        },
        {
          startTitle: "Third Note",
          startDescr: "Description for third note",
          title: "Third Note",
          descr: "Description for third note",
          date: "Created: " + new Date(Date.now()).toLocaleString(),
          class: "very-important",
          dateEdit: "",
        },
      ],
    };
  },
  computed: {
    notesFilter() {
      let array = this.notes,
        search = this.search;
      if (!search) return array;
      // Small
      search = search.trim().toLowerCase();
      // Filter
      array = array.filter(function (item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });
      // Error
      return array;
    },
  },
  methods: {
    goClassNote(data) {
      this.noteClass = data;
    },
    addNote() {
      let { title, descr } = this.note;
      if (title === "") {
        this.message = "Title can`t be blank!";
        return false;
      }
      this.notes.push({
        title,
        startTitle: title,
        descr,
        startDescr: descr,
        class: this.noteClass,
        date: "Created: " + new Date(Date.now()).toLocaleString(),
        dateEdit: "",
      });
      this.note.startTitle = "";
      this.note.startDescr = "";
      this.note.title = "";
      this.note.descr = "";
      this.note.class = "standart";
      this.message = null;
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    changeTitle(index) {
      let noteTitle = document.querySelectorAll(".note-title")[index];
      let noteInput = document.querySelectorAll(".note-input-title")[index];
      noteTitle.classList.toggle("display-none-d");
      noteInput.classList.toggle("display-none");
    },
    changeDescr(index) {
      let noteDescr = document.querySelectorAll(".note-descr")[index];
      let noteInputDescr = document.querySelectorAll(".note-input-descr")[
        index
      ];
      noteDescr.classList.toggle("display-none-d");
      noteInputDescr.classList.toggle("display-none");
    },
    doneEditTitle(index) {
      this.notes[index].startTitle = this.notes[index].title;
      let noteTitle = document.querySelectorAll(".note-title")[index];
      let noteInput = document.querySelectorAll(".note-input-title")[index];
      noteTitle.classList.toggle("display-none-d");
      noteInput.classList.toggle("display-none");
      this.notes[index].dateEdit =
        ", edited: " + new Date(Date.now()).toLocaleString();
    },
    cancelEditTitle(index) {
      this.notes[index].title = this.notes[index].startTitle;
      let noteTitle = document.querySelectorAll(".note-title")[index];
      let noteInput = document.querySelectorAll(".note-input-title")[index];
      noteTitle.classList.toggle("display-none-d");
      noteInput.classList.toggle("display-none");
    },
    doneEditDescr(index) {
      this.notes[index].startDescr = this.notes[index].descr;
      let noteDescr = document.querySelectorAll(".note-descr")[index];
      let noteInputDescr = document.querySelectorAll(".note-input-descr")[
        index
      ];
      noteDescr.classList.toggle("display-none-d");
      noteInputDescr.classList.toggle("display-none");
      this.notes[index].dateEdit =
        ", edited: " + new Date(Date.now()).toLocaleString();
    },
    cancelEditDescr(index) {
      this.notes[index].descr = this.notes[index].startDescr;
      let noteDescr = document.querySelectorAll(".note-descr")[index];
      let noteInputDescr = document.querySelectorAll(".note-input-descr")[
        index
      ];
      noteDescr.classList.toggle("display-none-d");
      noteInputDescr.classList.toggle("display-none");
    },
  },
};
</script>