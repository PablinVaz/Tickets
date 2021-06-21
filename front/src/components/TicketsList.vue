<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Search by title"
               v-model="title"/>
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button"
                  @click="searchTitle">
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <h4>Tickets List</h4>
      <ul class="list-group">
        <li class="list-group-item"
            :class="{ active: index === currentIndex }"
            v-for="(ticket, index) in tickets"
            :key="index"
            @click="setActiveTicket(ticket, index)"
        >
          {{ ticket.title }}
        </li>
      </ul>

      <button class="m-3 btn btn-danger" @click="removeAllTickets">
        Remove All
      </button>
    </div>
    <div class="col-md-6">
      <div v-if="currentTicket">
        <h4>Ticket</h4>
        <div>
          <label><strong>Title:</strong></label> {{ currentTicket.title }}
        </div>
        <div>
          <label><strong>Description:</strong></label> {{ currentTicket.description }}
        </div>
        <div>
          <label><strong>Status:</strong></label> {{ currentTicket.published ? "Published" : "Pending" }}
        </div>

        <router-link :to="'/tickets/' + currentTicket.id" class="badge badge-warning">Edit</router-link>
      </div>
      <div v-else>
        <br />
        <p>Please click on a Ticket...</p>
      </div>
    </div>
  </div>
</template>

<script>
import TicketDataService from "../services/TicketDataService";

export default {
  name: "tickets-list",
  data() {
    return {
      tickets: [],
      currentTicket: null,
      currentIndex: -1,
      title: ""
    };
  },
  methods: {
    retrieveTickets() {
      TicketDataService.getAll()
          .then(response => {
            this.tickets = response.data;
            console.log(response.data);
          })
          .catch(e => {
            console.log(e);
          });
    },

    refreshList() {
      this.retrieveTickets();
      this.currentTicket = null;
      this.currentIndex = -1;
    },

    setActiveTicket(ticket, index) {
      this.currentTicket = ticket;
      this.currentIndex = ticket ? index : -1;
    },

    removeAllTickets() {
      TicketDataService.deleteAll()
          .then(response => {
            console.log(response.data);
            this.refreshList();
          })
          .catch(e => {
            console.log(e);
          });
    },

    searchTitle() {
      TicketDataService.findByTitle(this.title)
          .then(response => {
            this.tickets = response.data;
            this.setActiveTicket(null);
            console.log(response.data);
          })
          .catch(e => {
            console.log(e);
          });
    }
  },
  mounted() {
    this.retrieveTickets();
  }
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>