<template>
  <div id="app">
    <Header />
    <UserRegisterForm />
    <UserEditForm />
    <UsersTable />
  </div>
</template>

<script>
import UserRegisterForm from './components/UserRegisterForm.vue';
import UserEditForm from './components/UserEditForm.vue';
import UsersTable from './components/UsersTable.vue';


export default {
  name: 'App',
  components: {
    UserRegisterForm,
    UsersTable,
    UserEditForm
  }
}
</script>

<style>
#app {
  margin-top: 40px;
}
</style>
