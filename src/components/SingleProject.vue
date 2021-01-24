<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions" @click="showDetails">
      <h3>{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons">
            edit
          </span>
        </router-link>
        <span @click="deleteProject" class="material-icons">
          delete
        </span>
        <span @click="completeProject" class="material-icons done">
          done
        </span>
      </div>
    </div>
    <div v-if="showdetails" class="details">
      {{ project.details }}
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showdetails: false,
      url: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    showDetails() {
      this.showdetails = !this.showdetails;
    },
    deleteProject() {
      //send a delete request to the json endpoint
      fetch(this.url, {
        method: "DELETE",
      }).then(() => {
        this.$emit("delete", this.project.id).catch((err) =>
          console.log(err.message)
        );
      });
    },
    completeProject() {
      fetch(this.url, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      }).then(() => {
        this.$emit("done", this.project.id).catch((err) =>
          console.log(err.message)
        );
      });
    },
  },
};
</script>

<style>
h3 {
  cursor: pointer;
}

.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.project.complete {
  border-left: 4px solid lightgreen;
}
.project.complete .done {
  color: lightgreen;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-icons:hover {
  color: #777;
}
</style>
