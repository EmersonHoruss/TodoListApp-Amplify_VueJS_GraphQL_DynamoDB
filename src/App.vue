<style scoped>
* {
  font-family: "Roboto", sans-serif;
}

.my-main {
  width: 100vw;
  display: grid;
  gap: 4rem;
  padding: 2rem;
  grid-template: "form table table";
}

.my-form {
  grid-area: form;
}

.my-table {
  grid-area: table;
  width: 100%;
  display: grid;
  grid-template:
    "my_header"
    "my_wrapper_table";
  grid-template-rows: 60px auto;
}

.my-header {
  grid-area: my_header;
}

.my-wrapper-table {
  width: 100%;
  overflow-x: scroll;
  grid-area: my_wrapper_table;
}

input {
  width: 100%;
}

.my-title {
  font-size: 1.5rem;
  font-weight: 500;
  margin-bottom: 1rem;
}

.no-margin-bottom {
  margin-bottom: 0;
}

.name,
.description {
  margin-bottom: 1rem;
}

.my-textarea {
  line-height: 1.7rem !important;
  width: 100%;
}

label {
  display: block;
  font-size: 1.05rem;
  margin-bottom: 0.2rem;
  color: var(--gray-color);
}

.my-input,
.my-textarea {
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid var(--border-color);
  border-radius: 0.3rem;
}

.my-input:focus,
.my-textarea:focus {
  border: 1px solid var(--primary-color) !important;
  outline: 0.5px solid var(--primary-color) !important;
}

button {
  width: 9rem;
  padding: 0.8rem;
  font-size: 1rem;
  border-radius: 2rem;
  border: 0px;
  color: var(--primary-color);
  background-color: transparent;
  border: 1px solid var(--border-color);
  font-weight: 500;
}

button:hover {
  background-color: var(--primary-hover-color);
}

table {
  border-spacing: 0;
  width: 100%;
}

th,
td {
  padding: 1rem;
  border-bottom: 1px solid var(--border-color);
  text-align: left;
}

th {
  font-weight: bold;
}

td input {
  width: auto;
}

tr {
  border-bottom: 1px solid var(--gray-color);
}

td:first-child,
th:first-child {
  width: 5rem;
}

svg {
  border-right: 1rem;
}

.options {
  width: 7rem;
}

.option {
  display: inline;
  margin-right: 1rem;
}

.amount {
  font-size: 0.8rem;
  margin-bottom: 0.2rem;
  color: var(--gray-color);
}

.tabs {
  display: flex;
  border-bottom: 1px solid blue;
}

.tab {
  padding: 0.5rem;
}

.bold-text {
  font-weight: 500;
}

.my-flex {
  display: flex;
  gap: 1rem;
}

.my-justify-content-space-between {
  justify-content: space-between;
}

.my-align-items-center {
  align-items: center;
}

.my-justify-content-end {
  justify-content: end;
}

.my-full {
  width: 100%;
}

.dangerous {
  color: var(--dangerous-color);
}

@media (max-width: 1000px) {
  .my-main {
    grid-template: "table";
  }
  .my-form {
    display: none;
  }
}

@media (min-width: 1000px) {
  .option .add {
    display: none;
  }
}

@media (min-width: 1250px) {
  .my-main {
    padding: 2rem 4rem;
  }
}

@media (min-width: 1500px) {
  .my-main {
    padding: 2rem 10rem;
  }
}

@media (max-width: 825px) {
  .my-main {
    gap: 2rem;
  }
}
</style>

