<template>
    <div class="container">
        <button v-on:click="// @ts-ignore
            showModal = !showModal" class="btn btn-lg btn-outline-primary float-right">Add Employee</button>

        <div v-bind:class="modalClass" class="modal-container">
            <div class="user-modal">
                <h3 class="text-primary">Add Employee</h3>
                <form>
                <div class="form-group">
                    <label for="name">Name</label>
                    <input required value="" type="text" id="nameRegister" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="phone">Phone</label>
                    <input required value="" type="text" id="phoneRegister" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="address">Address</label>
                    <input required value="" type="text" id="addressRegister" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="salary">Salary</label>
                    <input required value="" type="number" id="salaryRegister" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="job">Job</label>
                    <select id="jobOption" v-model="jobOption" class="form-control">
                        <option v-for="(jobs, index) in this.jobs" :key="index" :value="jobs.id">{{ jobs.name }}</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="division">Division</label>
                    <select id="divisionOption" v-model="divisionOption" class="form-control">
                        <option v-for="(divisions, index) in this.divisions" :key="index" :value="divisions.id">{{ divisions.name }}</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="status">Status</label>
                    <select id="statusOption" class="form-control">
                        <option value=1>Aktif</option>
                        <option value=0>Tidak Aktif</option>
                    </select>
                </div>

                <button v-on:click.prevent="// @ts-ignore
                    showModal = !showModal" class="btn btn-secondary mr-1">Cancel</button>
                <button type="submit" v-on:click="// @ts-ignore
                    createUser()" class="btn btn-primary">Add</button>
                </form>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';

const token = "1|gzQP78qhcaDung5LjaWACfDPcMVMREHCcwTCJtbTf799851e"
// @ts-ignore
export default {
    name: 'EmployeeRegisterForm',
    data() {
        return {
            users: [],
            showModal: false,
        };
    },
    mounted() {
        this.getDivision();
        this.getJobs();
    },

    methods: {
        async createUser() {
            const data = {
                // @ts-ignore
                name: document.getElementById('nameRegister').value,
                // @ts-ignore
                phone:document.getElementById('phoneRegister').value,
                // @ts-ignore
                address: document.getElementById('addressRegister').value,
                // @ts-ignore
                salary: Number(document.getElementById('salaryRegister').value),
                // @ts-ignore
                job_id:Number( document.getElementById('jobOption').value),
                // @ts-ignore
                division_id:Number( document.getElementById('divisionOption').value),
                // @ts-ignore
                status:Number(document.getElementById('statusOption').value),
            }
            console.log(data)
            await axios.post('http://127.0.0.1:8000/api/employee', data, {
                headers: { 'Content-type': 'application/json', 'Authorization': `Bearer ${token}` },
            })
                .then((response) => response.data)
                .then((data) => {
                    this.employees = data;
                    console.log('data adding successfully');
                })
                .catch((err) => {
                    console.log("error" + err);
                });
        },
        async getDivision() {
            await axios.get('http://127.0.0.1:8000/api/division')
                .then((response) => {
                    this.divisions = response.data.data
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        async getJobs() {
            await axios.get('http://127.0.0.1:8000/api/job')
                .then((response) => {
                    this.jobs = response.data.data
                })
                .catch((error) => {
                    console.log(error)
                })
        },

    },
    computed: {
        modalClass() {
            return this.showModal ? 'show' : '';
        }
    }
};
</script>

<style>
.modal-container {
    position: fixed;
    background-color: rgba(0, 0, 0, 0.3);
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;

    display: flex;
    align-items: center;
    justify-content: center;

    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease-in-out;
}

.modal-container.show {
    opacity: 1;
    pointer-events: auto;
}

.user-modal {
    color: #535353;
    background-color: #fff;
    background-image: url("../assets/background.jpg");
    width: 450px;
    padding: 40px 50px;
    max-width: 100%;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.user-modal h1 {
    margin: 0;
}

.user-modal p {
    opacity: 0.9;
}
</style>
