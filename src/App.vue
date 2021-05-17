<template>
  <div>
    <form @submit.prevent="add">
      <input type="hidden" v-model="form.id" />
      <input type="text" v-model="form.title" />
      <button type="submit" v-show="!updateSubmit">add</button>
      <button type="button" v-show="updateSubmit" @click="update(form)">
        Update
      </button>
    </form>
    <ul v-for="content in contents" :key="content.idd">
      <li>
        <span>{{ content.title }}</span> &#160;
        <button @click="edit(content)">Edit</button> ||
        <button @click="del(content)">Delete</button>
      </li>
    </ul>
  </div>
</template>
<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        title: "",
      },
      contents: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/contents")
        .then((res) => {
          this.contents = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/contents/", this.form).then((res) => {
        this.load();
        this.form.title = "";
      });
    },
    edit(content) {
      this.updateSubmit = true;
      this.form.id = content.id;
      this.form.title = content.title;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/contents/" + form.id, {
          title: this.form.title,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.title = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(content) {
      axios
        .delete("http://localhost:3000/contents/" + content.id)
        .then((res) => {
          this.load();
          let index = this.contents.indexOf(form.title);
          this.contents.splice(index, 1);
        });
    },
  },
};
</script>