<template>
  <!-- <div ></div> -->
  <div id="app" class="my-main">
    <div class="my-form">
      <form>
        <div class="my-title">Create Todo</div>

        <div class="name">
          <label for="name">Name:</label>
          <input
            type="text"
            v-model="nameCreate"
            placeholder="Program a list todo app."
            name="name"
            class="my-input"
          />
        </div>

        <div class="description">
          <label for="description">Description:</label>
          <textarea
            v-model="descriptionCreate"
            placeholder="App should be implemented using Amplify, VueJS, GraphQL and DynamoDB."
            name="description"
            rows="3"
            cols="32"
            class="my-textarea"
          >
          </textarea>
        </div>

        <div class="my-flex my-justify-content-end">
          <button type="button" v-on:click="createTodo">Save</button>
        </div>
      </form>
    </div>

    <div class="my-table">
      <div class="my-header">
        <div
          class="my-flex my-justify-content-space-between align-items-center"
        >
          <div class="my-title no-margin-bottom">Todos</div>
          <div
            class="option"
            data-bs-toggle="modal"
            data-bs-target="#modalCreate"
          >
            <svg
              version="1.1"
              id="Capa_1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              viewBox="0 0 223.574 223.574"
              style="enable-background: new 0 0 223.574 223.574"
              xml:space="preserve"
              width="1rem"
              height="1rem"
              class="add"
            >
              <polygon
                points="223.574,104.287 119.287,104.287 119.287,0 104.287,0 104.287,104.287 0,104.287 0,119.287 104.287,119.287 
		104.287,223.574 119.287,223.574 119.287,119.287 223.574,119.287 	"
              />
            </svg>
          </div>
        </div>
        <div class="amount">{{ todos.length + " todos" }}</div>
      </div>

      <!-- <div class="filters">
        <div class="tabs">
          <div class="tab" v-on:click="">All</div>
          <div class="tab" v-on:click="">Done</div>
          <div class="tab" v-on:click="">Pending</div>
        </div>
      </div> -->

      <div class="my-wrapper-table">
        <table>
          <tr>
            <th>#</th>
            <th>Name</th>
            <th>Description</th>
            <th>Done</th>
            <th>Options</th>
          </tr>
          <tr v-if="isLoading">
            <td colspan="5">Retrieving data</td>
          </tr>

          <tr v-if="!todos.length && !isLoading">
            <td colspan="5">Add todos, no data found.</td>
          </tr>

          <tr v-for="(todo, index) in todos" :key="todo.id">
            <td>{{ index + 1 }}</td>
            <td>{{ todo.name }}</td>
            <td>{{ todo.description }}</td>
            <td><input type="checkbox" /></td>
            <td class="options">
              <div
                class="option"
                v-on:click="openModal(todo)"
                data-bs-toggle="modal"
                data-bs-target="#modalConfirm"
              >
                <svg
                  version="1.1"
                  id="Capa_1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                  x="0px"
                  y="0px"
                  width="1.2rem"
                  height="1.2rem"
                  viewBox="0 0 482.428 482.429"
                  style="enable-background: new 0 0 482.428 482.429"
                  xml:space="preserve"
                >
                  <path
                    d="M381.163,57.799h-75.094C302.323,25.316,274.686,0,241.214,0c-33.471,0-61.104,25.315-64.85,57.799h-75.098
			c-30.39,0-55.111,24.728-55.111,55.117v2.828c0,23.223,14.46,43.1,34.83,51.199v260.369c0,30.39,24.724,55.117,55.112,55.117
			h210.236c30.389,0,55.111-24.729,55.111-55.117V166.944c20.369-8.1,34.83-27.977,34.83-51.199v-2.828
			C436.274,82.527,411.551,57.799,381.163,57.799z M241.214,26.139c19.037,0,34.927,13.645,38.443,31.66h-76.879
			C206.293,39.783,222.184,26.139,241.214,26.139z M375.305,427.312c0,15.978-13,28.979-28.973,28.979H136.096
			c-15.973,0-28.973-13.002-28.973-28.979V170.861h268.182V427.312z M410.135,115.744c0,15.978-13,28.979-28.973,28.979H101.266
			c-15.973,0-28.973-13.001-28.973-28.979v-2.828c0-15.978,13-28.979,28.973-28.979h279.897c15.973,0,28.973,13.001,28.973,28.979
			V115.744z"
                  />
                  <path
                    d="M171.144,422.863c7.218,0,13.069-5.853,13.069-13.068V262.641c0-7.216-5.852-13.07-13.069-13.07
			c-7.217,0-13.069,5.854-13.069,13.07v147.154C158.074,417.012,163.926,422.863,171.144,422.863z"
                  />
                  <path
                    d="M241.214,422.863c7.218,0,13.07-5.853,13.07-13.068V262.641c0-7.216-5.854-13.07-13.07-13.07
			c-7.217,0-13.069,5.854-13.069,13.07v147.154C228.145,417.012,233.996,422.863,241.214,422.863z"
                  />
                  <path
                    d="M311.284,422.863c7.217,0,13.068-5.853,13.068-13.068V262.641c0-7.216-5.852-13.07-13.068-13.07
			c-7.219,0-13.07,5.854-13.07,13.07v147.154C298.213,417.012,304.067,422.863,311.284,422.863z"
                  />
                </svg>
              </div>

              <div
                class="option"
                v-on:click="openModal(todo)"
                data-bs-toggle="modal"
                data-bs-target="#modalUpdate"
              >
                <svg
                  width="1rem"
                  height="1rem"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    fill="none"
                    stroke="#000"
                    stroke-width="2"
                    d="M1.7507,16.0022 C3.3517,20.0982 7.3367,23.0002 11.9997,23.0002 C18.0747,23.0002 22.9997,18.0752 22.9997,12.0002 M22.2497,7.9982 C20.6487,3.9012 16.6627,1.0002 11.9997,1.0002 C5.9247,1.0002 0.9997,5.9252 0.9997,12.0002 M8.9997,16.0002 L0.9997,16.0002 L0.9997,24.0002 M22.9997,0.0002 L22.9997,8.0002 L14.9997,8.0002"
                  />
                </svg>
              </div>
            </td>
          </tr>
        </table>
      </div>
    </div>

    <!-- Modal Create -->
    <div class="modal fade" id="modalCreate" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5">Todo</h1>

            <div class="option">
              <svg
                width="1.2rem"
                height="1.2rem"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg"
                fill="red"
                color="red"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="feather feather-x"
                data-bs-dismiss="modal"
              >
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </div>
          </div>
          <div class="modal-body">
            <form>
              <div class="name">
                <label for="name">Name:</label>
                <input
                  type="text"
                  v-model="name"
                  placeholder="Program a list todo app."
                  name="name"
                  class="my-input"
                />
              </div>

              <div class="description">
                <label for="description">Description:</label>
                <textarea
                  v-model="description"
                  placeholder="App should be implemented using Amplify, VueJS, GraphQL and DynamoDB."
                  name="description"
                  rows="3"
                  cols="32"
                  class="my-textarea"
                >
                </textarea>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              v-on:click="updateTodo"
              data-bs-dismiss="modal"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal Update -->
    <div class="modal fade" id="modalUpdate" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5">Todo</h1>

            <div class="option">
              <svg
                width="1.2rem"
                height="1.2rem"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg"
                fill="red"
                color="red"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="feather feather-x"
                data-bs-dismiss="modal"
              >
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </div>
          </div>
          <div class="modal-body">
            <form>
              <div class="name">
                <label for="name">Name:</label>
                <input
                  type="text"
                  v-model="name"
                  placeholder="Program a list todo app."
                  name="name"
                  class="my-input"
                />
              </div>

              <div class="description">
                <label for="description">Description:</label>
                <textarea
                  v-model="description"
                  placeholder="App should be implemented using Amplify, VueJS, GraphQL and DynamoDB."
                  name="description"
                  rows="3"
                  cols="32"
                  class="my-textarea"
                >
                </textarea>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              v-on:click="updateTodo"
              data-bs-dismiss="modal"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal Confirm -->
    <div class="modal fade" id="modalConfirm" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5">Confirm message</h1>

            <div class="option">
              <svg
                width="1.2rem"
                height="1.2rem"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg"
                fill="red"
                color="red"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="feather feather-x"
                data-bs-dismiss="modal"
              >
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </div>
          </div>
          <div class="modal-body">
            <p class="bold-text">Are you sure to delete the below todo?</p>
            <div class="my-flex">
              <p class="bold-text">Name:</p>
              <p>{{ name }}</p>
            </div>
            <div class="my-flex">
              <p class="bold-text">Description:</p>
              <p>{{ description }}</p>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" data-bs-dismiss="modal">No</button>
            <button
              type="button"
              v-on:click="deleteTodo"
              data-bs-dismiss="modal"
              class="dangerous"
            >
              Yes
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Amplify, API, graphqlOperation } from "aws-amplify";
import awsconfig from "./aws-exports";
Amplify.configure(awsconfig);
import { createTodo } from "./graphql/mutations";
import { updateTodo } from "./graphql/mutations";
import { deleteTodo } from "./graphql/mutations";
import { listTodos } from "./graphql/queries";

