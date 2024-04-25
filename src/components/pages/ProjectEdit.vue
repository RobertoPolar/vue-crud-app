<template>
   <layout-div>
        <h2 class="text-center mt-5 mb-3">Edit Project</h2>
        <div class="card">
            <div class="card-header">
                <router-link 
                    class="btn btn-outline-info float-right"
                    to="/">View All Projects
                </router-link>
            </div>
            <div class="card-body">
                <form>
                  <div class="form-group">
                    <label htmlFor="name">Name</label>
                    <input 
                        v-model="project.name"
                        class="form-control"
                        id="name"
                        rows="3"
                        name="name">
                  </div>
                  <div class="form-group">
                    <label htmlFor="nickname">Nickname</label>
                    <input 
                        v-model="project.nickname"
                        class="form-control"
                        id="nickname"
                        rows="3"
                        name="nickname">
                  </div>
                  <div class="form-group">
                    <label htmlFor="birthday">Birthday</label>
                    <input 
                        v-model="project.birthday"
                        class="form-control"
                        id="birthday"
                        rows="3"
                        name="birthday"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="strength">Strength</label>
                    <input 
                        v-model="project.attributes.strength"
                        class="form-control"
                        id="strength"
                        rows="3"
                        name="strength"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="dexterity">Dexterity</label>
                    <input 
                        v-model="project.attributes.dexterity"
                        class="form-control"
                        id="dexterity"
                        rows="3"
                        name="dexterity"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="constitution">Constitution</label>
                    <input 
                        v-model="project.attributes.constitution"
                        class="form-control"
                        id="constitution"
                        rows="3"
                        name="constitution"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="intelligence">Intelligence</label>
                    <input 
                        v-model="project.attributes.intelligence"
                        class="form-control"
                        id="intelligence"
                        rows="3"
                        name="intelligence"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="wisdom">Wisdom</label>
                    <input 
                        v-model="project.attributes.wisdom"
                        class="form-control"
                        id="wisdom"
                        rows="3"
                        name="wisdom"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="charisma">Charisma</label>
                    <input 
                        v-model="project.attributes.charisma"
                        class="form-control"
                        id="charisma"
                        rows="3"
                        name="charisma"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="keyAttribute">KeyAttribute</label>
                    <input 
                        v-model="project.keyAttribute"
                        class="form-control"
                        id="keyAttribute"
                        rows="3"
                        name="keyAttribute"/>
                  </div>

                  <div class="form-group">
                    <h4>Weapons</h4>
                    <label htmlFor="name_weapon">Name</label>
                    <input 
                        v-model="weapon.name"
                        class="form-control"
                        id="name_weapon"
                        rows="3"
                        name="name_weapon"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="mod">Mod</label>
                    <input 
                        type="number"
                        v-model="weapon.mod"
                        class="form-control"
                        id="mod"
                        rows="3"
                        name="mod"/>
                  </div>

                  <div class="form-group">
                    <label htmlFor="attr">Attr</label>
                    <input 
                        v-model="weapon.attr"
                        class="form-control"
                        id="attr"
                        rows="3"
                        name="attr"/>
                  </div>
                  <div class="form-group">
                    <label htmlFor="equipped">Equipped</label>
                    <input 
                        type="checkbox"
                        v-model="weapon.equipped"
                        class="form-check-input"
                        id="equipped"
                        rows="3"
                        name="equipped"/>
                  </div>
                  <button 
                      @click="addWeapon()"
                      :disabled="isSaving"
                      type="button"
                      class="btn btn-outline-primary mt-3">
                      Add Weapon
                  </button>

                  <div class="form-group">
                    <table class="table table-bordered">
                        <thead>
                            <tr>
                                <th>Name</th>
                                <th>Mod</th>
                                <th>Attr</th>
                                <th>Equipped</th>
                                <th>Action</th>
                            </tr>
                        </thead>
                        <tbody>
                          <tr v-for="(weapon, index) in project.weapons" :key="weapon.name">
                            <td>{{weapon.name}}</td>
                            <td>{{weapon.mod}}</td>
                            <td>{{weapon.attr}}</td>
                            <td>{{weapon.equipped}}</td>
                            <td>
                              <button 
                                type="button"
                                @click="removeWeapon(index)"
                                className="btn btn-outline-danger mx-1">
                                Delete
                              </button>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                  </div>

                  <button 
                      @click="handleSave()"
                      :disabled="isSaving"
                      type="button"
                      class="btn btn-outline-primary mt-3">
                      Save Project
                  </button>
                </form>
            </div>
        </div>
   </layout-div>
</template>
 
<script>
import axios from 'axios';
import LayoutDiv from '../LayoutDiv.vue';
import Swal from 'sweetalert2'
 
export default {
  name: 'ProjectEdit',
  components: {
    LayoutDiv,
  },
  data() {
    return {
      project: {
        id: '',
        name: '',
        nickname: '',
        birthday: '',
        weapons: [
        ],
        attributes: {
          strength: 0,
          dexterity: 0,
          constitution: 0,
          intelligence: 0,
          wisdom: 0,
          charisma: 0
        },
        keyAttribute: '',
        
      },
      isSaving:false,
      weapon:{
          name: '',
          mod: 0,
          attr: '',
          equipped: false
      }
    };
  },
  created() {
    const id = this.$route.params.id;
    axios.get(`/api/knights/${id}`)
    .then(response => {
        let projectInfo = response.data
        this.project.id = projectInfo.id
        this.project.name = projectInfo.name
        this.project.nickname = projectInfo.nickname
        this.project.birthday = projectInfo.birthday
        this.project.keyAttribute = projectInfo.keyAttribute
        this.project.weapons = projectInfo.weapons
        this.project.attributes.strength = projectInfo.attributes.strength
        this.project.attributes.dexterity = projectInfo.attributes.dexterity
        this.project.attributes.constitution = projectInfo.attributes.constitution
        this.project.attributes.intelligence = projectInfo.attributes.intelligence
        this.project.attributes.wisdom = projectInfo.attributes.wisdom
        this.project.attributes.charisma = projectInfo.attributes.charisma

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
    handleSave() {
        this.isSaving = true
        const id = this.$route.params.id;
        axios.put(`/api/knights/${id}`, this.project)
          .then(response => {
            Swal.fire({
                icon: 'success',
                title: 'Project updated successfully!',
                showConfirmButton: false,
                timer: 1500
            })
            this.isSaving = false
            this.project.name = ""
            this.project.description = ""
            return response
          })
          .catch(error => {
            this.isSaving = false
            Swal.fire({
                icon: 'error',
                title: 'An Error Occured!',
                showConfirmButton: false,
                timer: 1500
            })
            return error
          });
    },
    addWeapon(){
      let newWeapon = {
        name: this.weapon.name,
        mod: this.weapon.mod,
        attr: this.weapon.attr,
        equipped: this.weapon.equipped
      }
      this.project.weapons.push(newWeapon)

      this.weapon.name = ''
      this.weapon.mod = 0
      this.weapon.attr = ''
      this.weapon.equipped = false
    },
    removeWeapon(index){
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
              this.project.weapons.splice(index,1)
            }
          })
    },
  },
};
</script>