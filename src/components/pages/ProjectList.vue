<template>
  <layout-div>
        <div class="container">
            <h2 class="text-center mt-5 mb-3">Knights Manager</h2>
            <div class="card">
                <div class="card-header">
                    <router-link to="/create"
                        class="btn btn-outline-primary"
                        >Create New Knight
                    </router-link>
                </div>
                <div class="card-body">
             
                    <table class="table table-bordered">
                        <thead>
                            <tr>
                                <th>Nome</th>
                                <th>Idade</th>
                                <th>Armas</th>
                                <th>Atributo</th>
                                <th>Ataque</th>
                                <th>Exp</th>
                                <th width="240px">Action</th>
                            </tr>
                        </thead>
                        <tbody>
                             
                            <tr v-for="project in projects" :key="project.id">
                                <td>{{project.nome}}</td>
                                <td>{{project.idade}}</td>
                                <td>{{project.armas}}</td>
                                <td>{{project.atributo}}</td>
                                <td>{{project.ataque}}</td>
                                <td>{{project.exp}}</td>
                                <td>
                                    <router-link :to="`/show/${project.id}`" class="btn btn-outline-info mx-1">Show</router-link>
                                    <router-link :to="`/edit/${project.id}`" class="btn btn-outline-success mx-1">Edit</router-link>
                                    <button 
                                        @click="handleDelete(project.id)"
                                        className="btn btn-outline-danger mx-1">
                                        Delete
                                    </button>
                                </td>
                            </tr>
                                 
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </layout-div>
</template>
 
<script>
import axios from 'axios';
import LayoutDiv from '../LayoutDiv.vue';
import Swal from 'sweetalert2'
 
export default {
  name: 'ProjectList',
  components: {
    LayoutDiv,
  },
  data() {
    return {
      projects:[]
    };
  },
  created() {
    this.fetchProjectList();
  },
  methods: {
    fetchProjectList() {
      axios.get('/api/knights')
        .then(response => {
            this.projects = response.data;
            return response
        })
        .catch(error => {
          return error
        });
    },
    handleDelete(id){
        Swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!'
          }).then((result) => {
            if (result.isConfirmed) {
                axios.delete(`/api/Guerrero/knights/${id}`)
                .then( response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Project deleted successfully!',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    this.fetchProjectList();
                    return response
                })
                .catch(error => {
                    Swal.fire({
                         icon: 'error',
                        title: 'An Error Occured!',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    return error
                });
            }
          })
    }
  },
};
</script>