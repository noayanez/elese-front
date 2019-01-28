<template>
	<div class="uk-position-relative uk-margin-medium">
		<div v-if="!admin" class="fullHouse uk-text-center uk-padding">
			<br><br>
			<br><br>
			<div class="uk-flex-center uk-child-width-1-4@m uk-child-width-1-2@s uk-child-width-1-1 uk-text-center" uk-grid>
				<div class="uk-panel uk-panel-box uk-form">
                    <div class="uk-form-row uk-margin">
                        <input class="uk-width-1-1 uk-form-large" v-model="user" type="text" placeholder="Usuario">
                    </div>
                    <div class="uk-form-row uk-margin">
                        <input class="uk-width-1-1 uk-form-large" v-model="pass" type="text" placeholder="Contraseña">
                    </div>
                    <div class="uk-form-row uk-width-1-2@s uk-width-1-1 uk-align-center">
                        <button v-on:click="fetchAdmin" class="uk-width-1-1 uk-button uk-button-secondary">Entrar</button>
                    </div>
					<div v-if="alerta!==''" class="uk-form-row uk-width-1-1 uk-align-center">
						<div class="uk-alert-danger" uk-alert>
							<a class="uk-alert-close" v-on:click="cerrarAlerta" uk-close></a>
							<p>{{this.alerta}}</p>
						</div>
					</div>
                </div>
			</div>
		</div>
		<div v-else class="uk-padding">
			<div v-if="tipo==='1'">
				<div class="uk-align-center">
					<button class="uk-button tagger-active uk-width-1-2" v-on:click="cambiarVista('1')">Persona</button>
					<button class="uk-button tagger uk-width-1-2" v-on:click="cambiarVista('2')">Empresa</button>
				</div>
			</div>
			<div v-else>
				<div class="uk-align-center">
					<button class="uk-button tagger uk-width-1-2" v-on:click="cambiarVista('1')">Persona</button>
					<button class="uk-button tagger-active uk-width-1-2" v-on:click="cambiarVista('2')">Empresa</button>
				</div>
			</div>

			<div v-if="!cargado1 && this.tipo==='1'">
				{{fetchUsuarios()}}
			</div>
			<TablaUsuarios v-if="cargado1 && tipo==='1'" v-bind:datos="datosUsuarios" v-on:created="fetchUsuarios"></TablaUsuarios>

			<div v-if="!cargado2 && this.tipo==='2'">
				{{fetchEmpresas()}}
			</div>
			<TablaEmpresas v-if="cargado2 && tipo==='2'" v-bind:datos="datosEmpresas" v-on:created="fetchEmpresas"></TablaEmpresas>
		</div>
	</div>
</template>

<script>
	import TablaUsuarios from '@/components/TablaUsuarios.vue';
	import TablaEmpresas from '@/components/TablaEmpresas.vue';
	export default{
		components: {
			'TablaUsuarios' : TablaUsuarios,
			'TablaEmpresas' : TablaEmpresas
		},
		data(){
			return{
				tipo : '1',
				datosUsuarios : [],
				datosEmpresas : [],
				cargado1 : false,
				cargado2 : false,
				admin : false,
				user : '',
				pass : '',
				alerta : ''
			}
		},
		methods:{
			fetchUsuarios: function(){
				console.log("FETCHING ALL USUARIOS");
				fetch("http://localhost:8000/getUsers")
				.then((response) =>{
					return response.json()
				})
				.then((result) => {
					this.datosUsuarios = result.items;
					this.cargado1 = true;
				});
			},
			fetchEmpresas: function(){
				console.log("FETCHING ALL EMPRESAS");
				fetch("http://localhost:8000/getCompanies")
				.then((response) =>{
					return response.json()
				})
				.then((result) => {
					console.log(result);
					this.datosEmpresas = result.items;
					this.cargado2 = true;
				});
			},
			fetchAdmin: function(){
				const data = {
					user : this.user,
					pass : this.pass
				}
				console.log("FETCHING ADMIN");
				fetch("http://localhost:8000/isAdmin",{
					method : 'POST',
					headers : {
						'Accept' : '*/*',
						'Content-Type' : 'application/json; charset=UTF-8',
					},
					body : JSON.stringify(data)
				})
				.then((response) =>{
					return response.json();
				})
				.then((result) => {	
					console.log(result.items);				
					if(result.items.length > 0){
						this.admin = true;
					}else{
						this.alerta = "Usuario o contraseña incorrecta";
					}
				});
			},
			cambiarVista(tipo){
				this.tipo = tipo;
			},
			cerrarAlerta(){
				this.alerta = '';
			}
		}
	}
</script>

<style scoped>
	Header{
		background: lightblue;
		padding: 10px;
	}

	h1{
		color: #222;
		text-align: center;
	}

	table {
		border-collapse: collapse;
	}

	table, th, td {
		border: 1px solid black;
	}
	
	.tagger{
		text-align: center;
		border: solid;
		border-width: 2px;
		margin: 0px;
		border-color: white;
		background-color: rgb(28, 30, 116);
		color: white;
		font-size: 22px;
	}
	
	.tagger:hover{
		color: #283593;
		background-color: rgb(102, 210, 236);
	}

	.tagger-active{
		text-align: center;
		border: solid;
		border-width: 2px;
		margin: 0px;
		border-color: white;
		opacity: 50%;
		font-size: 22px;
		color: #283593;
		background-color: white;
	}

	.fullHouse{
		height: 60.3vh;
	}
</style>
