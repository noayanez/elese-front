<template>
	<div class="uk-padding">
		<div class="uk-text-large uk-text-bold uk-text-center titulo">ELESE TE DA LA BIENVENIDA A LA FERIA EXCON 2019</div >
		<div class="uk-text-small uk-text-center subtitulo">Completa el formulario y obtén tu descuento ahora</div>
		<br>
		<div class="uk-flex-center uk-grid uk-child-width-1-2@m uk-child-width-1-1">	
			<div>
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
				<div v-if="tipo==='1'">
					<fieldset class="uk-fieldset">
						<div class="uk-margin">
							<input class="uk-input" v-model="nombre" type="text" placeholder="Nombre completo">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="dni" type="text" placeholder="DNI">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="celular" type="text" placeholder="Celular">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="correo" type="text" placeholder="Correo">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="direccion" type="email" placeholder="Dirección">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="departamento" type="text" placeholder="Departamento">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="provincia" type="text" placeholder="Provincia">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="distrito" type="text" placeholder="Distrito">
						</div>
						<div class="uk-margin">
							<textarea class="uk-textarea" rows="3" v-model="comentario" type="text" placeholder="Comentario"></textarea>
						</div>
						<div class="uk-grid uk-child-width-1-1@m uk-flex-center uk-margin uk-text-center">
							<div class="uk-width-1-1">
								<button class="uk-button btn-enviar" v-on:click="fetchInsertarUsuario">OBTENER DESCUENTO</button>
							</div>
						</div>
					</fieldset>
				</div>
				<div v-else>
					<fieldset class="uk-fieldset">
						<div class="uk-margin">
							<input class="uk-input" v-model="nombre" type="text" placeholder="Nombre de contacto">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="razon" type="text" placeholder="Razon">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="ruc" type="text" placeholder="RUC">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="celular" type="text" placeholder="Celular de contacto">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="correo" type="text" placeholder="Correo de contacto">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="direccion" type="email" placeholder="Dirección">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="departamento" type="text" placeholder="Departamento">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="provincia" type="text" placeholder="Provincia">
						</div>
						<div class="uk-margin">
							<input class="uk-input" v-model="distrito" type="text" placeholder="Distrito">
						</div>
						<div class="uk-margin">
							<textarea class="uk-textarea" rows="3" v-model="comentario" type="text" placeholder="Comentario"></textarea>
						</div>
						<div class="uk-grid uk-child-width-1-1@m uk-flex-center uk-margin uk-text-center">
							<div class="uk-width-1-1">
								<button class="uk-button btn-enviar" v-on:click="fetchInsertarEmpresa">OBTENER DESCUENTO</button>
							</div>
						</div>
					</fieldset>
				</div>	
			</div>		
		</div>
	</div>
</template>


<script>
	export default{
		data(){
			return{
				tipo : '1',
				nombre : '',
				razon : '',
				ruc : '',
				dni : '',
				celular : '',
				correo : '',
				direccion : '',
				departamento : '',
				provincia : '',
				distrito : '',
				comentario : ''
			}
		},
		methods:{
			fetchInsertarUsuario: function(){
				const data = {
					nombre: this.nombre,
					dni : this.dni,
					celular : this.celular,
					correo : this.correo,
					direccion : this.direccion,
					departamento : this.departamento,
					provincia : this.provincia,
					distrito : this.distrito,
					comentario : this.comentario
				};
				console.log(data);
				if(this.nombre !== "" && this.correo !== "" && this.comentario !== ""){
					console.log("GO fetchInsertarUsuario");
					fetch("http://localhost:8000/insertUser",{
						method : 'POST',
						headers : {
							'Accept' : '*/*',
							'Content-Type' : 'application/json; charset=UTF-8',
						},
						body : JSON.stringify(data)
					})
					.then((response) =>{
						return response
					})
					.then((result) => {
						console.log(result);
					});
				}else{
					console.log("Faltan rellenar campos");
				}
			},
			fetchInsertarEmpresa: function(){
				const data = {
					nombre: this.nombre,
					razon : this.razon,
					ruc : this.ruc,
					celular : this.celular,
					correo : this.correo,
					direccion : this.direccion,
					departamento : this.departamento,
					provincia : this.provincia,
					distrito : this.distrito,
					comentario : this.comentario
				};
				console.log(data);
				if(this.razon !== "" && this.correo !== "" && this.comentario !== ""){
					console.log("GO fetchInsertarEmpresa");
					fetch("http://localhost:8000/insertCompany",{
						method : 'POST',
						headers : {
							'Accept' : '*/*',
							'Content-Type' : 'application/json; charset=UTF-8',
						},
						body : JSON.stringify(data)
					})
					.then((response) =>{
						return response
					})
					.then((result) => {
						console.log(result);
					});
				}else{
					console.log("Faltan rellenar campos");
				}
			},
			cambiarVista(tipo){
				this.tipo = tipo;
			}
		}
	}
</script>

<style>
	.formulario{
		text-align: center;
	}

	.titulo{
		color: white;
	}

	.subtitulo{
		color: white;
	}

	.tagger{
		text-align: center;
		border: solid;
		border-width: 2px;
		margin: 0px;
		border-color: white;
		background-color: #00BCD4;
		color: white;
		font-size: 20px;
	}
	
	.tagger:hover{
		color: rgb(3, 3, 75);
		background-color: rgb(97, 225, 241);
	}

	.tagger-active{
		font-size-adjust: inherit;
		text-align: center;
		border: solid;
		border-width: 2px;
		margin: 0px;
		border-color: white;
		border-bottom-color: rgb(3, 3, 75);
		opacity: 50%;
		color: rgb(3, 3, 75);
		background-color: white;
		font-size: 20px;
	}

	.btn-enviar{
		background-color: rgb(18, 126, 168);
		border: solid;
		border-width: 2px;
		border-color: white;
		color: white;
		font-weight: bold;
		font-size: 20px;
	}

	.btn-enviar:hover{
		background-color: rgb(97, 225, 241);
		border: solid;
		border-width: 2px;
		border-color: white;
		color: rgb(3, 3, 75);
	}

</style>
