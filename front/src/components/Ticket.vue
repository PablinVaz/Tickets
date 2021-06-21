<template>
  <div v-if="currentTicket" class="edit-form">
    <h4>Tickets</h4>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" id="title"
               v-model="currentTicket.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description"
               v-model="currentTicket.description"
        />
      </div>

      <div class="form-group">
        <label><strong>Status:</strong></label>
        {{ currentTicket.published ? "Published" : "Pending" }}
      </div>
    </form>

    <button class="badge badge-primary mr-2"
            v-if="currentTicket.published"
            @click="updatePublished(false)"
    >
      UnPublish
    </button>
    <button v-else class="badge badge-primary mr-2"
            @click="updatePublished(true)"
    >
      Publish
    </button>

    <button class="badge badge-danger mr-2"
            @click="deleteTicket"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
            @click="updateTicket"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Ticket...</p>
  </div>
</template>

<script>
import TicketDataService from "../services/TicketDataService";

export default {
  name: "ticket",
  data() {
    return {
      currentTicket: null,
      message: ''
    };
  },
  methods: {
    getTicket(id) {
      TicketDataService.get(id)
          .then(response => {
            this.currentTicket = response.data;
            console.log(response.data);
          })
          .catch(e => {
            console.log(e);
          });
    },

    updatePublished(status) {
      var data = {
        id: this.currentTicket.id,
        title: this.currentTicket.title,
        description: this.currentTicket.description,
        published: status
      };

      TicketDataService.update(this.currentTicket.id, data)
          .then(response => {
            console.log(response.data);
            this.currentTicket.published = status;
            this.message = 'The status was updated successfully!';
          })
          .catch(e => {
            console.log(e);
          });
    },

    updateTicket() {
      TicketDataService.update(this.currentTicket.id, this.currentTicket)
          .then(response => {
            console.log(response.data);
            this.message = 'The ticket was updated successfully!';
          })
          .catch(e => {
            console.log(e);
          });
    },

    deleteTicket() {
      TicketDataService.delete(this.currentTicket.id)
          .then(response => {
            console.log(response.data);
            this.$router.push({ name: "tickets" });
          })
          .catch(e => {
            console.log(e);
          });
    }
  },
  mounted() {
    this.message = '';
    this.getTicket(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>