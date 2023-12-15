<template>
	<main class="login">
		<section class="forms">

			<!-- <form class="register" @submit.prevent="register">
				<h2>Register</h2>
				<input 
					type="email" 
					placeholder="Email address"
					v-model="register_form.email" />
				<input 
					type="password" 
					placeholder="Password" 
					v-model="register_form.password" />
				<input 
					type="submit" 
					value="Register" />
			</form> -->

			<form class="login" @submit.prevent="login">
				<h2>Login</h2>
				<input 
					class="input1"
					type="email" 
					placeholder="Email address"
					v-model="login_form.email" />
				<input 
					class="input1"
					type="password" 
					placeholder="Password" 
					v-model="login_form.password" />
				<input 
					class="input1"
					type="submit" 
					value="Login" />
				<div>1234!@#</div>
			</form>


		</section>
	</main>
</template>

<script>
import { ref } from 'vue'
import { useStore } from 'vuex'

export default {
	setup () {
		const login_form = ref({});
		const register_form = ref({});
		const store = useStore();

		const login = () => {
			store.dispatch('login', login_form.value);
		}

		const register = () => {
			store.dispatch('register', register_form.value);
		}

		return {
			login_form,
			register_form,
			login,
			register
		}
	}
}
</script>

<style>
.forms {
	display: flex;
	min-height: 100vh;
}

form {
	flex: 1 1 0%;
	padding: 8rem 1rem 1rem;
}

form.register {
	color: #FFF;
	background-color: rgb(57, 133, 246);
	background-image: linear-gradient(
		to bottom right,
		rgb(125, 165, 229) 0%,
		rgb(27, 151, 240) 100%
	);
	/* background-color: rgb(245, 66, 101);
	background-image: linear-gradient(
		to bottom right,
		rgb(245, 66, 101) 0%,
		rgb(189, 28, 60) 100%
	); */
}

h2 {
	font-size: 2rem;
	text-transform: uppercase;
	margin-bottom: 2rem;
}

.input1 {
	appearance: none;
	border: none;
	outline: none;
	background: none;

	display: block;
	width: 100%;
	max-width: 400px;
	margin: 0 auto;
	font-size: 1.5rem;
	margin-bottom: 2rem;
	/* padding: 0.5rem 0rem; */
	padding: 0.5rem 0.5rem;
}

.input1:not([type="submit"]) {
	opacity: 0.8;
	transition: 0.4s;
}

.input1:focus:not([type="submit"]) {
	opacity: 1;
}

.input1::placeholder {
	color: inherit;
}

form.register .input1:not([type="submit"]) {
	color: #FFF;
	border-bottom: 2px solid #FFF;
}

form.login .input1:not([type="submit"]) {
	color: #2c3e50;
	border-bottom: 2px solid #2c3e50;
}

form.login .input1[type="submit"] {
	/* background-color: rgb(245, 66, 101); */
	background-color: rgb(14, 187, 235);
	color: #FFF;
	font-weight: 700;
	padding: 1rem 2rem;
	border-radius: 0.5rem;
	cursor: pointer;
	text-transform: uppercase;
}

form.register .input1[type="submit"] {
	background-color: #FFF;
	/* color: rgb(245, 66, 101); */
	color: rgb(30, 175, 232);
	font-weight: 700;
	padding: 1rem 2rem;
	border-radius: 0.5rem;
	cursor: pointer;
	text-transform: uppercase;
}
</style>