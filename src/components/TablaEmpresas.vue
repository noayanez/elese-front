<template>
	<div v-if="!boolModal">
		<div v-if="datos.length === 0">
			<div class="uk-alert-primary" uk-alert>
				<a class="uk-alert-close" uk-close></a>
				<p>No hay datos de empresas.</p>
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
								<th>Razon</th>
								<th>RUC</th>
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
								<td>{{dato.razon}}</td> 
								<td>{{dato.ruc}}</td> 
								<td>{{dato.celular}}</td>
								<td>{{dato.correo}}</td>
								<td>
									<button class="uk-button uk-width-1-1 uk-button-primary" v-on:click="abrirModal(dato)">
										<b>EDITAR</b>
									</button>
								</td>
								<td>
									<button class="uk-button uk-width-1-1 uk-button-danger" v-on:click="fetchEliminarEmpresa(dato._id)">
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
		<div class="uk-grid uk-child-width-1-3@m uk-child-width-1-2@s uk-child-width-1-1 uk-flex-center">
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Nombre:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.nombre" type="text" placeholder="Nombre de contacto">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Razon:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.razon" type="text" placeholder="Razon">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Ruc:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.ruc" type="text" placeholder="RUC">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Celular:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.celular" type="text" placeholder="Celular de contacto">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Correo:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.correo" type="text" placeholder="Correo de contacto">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Direccion:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.direccion" type="email" placeholder="Dirección">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Departamento:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.departamento" type="text" placeholder="Departamento">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Provincia:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.provincia" type="text" placeholder="Provincia">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Distrito:</label>
					</div>
					<div>
						<input class="uk-input uk-form-width-medium uk-form-small" v-model="datox.distrito" type="text" placeholder="Distrito">
					</div>
				</div>
			</div>
			<div class="uk-text-center uk-grid uk-child-width-1-1 pad3">
				<div class="uk-child-width-1-1">
					<div class="uk-text-center">
						<label class="uk-text-large labelx">Comentario:</label>
					</div>
					<div>
						<textarea row="3" class="uk-input uk-form-width-medium uk-form-small" v-model="datox.comentario" type="text" placeholder="Comentario"/>
					</div>
				</div>
			</div>
		</div>
		<div class="uk-margin uk-text-center">
			<button class="uk-button uk-button-secondary" v-on:click="fetchEditarEmpresa">Guardar</button>
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
			fetchEliminarEmpresa: function(idu){
				const data = {
					id : idu
				};
				fetch("http://localhost:8000/deleteCompany",{
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
			fetchEditarEmpresa: function(){
				const data = {
                    id : this.datox._id,
					nombre: this.datox.nombre,
					razon : this.datox.razon,
					ruc : this.datox.ruc,
					celular : this.datox.celular,
					correo : this.datox.correo,
					direccion : this.datox.direccion,
					departamento : this.datox.departamento,
					provincia : this.datox.provincia,
					distrito : this.datox.distrito,
					comentario : this.datox.comentario
				};
				fetch("http://localhost:8000/updateCompany",{
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
		transition: background-color 0.5s ease;
	}

	.btn-actualizar:hover{
		background-color: rgb(0, 255, 149);
	}

	.labelx{
		color: white;
	}

	.pad3{
		margin-top: 15px;
	}
</style>