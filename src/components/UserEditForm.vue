<template>
    <div class="container">
        <div v-bind:class="modalClass" class="modal-container">
            <div class="user-modal">
                <h3 class="text-primary">Edit Employee</h3>
                <form>
                <div class="form-group">
                    <label for="name">Name</label>
                    <input v-bind:value="employee.name" type="text" required id="nameEdit" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="phone">Phone</label>
                    <input v-bind:value="employee.phone" type="text" id="phoneEdit" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="address">Address</label>
                    <input v-bind:value="employee.address" type="text" id="addressEdit" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="salary">Salary</label>
                    <input v-bind:value="employee.salary" type="number" id="salaryEdit" class="form-control" />
                </div>

                <div class="form-group">
                    <label for="job">Job</label>
                    <select id="jobSelected" v-model="jobSelected" required class="form-control">
                        <option v-for="(jobs, index) in this.jobs" :key="index" :value="jobs.id">{{ jobs.name }}</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="division">Division</label>
                    <select id="divisionSelected" v-model="divisionSelected" required class="form-control">
                        <option v-for="(divisions, index) in this.divisions" :key="index" :value="divisions.id">{{ divisions.name }}</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="status">Status</label>
                    <select id="statusSelected" class="form-control">
                        <option value=1>Aktif</option>
                        <option value=0>Tidak Aktif</option>
                    </select>
                </div>

                <button v-on:click.prevent="// @ts-ignore
                    showModal = !showModal" class="btn btn-secondary mr-1">Cancel</button>
                <button v-on:click="// @ts-ignore
                    updateEmployee(employee.id)" type="button" class="btn btn-primary">Edit</button>
                </form>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
import { EventBus } from '../main.js';

const token = "1|gzQP78qhcaDung5LjaWACfDPcMVMREHCcwTCJtbTf799851e"
// @ts-ignore
export default {
    name: 'EmployeeEditForm',
    data() {
        return {
            employees: [],
            employee: '',
            showModal: false,
        }
    },
    mounted() {
        this.getDivision();
        this.getJobs();
    },
    methods: {
        async updateEmployee(id) {
            const data = {
                // @ts-ignore
                name: document.getElementById('nameEdit').value,
                // @ts-ignore
                phone: document.getElementById('phoneEdit').value,
                // @ts-ignore
                address: document.getElementById('addressEdit').value,
                // @ts-ignore
                salary: Number(document.getElementById('salaryEdit').value),
                // @ts-ignore
                job_id: Number(document.getElementById('jobSelected').value),
                // @ts-ignore
                division_id: Number(document.getElementById('divisionSelected').value),
                // @ts-ignore
                status: Number(document.getElementById('statusSelected').value)
            }
            console.log(id,data);
            axios.put(`http://127.0.0.1:8000/api/employee/${id}`, data, {
                headers: { 'Content-type': 'application/json', 'Authorization': `Bearer ${token}` }
            })
                .then((data) => {
                    this.employees = data;
                    console.log('data Edited Successfully');
                    window.location.reload()
                })
                .catch((err) => {
                    console.error(err);
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
    created() {
        EventBus.$on('click', (modal, employee) => {
            this.showModal = !modal;
            this.employee = employee;
            console.log('showModal', this.showModal);
            console.log('modal', modal)
        })
    },
    computed: {
        modalClass() {
            return this.showModal ? 'show' : '';
        }
    }

};
</script>

<style></style>