export default {
  name: "App",
  async created() {
    this.getTodos();
  },
  data() {
    return {
      id: "",
      name: "",
      description: "",
      nameCreate: "",
      descriptionCreate: "",
      todos: [],
      isLoading: true,
    };
  },
  methods: {
    openModal: function (todo) {
      this.name = todo.name;
      this.description = todo.description;
      this.id = todo.id;
    },

    async updateTodo() {
      if (!this.name) return;
      const todo = {
        id: this.id,
        name: this.name,
        description: this.description,
      };
      await API.graphql({
        query: updateTodo,
        variables: { input: todo },
      });
      this.getTodos();
    },

    async deleteTodo() {
      const todo = {
        id: this.id,
      };
      await API.graphql({
        query: deleteTodo,
        variables: { input: todo },
      });
      this.getTodos();
    },

    async createTodo() {
      if (!this.nameCreate) return;
      const todo = {
        name: this.nameCreate,
        description: this.descriptionCreate,
      };
      this.todos = [...this.todos, todo];
      await API.graphql({
        query: createTodo,
        variables: { input: todo },
      });
      this.nameCreate = "";
      this.descriptionCreate = "";
    },

    async getTodos() {
      const todos = await API.graphql({
        query: listTodos,
      });
      this.isLoading = false;
      this.todos = todos.data.listTodos.items;
    },
  },
};
</script>
