<template>
	<div v-if="!boolModal">
		<div v-if="datos.length === 0">
			<div class="uk-alert-primary" uk-alert>
				<a class="uk-alert-close" uk-close></a>
				<p>No hay datos de usuarios.</p>
			</div>
		</div>
		<div v-else>
			<div class="uk-overflow-auto">
				<div class="uk-grid uk-child-width-1-1">
					<table class="uk-table uk-table-small uk-table-middle uk-table-striped uk-table-hover tablaCustomizada">
						<thead>
							<tr>
								<th>ID</th>
								<th>Nombre</th>
								<th>Dni</th>
								<th>Celular</th>
								<th>Correo</th>
								<th></th>
								<th></th>
							</tr>
						</thead>
						<tbody>
							<tr v-bind:key="index" v-for="(dato, index) in datos">
								<td>{{index}}</td>
								<td>{{dato.nombre}}</td>
								<td>{{dato.dni}}</td> 
								<td>{{dato.celular}}</td>
								<td>{{dato.correo}}</td>
								<td>
									<button class="uk-button uk-width-1-1 uk-button-primary" v-on:click="abrirModal(dato)">
										<b>EDITAR</b>
									</button>
								</td>
								<td>
									<button class="uk-button uk-width-1-1 uk-button-danger" v-on:click="fetchEliminarUsuario(dato._id)">
										<b>ELIMINAR</b>
									</button>
								</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>	
			<div>
				<button class="uk-button uk-width-1-1 btn-actualizar" v-on:click="actualizar">ACTUALIZAR</button>
			</div>
		</div>
	</div>
	<div v-else>
		<div class="formulario">
			<label>NOMBRE:</label><input v-model="datox.nombre" type="text" placeholder="Nombre"><br>
			<label>DNI:</label><input v-model="datox.dni" type="text" placeholder="DNI"><br>
			<label>CELULAR:</label><input v-model="datox.celular" type="text" placeholder="Celular"><br>
			<label>CORREO:</label><input v-model="datox.correo" type="text" placeholder="Correo"><br>
			<label>DIRECCION:</label><input v-model="datox.direccion" type="email" placeholder="Dirección"><br>
			<label>DEPARTAMENTO:</label><input v-model="datox.departamento" type="text" placeholder="Departamento"><br>
			<label>PROVINCIA:</label><input v-model="datox.provincia" type="text" placeholder="Provincia"><br>
			<label>DISTRITO:</label><input v-model="datox.distrito" type="text" placeholder="Distrito"><br>
			<label>COMENTARIO:</label><input v-model="datox.comentario" type="text" placeholder="Comentario"><br>
			<button v-on:click="fetchEditarUsuario">Guardar</button>
		</div>
	</div>
</template>

<script>
	export default{
        props: {
            datos: {
                type: Array,
                required: true
            }
        },
		data(){
			return{
				datox : [],
				boolModal : false
			}
		},
		methods:{
			fetchEliminarUsuario: function(idu){
				const data = {
					id : idu
				};
				fetch("http://localhost:8000/deleteUser",{
					method : 'POST',
					headers : {
						'Accept' : '*/*',
						'Content-Type' : 'application/json; charset=UTF-8',
					},
					body : JSON.stringify(data)
                })
                .then((response) =>{
					return response;
				})
				.then((result) => {
                    console.log(result);
                    console.log("evento lanzado ELIMINADO!");
                    this.$emit('created');
				});
			},
			abrirModal: function(datox){
				this.datox = datox;
				this.boolModal = true;
			},
			cerrarModal: function(){
				this.boolModal = false;
			},
			fetchEditarUsuario: function(){
				const data = {
                    id : this.datox._id,
					nombre: this.datox.nombre,
					dni : this.datox.dni,
					celular : this.datox.celular,
					correo : this.datox.correo,
					direccion : this.datox.direccion,
					departamento : this.datox.departamento,
					provincia : this.datox.provincia,
					distrito : this.datox.distrito,
					comentario : this.datox.comentario
				};
				fetch("http://localhost:8000/updateUser",{
					method : 'POST',
					headers : {
						'Accept' : '*/*',
						'Content-Type' : 'application/json; charset=UTF-8',
					},
					body : JSON.stringify(data)
                })
                .then((response) =>{
					return response;
				})
				.then((result) => {
                    console.log(result);
					console.log("evento lanzado EDITADO!");
					this.cerrarModal();
				});
			},
			actualizar: function(){
				console.log("evento lanzado ACTUALIZANDO!");
                this.$emit('created');
			}
        },
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

	label{
		width: 30px;
	}

	input{
		width: 30vh;
	}

	.tablaCustomizada{
		background-color: white;
		color: black; 
	}

	th {
		background-color: rgb(28, 30, 116);
		color: white; 
	}

	.btn-actualizar{
		transition: background-color 0.5s ease, color 0.5s ease;;
	}
	
	.btn-actualizar:hover{
		background-color: rgb(0, 255, 149);
		color: black;
	}
</style>