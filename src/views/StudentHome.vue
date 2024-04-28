<template>
  <div>
    <UserNavbar :id_="1"></UserNavbar>

    <b-container fluid="lg">
      <b-row class="mt-4" align-h="between">
        <b-col>
          <div>
            <b-button-group>
              <b-button v-if="all_tickets" variant="primary" @click="all_tickets_toggle(1)">All Tickets</b-button>
              <b-button v-else variant="outline-primary" @click="all_tickets_toggle(1)">All Tickets</b-button>
              <b-button v-if="!all_tickets" variant="primary" @click="all_tickets_toggle(2)">My tickets</b-button>
              <b-button v-else variant="outline-primary" @click="all_tickets_toggle(2)">My Tickets</b-button>
            </b-button-group>
          </div>
          <div class="mt-3">
            <b-button v-b-modal.create-modal variant="outline-primary">Create ticket</b-button>
          </div>
        </b-col>
        <b-col>
          <div>
            <b-form-input v-model="search" placeholder="Search here"></b-form-input>
          </div>
        </b-col>
      </b-row>
      <TicketCard v-for="t in ticket_card_details" :type="1" :ticket_object="t" @edit="edit_modal(t)" @delete="delete_modal(t)"
        @like="get_tickets" />
      <div>
        <b-modal hide-backdrop hide-footer content-class="shadow" id="create-modal" size="lg" title="Create ticket">
          <TicketForm @created="get_tickets" :hideReset=false :editTicket=false></TicketForm>
        </b-modal>
        <b-modal hide-backdrop hide-footer content-class="shadow" id="edit-modal" size="lg" title="Edit ticket">
          <TicketForm :title="edit_t.title" :description="edit_t.description" :priority="edit_t.priority"
            :tags="edit_t.tags" :ticket_id="edit_t.ticket_id" @created="get_tickets" :hideReset=true :editTicket=true>
          </TicketForm>
        </b-modal>
        <b-modal hide-backdrop content-class="shadow" id="delete-modal" size="lg" title="Delete ticket"
          @ok="delete_ticket">
          Are you sure you want to delete ticket?
        </b-modal>
      </div>
    </b-container>
  </div>
</template>

<script>
import UserNavbar from "../components/UserNavbar.vue";
import * as common from "../assets/common.js";
import TicketCard from "../components/Ticket_card.vue";
import InfoCard from "../components/InfoCard.vue";
import TicketForm from "../components/TicketForm.vue";
import SearchTicket from "@/components/SearchTicket.vue";


export default {
  name: "StudentHome",
  components: { UserNavbar, TicketCard, InfoCard, TicketForm, SearchTicket },
  data() {
    return {
      all_tickets: true,
      n_tickets_created: 0,
      n_tickets_resolved: 0,
      n_tickets_pending: 0,
      n_tickets_upvoted: 0,
      user_id: this.$store.getters.get_user_id,
      ticket_card_details: [],
      edit_t: {},
      delete_id: -1,
      search: "",
    };
  },
  created() {
    let form = {
      filter_status: ["pending"],
    };
    let params = "";
    params = new URLSearchParams(form).toString();

    fetch(common.STUDENT_API + `/${this.user_id}`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        web_token: this.$store.getters.get_web_token,
        user_id: this.user_id,
      },
    })
      .then((response) => response.json())
      .then((data) => {
        if (data.category == "success") {
          this.flashMessage.success({
            message: "User data retrieved.",
          });
          this.n_tickets_created = data.message.n_tickets_created;
          this.n_tickets_resolved = data.message.n_tickets_resolved;
          this.n_tickets_pending = data.message.n_tickets_pending;
          this.n_tickets_upvoted = data.message.n_tickets_upvoted;
        }
        if (data.category == "error") {
          this.flashMessage.error({
            message: data.message,
          });
        }
      })
      .catch((error) => {
        this.$log.error(`Error : ${error}`);
        this.flashMessage.error({
          message: "Internal Server Error",
        });
      });

    this.get_tickets();
  },
  mounted() { },
  methods: {
    all_tickets_toggle(num) {
      if (num == 1) {
        this.all_tickets = true;
      }
      else
        this.all_tickets = false
      this.get_tickets();
    },
    get_tickets() {
      this.$bvModal.hide('create-modal');
      this.$bvModal.hide('edit-modal');
      this.$bvModal.hide('delete-modal');
      let form = {
        filter_status: ["pending"],
      };
      let params = "";
      params = new URLSearchParams(form).toString();
      if (this.all_tickets) {
        fetch(common.TICKET_API_ALLTICKETS, {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            web_token: this.$store.getters.get_web_token,
            user_id: this.user_id,
          },
        })
          .then((response) => response.json())
          .then((data) => {
            if (data.category == "success") {
              this.ticket_card_details = data.message.filter((t) => {
                return String(t.title).includes(this.search);
              });
            }
            if (data.category == "error") {
              this.flashMessage.error({
                message: data.message,
              });
            }
          })
          .catch((error) => {
            this.$log.error(`Error : ${error}`);
            this.flashMessage.error({
              message: "Internal Server Error",
            });
          });
      }
      else {
        fetch(common.TICKET_API_ALLTICKETS + `/${this.user_id}`, {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            web_token: this.$store.getters.get_web_token,
            user_id: this.user_id,
          },
        })
          .then((response) => response.json())
          .then((data) => {
            if (data.category == "success") {
              this.ticket_card_details = data.message.filter((t) => {
                return String(t.title).includes(this.search);
              });
            }
            if (data.category == "error") {
              this.flashMessage.error({
                message: data.message,
              });
            }
          })
          .catch((error) => {
            this.$log.error(`Error : ${error}`);
            this.flashMessage.error({
              message: "Internal Server Error",
            });
          });
      }
    },
    edit_modal(t) {
      this.edit_t = t;
      this.$bvModal.show('edit-modal')
    },
    delete_modal(t) {
      this.delete_id = t.ticket_id;
      this.$bvModal.show("delete-modal");
    },
    delete_ticket() {
      fetch(common.TICKET_API + `/${this.delete_id}/${this.user_id}`, {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
          web_token: this.$store.getters.get_web_token,
          user_id: this.user_id,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          if (data.category == "success") {
            this.get_tickets();
          }
        })
    }
  },
  watch: {
    search() {
      this.get_tickets()
    }
  }
};
</script>
