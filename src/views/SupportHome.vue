<template>
  <div>
    <UserNavbar :id_="1"></UserNavbar>

    <b-container fluid="lg">
      <b-row class="mt-4" align-h="between">
        <b-col>
          <div>
            <b-button-group>
              <b-button v-if="tab_id == 0" variant="primary" @click="tab_toggle(0)">All</b-button>
              <b-button v-else variant="outline-primary" @click="tab_toggle(0)">All</b-button>
              <b-button v-if="tab_id == 1" variant="primary" @click="tab_toggle(1)">Pending</b-button>
              <b-button v-else variant="outline-primary" @click="tab_toggle(1)">Pending</b-button>
            </b-button-group>
          </div>
        </b-col>
        <b-col>
          <div>
            <b-form-input v-model="search" placeholder="Search here"></b-form-input>
          </div>
        </b-col>
      </b-row>
      <div>
        <TicketCard v-for="t in tickets" :type="2" :ticket_object="t" :expanded-view="false"></TicketCard>
      </div>
    </b-container>
  </div>
</template>

<script>
import UserNavbar from "../components/UserNavbar.vue";
import * as common from "../assets/common.js";
import SearchTicket from "../components/SearchTicket.vue";
import TicketCard from "@/components/Ticket_card.vue";

export default {
  name: "SupportHome",
  components: { UserNavbar, SearchTicket, TicketCard },
  data() {
    return {
      tab_id: 0,
      n_tickets_resolved: 0,
      n_total_unresolved_tickets: 0,
      tickets: [],
      user_id: this.$store.getters.get_user_id,
      search: "",
    };
  },
  created() {
    this.get_tickets();
  },
  mounted() { },
  methods: {
    tab_toggle(i) {
      this.tab_id = i;
      this.get_tickets();
    },
    get_tickets() {
      let form = {
        filter_status: ["pending"],
      };
      let params = "";
      params = new URLSearchParams(form).toString();

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
            this.tickets = data.message;
            if (this.tab_id == 0) {
              this.tickets = data.message.filter((t) => {
                return String(t.title).includes(this.search);
              });
            }
            else if (this.tab_id == 1) {
              this.tickets = data.message.filter((t) => {
                return String(t.title).includes(this.search);
              });
              this.tickets = this.tickets.filter(t => t.status == 'pending');
            }
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
    },
  },
  computed: {
    pending: function () {
      return this.tickets.filter(t => t.status == 'pending')
    }
  },
  watch: {
    search() {
      this.get_tickets()
    }
  }
};
</script>

<style></style>
