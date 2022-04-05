<template>
  <div id="task">
    <form @submit.prevent="addItem">
      <input type="text" placeholder="Tarefa de Hoje?" v-model="tarefa" />
      <button type="submit" @click="pendencia">Adicionar</button>
    </form>

    <Item :lista="tarefas" :delete="deleteTask" />

    <span v-show="tarefas.length > 0">
      Você tem
      <strong :class="{ pend: pendente }"> {{ tarefas.length }} </strong>
      tarefas pendentes.
    </span>
  </div>
</template>

<script>
import Item from "./Item.vue";
export default {
  name: "Task",
  data() {
    return {
      tarefa: "",
      tarefas: [],
      pendente: false,
    };
  },
  methods: {
    addItem() {
      if (this.tarefa !== "") {
        this.tarefas.push({
          text: this.tarefa,
          key: Date.now(),
        });
      } else {
        alert("Digite uma tarefa..");
        return;
      }
      this.tarefa = "";
      console.log(this.tarefas);
    },
    deleteTask(key) {
      let filtro = this.tarefas.filter((item) => {
        return item.key !== key;
      });

      return (this.tarefas = filtro);
    },
  },
  components: {
    Item,
  },
  watch: {
    tarefas: {
      deep: true,
      handler() {
        localStorage.setItem("tasks", JSON.stringify(this.tarefas));
        this.tarefas.length > 4
          ? (this.pendente = true)
          : (this.pendente = false);
      },
    },
  },
  created() {
    // é chamado automaticamente após a instancia do vue ser criada
    const minhaLista = localStorage.getItem("tasks");
    this.tarefas = JSON.parse(minhaLista) || [];
  },
};
</script>

<style scoped>
#task {
  max-width: 700px;
  background-color: #fff;
  border-radius: 4px;
  padding: 20px;
  margin: 20px auto;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

form {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
}
form button {
  cursor: pointer;
  background-color: #0f5959;
  border: none;
  border-radius: 4px;
  margin-left: 10px;
  padding: 0 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

input {
  flex: 1;
  border: 1px solid #eee;
  padding: 6px 10px;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.pend {
  color: red;
}
</style>