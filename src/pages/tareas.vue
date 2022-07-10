<template>
  <q-page class="q-pa-lg">
    <!-- Agregar tarea -->
    <div class="row q-pa-sm bg-warning puntas">
      <q-input
        filled
        v-model="nuevaTarea"
        class="col"
        label="Agregar tarea"
        @keyup.enter="agregarTarea"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="agregarTarea" />
        </template>
      </q-input>
    </div>

    <!-- Listado de tareas -->
    <q-list separator>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.titulo"
        @click="task.estado = !task.estado"
        :class="{ hecha: task.estado }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.estado"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.titulo }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.estado" side>
          <q-btn
            @click.stop="borrarTarea(index)"
            push
            color="white"
            dense
            text-color="negative"
            round
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <!-- Mensaje 'NO HAY TAREAS' -->
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="accent"></q-icon>
      <div class="text-h5 text-accent text-center">Sin tareas</div>
    </div>
  </q-page>
</template>

<!-- SCRIPT -->
<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      nuevaTarea: "",
      tasks: [],
    };
  },
  methods: {
    borrarTarea(index) {
      this.$q
        .dialog({
          title: "Cuidado!",
          message: "Desea eliminar la nota?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "La tarea ha sido eliminada",
            color: "accent",
          });
        });
    },
    agregarTarea() {
      this.tasks.push({
        titulo: this.nuevaTarea,
        estado: false,
      });

      this.nuevaTarea = "";
    },
  },
});
</script>

<!-- STYLE -->
<style lang="scss">
.hecha {
  .q-item__label {
    text-decoration: line-through;
    color: rgb(97, 97, 97);
  }
}
.puntas {
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}
.no-tasks {
  opacity: 0.5;
}
</style>
