<template>
  <div id="appStudent">
    <v-container>
      <table class="table" id="liste">
        <tr>
          <th class="table-danger" scope="col" id="tblId">Id</th>
          <th class="table-danger" scope="col" id="tblName">Name</th>
          <th class="table-danger" scope="col" id="tblSurname">Surname</th>
          <th class="table-danger" scope="col" id="tblNumber">Number</th>
          <th class="table-danger" scope="col" id="tblAge">Age</th>
          <th class="table-danger" scope="col" id="tblGpa">Gpa</th>
          <th class="table-danger" scope="col" id="tblSchool">School Name</th>
          <th class="table-danger" scope="col" id="tblApply">Apply</th>
        </tr>

        <tbody>
          <tr v-for="(item, index) in studentList" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.surname }}</td>
            <td>{{ item.number }}</td>
            <td>{{ item.age }}</td>
            <td>{{ item.gpa }}</td>
            <td>{{ item.school.name }}</td>
            <td>
              <input
                type="button"
                value="Update"
                @click="updatesWrite(index)"
                class="btn btn-info"
              />
              <input
                type="button"
                @click="deleteStudent(item.id)"
                value="Delete"
                class="btn btn-danger"
              />
            </td>
          </tr>
        </tbody>
      </table>

      <div id="appId" style="margin-left: 350px">
        <form @submit="saveStudent($event)" id="formOne">
          <input
            v-model="selectedStudent.id"
            type="text"
            id="textId"
            style="display: none"
          />
          <font id="name" color="blue">Name :</font>
          <input
            v-model="student.name"
            type="text"
            id="textName"
            scope="row"
            placeholder="name..."
          />
          <font id="surname" color="blue">Surname :</font>
          <input
            v-model="student.surname"
            type="text"
            id="textSurname"
            scope="row"
            placeholder="surname..."
          />
          <font id="number" color="blue">Number :</font>
          <input
            v-model="student.number"
            type="text"
            id="textNumber"
            scope="row"
            placeholder="number..."
          />
          <font id="age" color="blue">Age :</font>
          <input
            v-model="student.age"
            type="number"
            id="textAge"
            scope="row"
            placeholder="age..."
          />
          <font id="gpa" color="blue">Gpa :</font>
          <input
            v-model="student.gpa"
            type="number"
            id="textGpa"
            scope="row"
            placeholder="gpa..."
          />
          <font id="schoolName" color="blue">School Name :</font>
          <select v-model="student.school.id">
            <option :value="null">Seçiniz</option>
            <option v-for="c in schools" :key="c.id" :value="c.id">
              {{ c.name }}
            </option>
          </select>

          <input
            class="btn btn-outline-success"
            @click="saveStudent()"
            value="SAVE"
            id="add"
            style="width: 10%; height: 10%"
          />
          <input
            class="btn btn-outline-success"
            type="button"
            @click="clear()"
            value="CLEAR"
            id="clear"
            style="width: 10%; height: 10%"
          />
        </form>
      </div>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "app",
  data: () => ({
    student: {
      id: "",
      name: "",
      surname: "",
      number: "",
      age: "",
      gpa: "",
      school: {},
    },
    studentList: [],
    schools: [],
    selectedStudent: {},
  }),
  methods: {
    async getSchoolList() {
      const response = await this.axios.get("http://localhost:8080/school");
      this.schools = response.data._embedded.schools;
      console.log(this.schools);
    },

    async getStudentList() {
      const response = await this.axios.get("http://localhost:8080/student");
      this.studentList = response.data._embedded.students;
    },

    async addStudent() {
      await this.axios.post("http://localhost:8080/student/", this.student);
      console.log(this.student);
      this.getStudentList();
      this.clear();
    },

    updatesWrite(i) {
      console.log(i);
      this.student.id = this.studentList[i].id;
      this.student.name = this.studentList[i].name;
      this.student.surname = this.studentList[i].surname;
      this.student.number = this.studentList[i].number;
      this.student.age = this.studentList[i].age;
      this.student.gpa = this.studentList[i].gpa;
      this.student.school = this.studentList[i].school;
    },

    async updateStudent() {
      console.log(this.student);
      await this.axios.put(
        "https://localhost:8080/student/" + this.student.id,
        this.student
      );
      this.clear();
      this.getStudentList();
    },

    async deleteStudent(id) {
      await this.axios.delete("http://localhost:8080/student/" + id);
      this.getStudentList();
    },

    saveStudent() {
      if (this.selectedStudent.id == null || this.selectedStudent.id == "") {
        this.addStudent();
        this.clear();
      } else {
        this.updateStudent();
        this.clear();
      }
    },

    clear() {
      this.student = {
        id: null,
        name: "",
        surname: "",
        number: "",
        age: "",
        gpa: "",
        school: "",
      };
    },
  },

  mounted() {
    this.getStudentList();
    this.getSchoolList();
  },
};
</script>

<style>
#studentHd {
  position: relative;
  margin-left: 550px;
}
#Liste {
  position: relative;
  margin-left: 400px;
}
#appid {
  position: relative;
  margin-right: 400px;
  margin-bottom: 200px;
}
#selectId {
  position: relative;
  margin-right: 100px;
}
#textName {
  background-color: pink;
}
#textSurname {
  background-color: pink;
}
#textNumber {
  background-color: pink;
}
#textAge {
  background-color: pink;
}
#textGpa {
  background-color: pink;
}
#selectId {
  background-color: pink;
}
#add {
  background-color: rgb(214, 252, 109);
  position: relative;
  margin-left: 170px;
}
#name {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#surname {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#number {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#age {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#gpa {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#schoolName {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
</style>