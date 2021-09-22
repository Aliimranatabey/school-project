<template>
  <div id="app">
    <v-container>
      <tbody id="schoolList">
        <div class="input-group rounded">
          <input
            id="searchName"
            type="search"
            onkeyup="searchSchool()"
            class="form-control rounded"
            placeholder="Search Name"
            aria-label="Search"
            aria-describedby="search-addon"
          />
          <span class="input-group-text border-0" id="search-addon">
            <i class="fas fa-search"></i>
          </span>
          <input
            id="searchCode"
            type="search"
            onkeyup="searchSchool()"
            class="form-control rounded"
            placeholder="Search Code"
            aria-label="Search"
            aria-describedby="search-addon"
          />
          <span class="input-group-text border-0" id="search-addon">
            <i class="fas fa-search"></i>
          </span>
        </div>
      </tbody>
      <table class="table" id="liste">
        <tr>
          <th class="table-danger" scope="col" id="tblId">Id</th>
          <th class="table-danger" scope="col" id="tblName">Name</th>
          <th class="table-danger" scope="col" id="tblCode">Code</th>
          <th class="table-danger" scope="col" id="tblActive">Active</th>
          <th class="table-danger" scope="col" id="tblApply">Apply</th>
        </tr>

        <tbody>
          <tr v-for="(item, index) in schoolList" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.code }}</td>
            <td>{{ item.active }}</td>
            <td>
              <input
                type="button"
                value="Update"
                @click="updatesWrite(index)"
                class="btn btn-info"
              />
              <input
                type="button"
                @click="deleteSchool(item.id)"
                value="Delete"
                class="btn btn-danger"
              />
            </td>
          </tr>
        </tbody>
      </table>

      <div id="appId" style="margin-left: 350px">
        <form @submit="saveSchool($event)" id="formOne">
          <input
            v-model="selectedSchool.id"
            type="text"
            id="textId"
            style="display: none"
          />
          <font id="name" color="blue">Name :</font>
          <input
            v-model="school.name"
            type="text"
            id="textName"
            scope="row"
            placeholder="name..."
          />
          <font id="code" color="blue">Code :</font>
          <input
            v-model="school.code"
            type="text"
            id="textCode"
            scope="row"
            placeholder="code..."
          />
          <font id="active" color="blue">Active :</font>
          <input
            v-model="school.active"
            checked="false"
            type="checkbox"
            id="checkActive"
            class="form-check-input"
            style="width: 3%; height: 2%"
          />
          <input
            class="btn btn-outline-success"
            @click="saveSchool()"
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
    school: { id: null, name: "", code: "", active: false },
    schoolList: [],
    selectedSchool: {},
  }),
  methods: {
    async getSchoolList() {
      const response = await this.axios.get("http://localhost:8080/school");
      this.schoolList = response.data._embedded.schools;
    },

    async addSchool() {
      await this.axios.post("http://localhost:8080/school/", this.school);
      this.getSchoolList();
      this.clear();
    },

    updatesWrite(i) {
      console.log(i);
      this.school.id = this.schoolList[i].id;
      this.school.name = this.schoolList[i].name;
      this.school.code = this.schoolList[i].code;
      this.school.active = this.schoolList[i].active;
    },

    async updateSchool() {
      console.log(this.school);
      await this.axios.put(
        "https://localhost:8080/school/" + this.school.id,
        this.school
      );
      this.clear();
      this.getSchoolList();
    },

    async deleteSchool(id) {
      await this.axios.delete("http://localhost:8080/school/" + id);
      this.getSchoolList();
    },

    saveSchool() {
      if (this.selectedSchool.id == null || this.selectedSchool.id == "") {
        this.addSchool();
        this.clear();
      } else {
        this.updateSchool();
        this.clear();
      }
    },

    clear() {
      this.school = { id: null, name: "", code: "", active: false };
    },
  },

  mounted() {
    this.getSchoolList();
  },
};
</script>

<style>
#schoolHd {
  position: relative;
  margin-left: 600px;
}
#Liste {
  position: relative;
  margin-left: 500px;
}
#tblname {
  position: relative;
  margin: 10px 0;
  width: 150px;
}
#tblcode {
  position: relative;
  margin: 10px 0;
  width: 50px;
}
#tblactive {
  position: relative;
  margin: 10px 0;
  width: 70px;
}
#tblapply {
  position: relative;
  margin: 10px 0;
  width: 150px;
}
#textName {
  background-color: pink;
}
#textCode {
  background-color: pink;
}
#checkActive {
  background-color: pink;
}
#add {
  background-color: rgb(214, 252, 109);
  position: relative;
  margin-left: 62px;
}
#name {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#code {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
#active {
  width: 100px;
  height: 100px;
  padding: 5px;
  background: #dab3da;
  color: rgb(8, 42, 44);
}
</style>