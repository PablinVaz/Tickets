<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
            type="text"
            class="form-control"
            id="title"
            required
            v-model="ticket.title"
            name="title"
        />
      </div>

      <div class="form-group mt-2">
        <label for="description">Description</label>
        <input
            class="form-control"
            id="description"
            required
            v-model="ticket.description"
            name="description"
        />
      </div>

      <button @click="saveTicket" class="btn btn-success mt-2">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newTicket">Add</button>
    </div>
  </div>
</template>

<script>
import TicketDataService from "../services/TicketDataService";

export default {
  name: "add-ticket",
  data() {
    return {
      ticket: {
        id: null,
        title: "",
        description: "",
        published: false
      },
      submitted: false
    };
  },
  methods: {
    saveTicket() {
      let data = {
        title: this.ticket.title,
        description: this.ticket.description
      };

      TicketDataService.create(data)
          .then(response => {
            this.ticket.id = response.data.id;
            console.log(response.data);
            this.submitted = true;
          })
          .catch(e => {
            console.log(e);
          });
    },

    newTicket() {
      this.submitted = false;
      this.ticket = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>>