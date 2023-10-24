<template>
    <div class="container">

        <h3 class="mt-2 mb-3 float-left text-primary">Employee</h3>
        <!-- <div > -->
            <form @submit="fetchEmployees" class="d-flex flex-row pt-3 pb-3 pl-5">
                <div class="form-group d-flex flex-row align-items-center">
                <label for="status">Status</label>
                <select id="statusOption" v-model="filter.selected" class="form-control ml-2">
                    <option value=1>Aktif</option>
                    <option value=false>Tidak Aktif</option>
                </select>
            </div>
            <div class="form-group d-flex flex-row align-items-center ml-3 mr-3">
                <label for="division">Division</label>
                <select id="divisionOption" class="form-control ml-2">
                    <option value=1>Keuangan</option>
                    <option value=2>HRD</option>
                    <option value=3>Pemasaran</option>
                    <option value=4>Produksi </option>
                    <option value=5>Divisi Umum</option>
                    <option value=16>IT</option>
                </select>
            </div>
            <div>
                <button type="submit" class="btn btn-primary ">filter</button>
            </div>
            </form>
            
        <!-- </div> -->



        <table class="table table-striped">
            <thead>
                <tr>
                    <th scope="col">Nama</th>
                    <th scope="col">Phone</th>
                    <th scope="col">Address</th>
                    <th scope="col">Job</th>
                    <th scope="col">Division</th>
                    <th scope="col">Salary</th>
                    <th scope="col">Status</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(employees, index) in this.employees" :key="index" class="m-5">
                    <td>{{ employees.name }}</td>
                    <td>{{ employees.phone }}</td>
                    <td>{{ employees.address }}</td>
                    <td>{{ employees.job.name }}</td>
                    <td>{{ employees.division.name }}</td>
                    <td>{{ employees.salary }}</td>
                    <td>{{ employees.status }}</td>

                    <td>
                        <!-- <form> -->
                        <button v-on:click.prevent="// @ts-ignore
                            emitShowModal(employees)" class="btn btn-sm btn-info mr-1">Edit</button>
                        <button v-on:click="// @ts-ignore
                            deleteEmployee(employees.id)" type="submit" class="btn btn-sm btn-danger">Delete</button>
                        <!-- </form> -->
                    </td>
                </tr>

            </tbody>
        </table>

    </div>
</template>

<script>
import { EventBus } from '../main.js';
import axios from 'axios';

const config = {
    headers: { Authorization: `Bearer 1|gzQP78qhcaDung5LjaWACfDPcMVMREHCcwTCJtbTf799851e` },
};
// @ts-ignore
export default {
    name: 'EmployeesTable',
    data() {
        return {
            employees: [],
            showModal: false,
            filter: {
                selected: 1
            }
        }
    },

    async created() {
        await axios.get('http://127.0.0.1:8000/api/employee', config)
            .then((response) => {
                this.employees = response.data.data
            })
            .catch((error) => {
                console.log(error)
            })
    },
    computed: {
        filteredEmployees() {
            return this.employees.filter(employee => {
                if (this.filter.selected) {
                    return employee.status = this.filter.selected;
                }
                return true;
            });
        },
    },
    methods: {

        async deleteEmployee(id) {
            await axios.delete(`http://localhost:8000/api/employee/${id}`, config)
                .then(() => { console.log('data deleted successfully'); window.location.reload() })
                .catch((err) => {
                    console.error(err);
                });
        },
   
        async fetchEmployees(event) {
            event.preventDefault();
            // Replace with your API endpoint and appropriate query parameters
            const apiUrl = 'http://localhost:8000/api/employee';
            const queryParams = {
                status: this.filter.selected,
            };

            return await axios.get(apiUrl, {
                params: queryParams,
                headers: { Authorization: `Bearer 1|gzQP78qhcaDung5LjaWACfDPcMVMREHCcwTCJtbTf799851e` },
            }).then((res) => this.employees = res.data).catch((err) => console.log(err));
        },
        
        emitShowModal(employee) {
            this.employee = employee;
            EventBus.$emit('click', this.showModal, this.employee);

            console.log(this.employee);
            console.log('EmployeesTable:', this.showModal);
        }
    }
}
</script>

<style scoped>
th {
    padding-left: 1.2rem;
}
</style>
