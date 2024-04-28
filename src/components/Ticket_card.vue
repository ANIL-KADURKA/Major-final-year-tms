<template>
  <div class="m-4">
    <b-card :title="ticket_object.title" :header="'Posted at ' + createDate">
      <div>
        <b-badge v-if="ticket_object.tag_1" pill class="mr-1" variant="info">#{{ ticket_object.tag_1 }}</b-badge>
        <b-badge v-if="ticket_object.tag_2" pill class="mr-1" variant="info">#{{ ticket_object.tag_2 }}</b-badge>
        <b-badge v-if="ticket_object.tag_3" pill class="mr-1" variant="info">#{{ ticket_object.tag_3 }}</b-badge>
        <b-badge v-if="ticket_object.status == 'resolved'" pill class="mr-1" variant="success">Solved</b-badge>
        <b-badge v-else-if="ticket_object.priority == 'high'" pill class="mr-1" variant="danger">High priority</b-badge>
        <b-badge v-else-if="ticket_object.priority == 'medium'" pill class="mr-1" variant="warning">Medium
          priority</b-badge>
        <b-badge v-else-if="ticket_object.priority == 'low'" pill class="mr-1" variant="success">Low priority</b-badge>
      </div>
      <p></p>
      <b-card-text>
        {{ ticket_object.description }}
      </b-card-text>
      <div v-if="expandedView"><i>Attachments</i>
        <div v-for="a in ticket_object.attachments"><img :src="a.attachment_loc" width="100%">
        </div>
      </div>
      <div class="mt-3 ml-1 reverse">
        <b-button-group v-if="!myTicket&&!expandedView&&type==1">
          <b-button variant="primary" v-if="is_upvoted"
            @click="rem_upvote"><b-icon-heart-fill></b-icon-heart-fill> {{ ticket_object.votes
            }}</b-button>
          <b-button variant="outline-primary" v-else @click="upvote"><b-icon-heart></b-icon-heart> {{
      ticket_object.votes
    }}</b-button>
        </b-button-group>
        <b-button-group v-if="myTicket&&!expandedView&&type==1&&ticket_object.status!='resolved'">
          <b-button class="m-1" variant="link" @click="$emit('edit')">Edit</b-button>
          <b-button class="m-1" variant="link" @click="$emit('delete')">Delete</b-button>
        </b-button-group>
        <b-button class="m-1" v-if="!expandedView" @click="redirect" variant="link">Open </b-button>
      </div>
    </b-card>
    <!-- {{ ticket_object }} -->
  </div>
</template>

<script>
import * as common from "../assets/common.js";
export default {
  name: 'TicketCard',
  props: ['ticket_object', 'expandedView','type'],
  data() {
    return {
      ticketId: 1,
      my_user_id: this.$store.getters.get_user_id,
      is_upvoted: false,
    }
  },
  mounted() {
    if(this.type==1) {this.check_upvote();}
  },
  updated() {
    if(this.type==1) {this.check_upvote();}
  },
  computed: {
    createDate: function () {
      const d = new Date(this.ticket_object.created_on * 1000);
      return d.toLocaleString("en-IN", { timeZone: 'Asia/Kolkata' });
    },
    myTicket: function () {
      if (this.my_user_id == this.ticket_object.created_by)
        return true;
      else return false;
    }
  },
  methods: {
    upvote() {
      fetch(common.TICKET_API + `/upvote`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          web_token: this.$store.getters.get_web_token,
          user_id: this.$store.getters.get_user_id,
        },
        body: JSON.stringify({
          "ticket_id": this.ticket_object.ticket_id,
          "uid": this.my_user_id,
        })
      }).then((response) => response.json())
        .then((data) => {
          if (data.category == "success") {
            this.$emit('like');
          }
        })
    },
    check_upvote() {
      fetch(common.TICKET_API + "/is_upvoted",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            web_token: this.$store.getters.get_web_token,
            user_id: this.$store.getters.get_user_id,
          },
          body: JSON.stringify({
            "ticket_id": this.ticket_object.ticket_id,
            "uid": this.my_user_id,
          })
        }).then((response) => response.json())
        .then((data) => {
          if (data.message.status == true) {
            this.is_upvoted = true;
          } else {
            this.is_upvoted = false;
          }
        })
    },
    rem_upvote() {
      fetch(common.TICKET_API + `/upvote`, {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
          web_token: this.$store.getters.get_web_token,
          user_id: this.$store.getters.get_user_id,
        },
        body: JSON.stringify({
          "ticket_id": this.ticket_object.ticket_id,
          "uid": this.my_user_id,
        })
      }).then((response) => response.json())
        .then((data) => {
          if (data.category == "success") {
            this.$emit('like');
          }
        })
    },
    redirect() {
      this.$store.dispatch("set_state_for_individual_ticket_page", this.ticket_object);
      this.$router.push('/ticket-page')
    }
  }
}
</script>

<style>
.reverse {
  display: flex;
  flex-direction: row-reverse;
}
</style>
