<template>
    <v-main>
      <h1 class="text-center display-4">Lista de tareas</h1>
      <hr />
    <v-container class="mt-5">
        <v-row justify="center">
            <v-col cols="8">
                <v-card class="pa-2" elevation="1" tile>
                    <v-list-item>
                        <v-text-field 
                            label="Agregar tarea"
                            v-model="tarea"
                            :loading="loading"
                            :disabled="loading"
                            @keydown.enter="agregarTarea"
                        ></v-text-field>
                        <v-list-item-action>
                            <v-btn color="green" dark @click="agregarTarea">Agregar</v-btn>
                        </v-list-item-action>
                    </v-list-item>
                    <br />
                    <h3 class="text-center mb-5" v-if="listaTareas.length === 0">No hay tareas para realizar</h3>
                    <template v-for="(tarea, index) in listaTareas">
                    <v-card elevation="0" outlined :key="index" class="mb-3">
                        <v-list height="70">
                            <v-list-item class="text-center">
                                <v-list-item-icon style="cursor: pointer;" @click="editarTarea(tarea, index)">
                                    <v-icon v-if="!tarea.estado" class="btn-succes-hover" >mdi-checkbox-blank-circle-outline</v-icon>
                                    <v-icon v-else color="green" style="transition: all 001ms ease">mdi-checkbox-marked-circle</v-icon>
                                </v-list-item-icon>
                                <v-list-item-title>
                                    {{ tarea.nombre }}
                                </v-list-item-title>
                                <v-list-item-icon style="cursor: pointer;" @click="eliminarTarea(tarea.id, index)">
                                    <v-icon class="btn-trash-hover">mdi-trash-can</v-icon>
                                </v-list-item-icon>
                            </v-list-item>
                        </v-list>
                    </v-card>
                    </template>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
  </v-main>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Tarea',
    data: () => ({
        tarea: '',
        listaTareas: [],
        loading: false,
        btnLoading: true,
    }),
    methods: {
        agregarTarea(){
            this.loading = true;
            const tarea = { nombre: this.tarea, estado: false };
            // this.listaTareas = [ tarea, ...this.listaTareas ];
            axios.post(`http://localhost:62439/api/Tarea/`, tarea).then(response => {
                this.obtenerTareas();
            }).catch(console.error);
            this.tarea = '';
            this.loading = false;
        },
        eliminarTarea(id, index){
            this.btnLoading = true;
            this.listaTareas.splice(index, 1);
            axios.delete(`http://localhost:62439/api/Tarea/${id}`).then(() => {
            }).catch(console.error);
            this.btnLoading = false;
        },
        editarTarea(tarea, index){
            this.btnLoading = true;
            axios.put(`http://localhost:62439/api/Tarea/${tarea.id}`, tarea).then(() => {
                this.listaTareas[index].estado = !tarea.estado;
            }).catch(console.error);
            this.btnLoading = true;
        },
        obtenerTareas(){
            this.btnLoading = true;
            axios.get("http://localhost:62439/api/Tarea").then(response => {
                this.listaTareas = response.data;
            }).catch(console.error);
            this.btnLoading = true;
        }
    },
    created(){
        this.obtenerTareas();
    },
}
</script>

<style scoped>
.btn-trash-hover:hover{
    color: red;
    transition: all 100ms ease;
}
.btn-succes-hover:hover{
    color: green;
    transition: all 100ms ease;
}
</style>