<template>
   <layout-div>
        <h2 class="text-center mt-5 mb-3">Show Project</h2>
        <div class="card">
            <div class="card-header">
                <router-link 
                    class="btn btn-outline-info float-right"
                    to="/">View All Projects
                </router-link>
            </div>
            <div class="card-body">
                <b className="text-muted">Name:</b>
                <p>{{project.name}}</p>
                <b className="text-muted">Nickname:</b>
                <p>{{project.nickname}}</p>
                <b className="text-muted">Birthday:</b>
                <p>{{project.birthday}}</p>
                <b className="text-muted">KeyAttribute:</b>
                <p>{{project.keyAttribute}}</p>
            </div>

            <div class="card-body">
                <b className="text-muted">Strength:</b>
                <p>{{project.strength}}</p>
                <b className="text-muted">Dexterity:</b>
                <p>{{project.dexterity}}</p>
                <b className="text-muted">Constitution:</b>
                <p>{{project.constitution}}</p>
                <b className="text-muted">Intelligence:</b>
                <p>{{project.intelligence}}</p>
                <b className="text-muted">Wisdom:</b>
                <p>{{project.wisdom}}</p>
                <b className="text-muted">Charisma:</b>
                <p>{{project.charisma}}</p>
            </div>

            <div class="card-body">
              <b className="text-muted">Weapons:</b>
              <table class="table table-bordered">
                        <thead>
                            <tr>
                                <th>Name</th>
                                <th>Mod</th>
                                <th>Attr</th>
                                <th>Equipped</th>
                            </tr>
                        </thead>
                        <tbody>
                          <tr v-for="weapon in weapons" :key="weapon.name">
                            <td>{{weapon.name}}</td>
                            <td>{{weapon.mod}}</td>
                            <td>{{weapon.attr}}</td>
                            <td>{{weapon.equipped}}</td>
                          </tr>
                        </tbody>
                </table>
            </div>
        </div>
   </layout-div>
</template>
 
<script>
import axios from 'axios';
import LayoutDiv from '../LayoutDiv.vue';
import Swal from 'sweetalert2'
 
export default {
  name: 'ProjectShow',
  components: {
    LayoutDiv,
  },
  data() {
    return {
      project: {
        name: '',
        nickname: '',
        birthday: '',
        keyAttribute: '',
        strength: '',
        dexterity: '',
        constitution: '',
        intelligence: '',
        wisdom: '',
        charisma: '',
      },
      isSaving:false,
      weapons:[]
    };
  },
  created() {
    const id = this.$route.params.id;
    axios.get(`/api/knights/${id}`)
    .then(response => {
        let projectInfo = response.data
        this.project.name = projectInfo.name
        this.project.nickname = projectInfo.nickname
        this.project.birthday = projectInfo.birthday
        this.project.keyAttribute = projectInfo.keyAttribute
        this.weapons = projectInfo.weapons
        this.project.strength = projectInfo.attributes.strength
        this.project.dexterity = projectInfo.attributes.dexterity
        this.project.constitution = projectInfo.attributes.constitution
        this.project.intelligence = projectInfo.attributes.intelligence
        this.project.wisdom = projectInfo.attributes.wisdom
        this.project.charisma = projectInfo.attributes.charisma

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
    })
  },
  methods: {
     
  },
};
</script>