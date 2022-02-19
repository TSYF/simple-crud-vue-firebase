```vue async
    <template>
	<div id="app">
		<h1>Todo App</h1>
    <h3 v-if="error">Por favor ingrese algo</h3>
		<form @submit.prevent="processForm">
			<input
				type="text"
				v-model.trim="textoTodo"
				placeholder="Ingrese tareas"
			/>
			<button type="submit">Agregar</button>
		</form>
		<h2 v-if="arrayTodos.length === 0">Empieza a agregar tareas!</h2>
		<ol>
			<li v-for="(item, index) of arrayTodos" :key="index">
				<span>{{ item.texto }}</span>
				<button @click="popItem(item.id)">Eliminar</button>
			</li>
		</ol>
	</div>
</template>

<script>
export default {
	data() {
		return {
			textoTodo: "",
			arrayTodos: [],
      error: false,
		};
	},
	methods: {
		async processForm() {
      try {
        const arrObj = {
          texto: this.textoTodo,
          id: Date.now(),
        };
  
        if (!this.textoTodo) return this.error = true;
  
        await fetch(
          `https://my-first-vue-app-bce76-default-rtdb.firebaseio.com/todos/${arrObj.id}.json`,
          {
            method: "put",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify(arrObj),
          }
        )

        this.arrayTodos.push(arrObj);
        this.textoTodo = "";

      } catch (err) {
        console.log(err);
      }
		},

		async popItem(val) {
			try {
				const res = await fetch(
					`https://my-first-vue-app-bce76-default-rtdb.firebaseio.com/todos/${val}.json`,
					{
						method: "delete",
					}
				);
				if (!res.ok) return console.log("Falló la eliminación");

				this.arrayTodos = this.arrayTodos.filter(
					(item) => item.id !== val
				);
			} catch (err) {
				console.log(err);
			}
		},
	},


	async created() {
		const res = await fetch(
			`https://my-first-vue-app-bce76-default-rtdb.firebaseio.com/todos.json`
		)
    const data = await res.json()

    for (const key in data) {
      console.log(data[key]);
      this.arrayTodos.push(data[key]);
    }
	},
};
</script>

<style></style>

```

```vue .then
	<template>
	<div id="app">
		<h2>Todo App</h2>
		<form @submit.prevent="processForm">
			<input
				type="text"
				v-model.trim="textoTodo"
				placeholder="Ingrese tareas"
			/>
			<button type="submit">Agregar</button>
		</form>
		<h2 v-if="arrayTodos.length === 0">Empieza a agregar tareas!</h2>
		<ol>
			<li v-for="(item, index) of arrayTodos" :key="index">
				<span>{{ item.texto }}</span>
				<button @click="popItem(item.id)">Eliminar</button>
			</li>
		</ol>
	</div>
</template>

<script>
export default {
	data() {
		return {
			textoTodo: "",
			arrayTodos: [],
      error: false,
		};
	},
	methods: {
		processForm() {
			const arrObj = {
				texto: this.textoTodo,
				id: Date.now(),
			};

      if (this.textoTodo) error = true;

			fetch(
				`https://my-first-vue-app-bce76-default-rtdb.firebaseio.com/todos/${arrObj.id}.json`,
				{
					method: "put",
					headers: {
						"Content-Type": "application/json",
					},
					body: JSON.stringify(arrObj),
				}
			)
				.then((res) => res.json())
				.then((data) => console.log(data))
				.catch((err) => console.log(err));

			this.arrayTodos.push(arrObj);
			this.textoTodo = "";
		},

		async popItem(val) {
			try {
				const res = await fetch(
					`https://my-first-vue-app-bce76-default-rtdb.firebaseio.com/todos/${val}.json`,
					{
						method: "delete",
					}
				);
				if (!res.ok) return console.log("Falló la eliminación");

				const data = await res.json();

				console.log(data);

				this.arrayTodos = this.arrayTodos.filter(
					(item) => item.id !== val
				);
			} catch (err) {
				console.log(err);
			}
		},
	},


	created() {
		fetch(
			`https://my-first-vue-app-bce76-default-rtdb.firebaseio.com/todos.json`
		)
			.then((res) => res.json())
			.then((data) => {
				for (const key in data) {
					console.log(data[key]);
					this.arrayTodos.push(data[key]);
				}
			});
	},
};
</script>

<style></style>

```