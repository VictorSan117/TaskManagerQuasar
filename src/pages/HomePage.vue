<!-- eslint-disable vue/no-v-text-v-html-on-component -->
<template>
  <div
    style="
      margin-left: 20px;
      margin-top: 5px;
      margin-bottom: -20px;
      font-weight: bold;
      font-size: large;
    "
  >
    Add task
  </div>
  <div class="q-pa-md q-gutter-sm">
    <q-editor
      v-model="editor"
      :definitions="{
        save: {
          tip: 'Save your work',
          icon: 'save',
          label: 'Save',
          handler: saveWork,
        },
      }"
      :toolbar="[
        ['bold', 'italic', 'strike', 'underline'],
        ['upload', 'save'],
      ]"
    />

    <q-card
      flat
      bordered
      class="row justify-between"
      v-for="(item, index) in tasks"
      :key="index"
    >
      <q-card-section
        v-html="item.texto"
        class="col"
        :class="item.estado ? 'tachar' : ''"
      >
      </q-card-section>
      <q-btn flat color="blue" @click="item.estado = !item.estado" icon="check"
        >Checked</q-btn
      >
      <q-btn flat color="red" @click="eliminar(index)" icon="delete"></q-btn>
      <q-space />
    </q-card>

    <div v-if="tasks.length == 0" class="flex flex-center" style="opacity: 0.5">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  data() {
    return {
      editor: "",
      tasks: [
        // { texto: "Tarea #1", estado: false },
        // { texto: "Tarea #2", estado: true },
        // { texto: "Tarea #3", estado: false },
      ],
    };
  },
  methods: {
    saveWork() {
      this.tasks.push({
        texto: this.editor,
        estado: false,
      });
      this.$q.notify({
        message: "Task saved",
        color: "green-4",
        textColor: "white",
        icon: "cloud_done",
      });
      this.editor = "";
    },
    eliminar(index) {
      this.$q
        .dialog({
          title: "Dangerous Action!",
          message: "Are you sure to delete this task?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Task deleted",
            color: "red-4",
            textColor: "white",
            icon: "delete",
          });
        });
    },
  },
  setup() {},
};
</script>

<style>
.tachar {
  text-decoration: line-through;
}
</style>

<style lang="scss" scoped></style>
