<template>
  <table id="display" className="display">
    <tbody>
      <tr>
        <th>Profile Image</th>
        <th>Name</th>
        <th>Gender</th>
        <th>Departments</th>
        <th>Salary</th>
        <th>Start Date</th>
        <th>Notes</th>
        <th>Actions</th>
      </tr>
      <tr v-for="employee in employees" :key="employee.id">
        <td><img :src="`{{employee.profilePic}}`" :alt="Profile" /></td>
        <td>{{ employee.name }}</td>
        <td>{{ employee.gender }}</td>
        <td>{{ employee.departments }}</td>
        <td>{{ employee.salary }}</td>
        <td>{{ employee.startDate }}</td>
        <td>{{ employee.note }}</td>
        <td>
          <img
            @click="remove(employee.id)"
            src="../assets/images delete.png"
            alt="delete"
            width="30"
            height="30"
          />
          <img
            @click="update(employee.id)"
            src="../assets/images 1.png"
            alt="edit"
            width="30"
            height="30"
          />
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
import EmployeeService from "../service/EmployeeService";
export default {
  name: "DisplayHome",
  data() {
    return {
      employees: [],
    };
  },
  methods: {
    getEmployees() {
      EmployeeService.getAllEmployees().then((response) => {
        console.log(response.data.data);
        this.employees = response.data.data;
      });
    },
    update(id) {
      console.log(id);
      this.$router.push({ name: "EditForm", params: { id: id } });
    },
    remove(id) {
      console.log(id);
      EmployeeService.deleteEmployee(id)
        .then((data) => {
          var answer = window.confirm(
            " Employee data once deleted cannot be restored!! Do you wish to continue ?",
            data
          );
          if (answer === true) {
            alert("Employee data deleted successfully!!");
            window.location.reload();
            this.getEmployees();
          } else {
            window.location.reload();
          }
        })
        .catch((error) => {
          alert("Something Went Wrong!");
        });
    },
  },
  created() {
    this.getEmployees();
  },
};
</script> 

