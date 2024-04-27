<template>
    <nav class="navbar bg-primary fixed-top">
        <a class="navbar-brand" @click="reset()" href="/" style="color:white;">App economía familiar VIC</a>
    </nav>
    <div>
        <h1>{{message}}</h1>
    </div>
    <div class="container" v-if="vermiembros">
        <br />
        <h3>Miembros o propiedades familiares con gastos</h3>
        <br />
        <br />
        <table class="table table-hover table-sm">
            <thead>
                <tr>
                    <th scope="col">ID</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Descripción</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="miembro in miembros" v-bind:key="miembro.id" @click="mostrarGastos(miembro.id, miembro.nombre)"> 
                    <th scope="row">{{miembro.id}}</th>
                    <td>{{miembro.nombre}}</td>
                    <td>{{miembro.descripcion}}</td>
                </tr>
            </tbody>
        </table> 
    </div>
    <div class="container" v-if="vergastos">
        <br />
        <h3>Gastos de {{ nombreMiembro }}</h3>
        <br />
        <br />
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">Concepto</th>
                    <th scope="col">Importe</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="gasto in gastos" v-bind:key="gasto.concepto"> 
                    <th scope="row">{{gasto.concepto}}</th>
                    <td><input v-model="gasto.importe"></td>
                </tr>
            </tbody>
        </table>
        <div>
            <button @click="guardarGastos()" class="btn btn-primary">Guardar</button>
        </div>
        <br />
        <br />
        <div class="alert alert-success alert-dismissible fade show" role="alert" v-if="veralerta">
            Los datos se han guardado correctamente!
            <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click="reset()">
                <span aria-hidden="true">&times;</span>
            </button>
        </div>
    </div>
</template>
<script>
    import axios from 'axios';
    export default {
        data() {
            return {
                miembros : axios.get('http://localhost:8080/vic-be/miembro')
                                .then(res => {
                                    this.miembros = res.data;
                                }),
                gastos : [],
                vermiembros : true,
                vergastos : false,
                veralerta: false,
                nombreMiembro : '',
                idMiembro : 0
            };    
        },
        methods: {
            mostrarGastos(idMiembro, nombreMiembro) {
                axios.get('http://localhost:8080/vic-be/gasto/' + idMiembro)
                    .then(res => {
                        this.gastos = res.data;
                        this.nombreMiembro = nombreMiembro;
                        this.idMiembro = idMiembro;
                    })
                this.vergastos = true;
                this.vermiembros = false;
            },
            guardarGastos(){
                axios.put('http://localhost:8080/vic-be/gasto/' + this.idMiembro, this.gastos)
                    .then(res => {
                        this.veralerta = res.data;
                    })
            },
            reset() {
                this.vergastos = false;
                this.veralerta = false;
                this.vermiembros = true;
                this.gastos = [];
                this.idMiembro = 0;
                this.nombreMiembro = '';
            }
        }
    }    
</script>
<style>
    h3 {
        margin: 40px 0 0;;
    }
    .closebtn {
        margin-left: 15px;
        color: white;
        font-weight: bold;
        float: right;
        font-size: 22px;
        line-height: 20px;
        cursor: pointer;
        transition: 0.3s;
    }

    .closebtn:hover {
        color: black;
    }
</style>