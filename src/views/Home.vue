<template>
  <div class="body">
    <h1>
      <span class="blue">&lt;</span>User List<span class="blue">&gt;</span>
    </h1>
    <h2>
      Admin: <a href="https://anybrain.gg/">{{ admin.name }}</a>
    </h2>
    <table class="container">
      <thead>
        <th><h1>User ID</h1></th>
        <th><h1>Name</h1></th>
        <th><h1>In-game Time</h1></th>
      </thead>
      <tbody>
        <tr
          v-for="user in usersData"
          @click="openModal(user.id)"
          :key="user.id"
        >
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.playtime }}h</td>
        </tr>
      </tbody>
    </table>
    <div class="report-list">
      <button class="report-list-button" @click="goReports()">
        Report List
      </button>
    </div>
    <!-- Opacidade -->
    <transition name="fade" appear>
      <div class="modal-overlay" v-if="showModal" @click="closeModal()"></div>
    </transition>
    <!-- Modal -->
    <transition name="slide" appear>
      <div class="modal" v-if="showModal">
        <header>
          <h1>Report Form</h1>
          <form action="" class="col-md-6" @submit.prevent="handleSubmit()">
            <label for="user-id">User ID</label>
            <input id="user-id" :value="chosenUser" type="text" readonly />
            <label for="user-name">User Name</label>
            <input
              id="user-name"
              :value="chosenUserName"
              type="text"
              readonly
            />
            <label for="">Report Title</label>
            <input type="text" required v-model="reportTitle" />
            <label for="">Report Description</label>
            <textarea
              type="text"
              required
              v-model="reportDescription"
            ></textarea>
            <label for="">Infraction:</label>
            <select v-model="infraction">
              <option value=""></option>
              <option value="Assisted Aim">Assisted Aim</option>
              <option value="Abusive Language">Abusive Language</option>
              <option value="AFKing">AFKing</option>
              <option value="Friendly Fire">Friendly Fire</option>
            </select>
            <div class="submit">
              <button>Submit Report</button>
            </div>
          </form>
        </header>
      </div>
    </transition>
  </div>
</template>
<script>
// @ is an alias to /src
import users from "../../Files/Users";
import reports from "../../Files/Reports";
export default {
  name: "Home",
  components: {},
  data() {
    return {
      usersData: users,
      reportsData: reports,
      admin: { id: 0, name: "AnyBrain" },
      showModal: false,
      chosenUser: 0,
      chosenUserName: "",
      reportTitle: "",
      reportDescription: "",
      infraction: "",
    };
  },
  methods: {
    openModal: function (userid) {
      for (var i of this.usersData) {
        if (i.id == userid) {
          this.chosenUserName = i.name;
        }
      }
      this.chosenUser = userid;
      this.showModal = true;
    },
    closeModal: function () {
      this.showModal = false;
      this.chosenUser = 0;
      this.chosenUserName = "";
      this.reportTitle = "";
      this.reportDescription = "";
      this.infraction = "";
    },
    handleSubmit: function () {
      var idTmp = this.reportsData[this.reportsData.length - 1].id + 1;
      const obj = {
        id: idTmp,
        userId: this.chosenUser,
        adminId: this.admin.id,
        title: this.reportTitle,
        description: this.reportDescription,
        infraction: this.infraction,
      };
      this.reportsData.push(obj);

      this.closeModal();
    },
    goReports: function () {
      this.$router.push("/reports");
    },
  },
};
</script>
<style scoped>
@charset "UTF-8";
@import url(https://fonts.googleapis.com/css?family=Open+Sans:300, 400, 700);

.body {
  font-family: "Open Sans", sans-serif;
  font-weight: 300;
  line-height: 1.42em;
  color: #a7a1ae;
  background-color: #1f2739;
  margin: 0;
}
h1 {
  padding-top: 2em;
  font-size: 3em;
  font-weight: 300;
  line-height: 1em;
  text-align: center;
  color: #4dc3fa;
}
h2 {
  font-size: 1em;
  font-weight: 300;
  text-align: center;
  display: block;
  line-height: 1em;
  padding-bottom: 2em;
  color: #fb667a;
}
h2 a {
  font-weight: 700;
  text-transform: uppercase;
  color: #fb667a;
  text-decoration: none;
}
.blue {
  color: #185875;
}

.container th h1 {
  font-weight: bold;
  font-size: 1em;
  text-align: left;
  color: #185875;
}

.container td {
  font-weight: normal;
  font-size: 1em;
  -webkit-box-shadow: 0 2px 2px -2px #0e1119;
  -moz-box-shadow: 0 2px 2px -2px #0e1119;
  -box-shadow: 0 2px 2px -2px #0e1119;
}

.container {
  text-align: left;
  overflow: hidden;
  width: 80%;
  margin: 0 auto;
  display: table;
  padding: 0 0 3em 0;
}

.container td,
.container th {
  padding-bottom: 2%;
  padding-top: 2%;
  padding-left: 2%;
}

.container tr:nth-child(odd) {
  background-color: #323c50;
}

.container tr:nth-child(even) {
  background-color: #2c3446;
}

.container td:first-child {
  color: #fb667a;
}

.container tr:hover {
  background-color: #fff842;
  color: #403e10;
  font-weight: bold;
  box-shadow: #7f7c21 -1px 1px, #7f7c21 -2px 2px, #7f7c21 -3px 3px,
    #7f7c21 -4px 4px, #7f7c21 -5px 5px, #7f7c21 -6px 6px;
  transform: translate3d(6px, -6px, 0);
  transition-delay: 0s;
  transition-duration: 0.4s;
  transition-property: all;
  transition-timing-function: line;
  cursor: pointer;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 98;
  background-color: rgba(0, 0, 0, 0.6);
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 99;

  width: 100%;
  max-width: 400px;
  background-color: #323c50;
  border-radius: 16px;

  padding: 25px;
}
.modal h1 {
  padding-top: 0.25em;
  font-size: 3em;
  font-weight: 300;
  line-height: 1em;
  text-align: center;
  color: #4dc3fa;
}

.modal p {
  color: #666;
  font-size: 18px;
  font-weight: 400;
  margin-bottom: 15px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateY(-50%) translateX(100vw);
}

/* Modal */

form {
  max-width: 420px;
  margin: 20px auto;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}

form label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  letter-spacing: 1px;
}
form input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
form textarea,
select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}

button {
  background: #4dc3fa;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: #323c50;
  border-radius: 20px;
}
button:hover {
  background-color: #fff842;
  color: #403e10;
  font-weight: bold;
  box-shadow: #7f7c21 -1px 1px, #7f7c21 -2px 2px, #7f7c21 -3px 3px,
    #7f7c21 -4px 4px, #7f7c21 -5px 5px, #7f7c21 -6px 6px;
  transform: translate3d(6px, -6px, 0);
  transition-delay: 0s;
  transition-duration: 0.4s;
  transition-property: all;
  transition-timing-function: line;
  cursor: pointer;
}
.submit {
  text-align: center;
}

.report-list {
  text-align: center;
  padding-bottom: 4em;
}

.report-list-button {
  background: #4dc3fa;
  border: 0;
  padding: 20px 40px;
  margin-top: 0px;
  color: #323c50;
  border-radius: 40px;
}
</style>
