<template>
	<header>
		<div class="users">
			<div class="section">
				<h2 class="title">Novo Usuário</h2>
				<form @submit.prevent="createUser">
					<input type="text" placeholder="Nome" v-model="form.name">
					<input type="text" placeholder="Email" v-model="form.email">
					<button type="submit">Adicionar novo usuário</button>
				</form>
			</div>
			<div class="container">
				<section>
					<h5 class="title">Lista de Usuários</h5>
					<transition>
						<ul>
							<li v-for="user in users" :key="user.id">
								<p>{{user.name}}</p>
								<small>{{user.email}}</small>
								<a class="destroy" @click="deleteUser(user.id)"></a>
							</li>
						</ul>
					</transition>
				</section>
			</div>
		</div>
	</header>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "@/utils/axios";
import { User } from "@/models/User";

export default defineComponent({
	data() {
		return {
			users: [] as User[],
			form: {
				name: "",
				email: "",
			},
		};
	},
	methods: {
		async searchUsers() {
			try {
				const { data } = await axios.get("/users");
				this.users = data;
			} catch (error) {
				console.warn(error);
			}
		},
		async createUser() {
			try {
				const { data } = await axios.post("/users", this.form);

				this.users.push(data);

				this.form.name = "";
				this.form.email = "";
			} catch (error) {
				console.warn(error);
			}
		},
		async deleteUser(id: User["id"]) {
			try {
				const { data } = await axios.delete(`users/${id}`);
				const userIndex = this.users.findIndex(
					(user) => user.id === id
				);
				this.users.splice(userIndex, 1);
			} catch (error) {
				console.warn(error);
			}
		},
	},
	created() {
		this.searchUsers();
	},
});
</script>

<style scoped>
.container {
	margin: 4rem auto;
	max-width: 500px;
	width: 90%;
	display: grid;
	grid-gap: 2.5rem;
}

.title {
	font-size: 1.25rem;
	font-weight: bold;
	margin: 4rem 0 1rem 0;
}

form {
	display: grid;
	grid-gap: 1rem;
}

input {
	background: transparent;
	border: 1px solid #999fc6;
	border-radius: 1rem;
	padding: 0.6rem;
	outline: none;
	color: #e1e8ef;
}

input::placeholder {
	color: #999fc6;
}

button {
	background-color: #8257e6;
	color: #e1e8ef;
	border: none;
	border-radius: 1rem;
	padding: 0.6rem 1.5rem;
	width: max-content;
	transition: all 0.3s linear;
	outline: none;
	cursor: pointer;
	font-weight: 600;
	box-shadow: 0 0 5px 3px rgba(127, 45, 234, 0.3);
}

button:hover {
	background-color: #751bdc;
}

p {
	margin: 0;
}

ul {
	padding: 0;
	margin: 0;
	display: grid;
	grid-gap: 1rem;
}

li {
	background-color: #202024;
	padding: 1.2rem 1rem;
	border-radius: 1rem;
	position: relative;
	list-style: none;
	color: #8b98a8;
}

.destroy {
	background-color: #e65757;
	width: 24px;
	height: 24px;
	border-radius: 0.5rem;
	cursor: pointer;
	transition: all 0.2s linear;
	position: absolute;
	top: 50%;
	transform: translateY(-50%);
	right: 1.3rem;
}

.destroy:before,
.destroy:after {
	content: "";
	width: 3px;
	height: 13px;
	background-color: #ececf6;
	border-radius: 1rem;
	position: absolute;
	top: 50%;
	left: 50%;
}

.destroy:before {
	transform: translate(-50%, -50%) rotate(45deg);
}

.destroy:after {
	transform: translate(-50%, -50%) rotate(130deg);
}

.destroy:hover {
	background-color: #bb3535;
}

.v-enter {
	opacity: 0;
	transform: translate3d(-20px, 0, 0);
}

.v-enter-active {
	transition: all 0.3s;
}

@media (max-width: 2060px) {
	.section {
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	input {
		width: 500px;
	}
}

@media (max-width: 520px) {
	input {
		width: 400px;
	}
}
</style>