<template>
    <div>
        <UserNavbar :id_="1"></UserNavbar>
        <b-container fluid="lg">
            <!-- {{ this.$store.getters.get_web_token }} -->
            <!-- {{ this.my_user_id }} -->
            <!-- {{ ticket_object }} -->
            <!-- {{ this.all_replies }} -->
            <b-button class="mt-4" variant="outline-primary"
                @click="go_home"><b-icon-arrow-left></b-icon-arrow-left></b-button>
            <TicketCard :ticket_object="ticket_object" :expanded-view="true" />
            <div class="mt-3 mb-3" v-if="role == 'support' && ticket_object.status != 'resolved'">
                <b-button @click="mark_resolved" variant="success"><b-icon-check2-circle></b-icon-check2-circle> Mark resolved</b-button>
            </div>
            <div class="mt-3 mb-3" v-if="role == 'student' && ticket_object.status == 'resolved' && myTicket ">
                <b-button @click="escalate" variant="danger">Escalate ticket</b-button>
            </div>
            <div>
                <h4>Replies</h4>
            </div>
            <div class="m-4" v-if="(myTicket || role == 'support') && ticket_object.status != 'resolved'">
                <b-form-textarea v-model="reply_text" rows="4" max-rows="4"
                    placeholder="Add reply..."></b-form-textarea>
                <div class="mt-3">
                    <b-button @click="post_reply" variant="primary">Post reply</b-button>
                </div>
                <hr>
            </div>
            <div class="m-4" v-if="all_replies.length == 0">
                No replies yet
            </div>
            <div class="m-4" v-for="r in all_replies">
                <b-card :header="'Posted by ' + r.name + ' at ' + r.time_created">
                    <b-card-text>
                        {{ r.rcontent }}
                    </b-card-text>
                    <div v-if="ticket_object.status != 'resolved'" class="reverse"><b-button @click="d_modal(r.rid)" class="m-0" v-if="r.uid == my_user_id"
                            variant="link">Delete</b-button></div>
                </b-card>
            </div>
            <b-modal hide-backdrop content-class="shadow" id="delete-modal-reply" size="lg" title="Delete reply"
                @ok="delete_reply()">
                Are you sure you want to delete reply?
            </b-modal>
        </b-container>
    </div>
</template>

<script>
import UserNavbar from "../components/UserNavbar.vue";
import * as common from "../assets/common.js";
import TicketCard from "../components/Ticket_card.vue";

export default {
    name: "IndividualTicket",
    components: { UserNavbar, TicketCard },
    data() {
        return {
            ticket_object: this.$store.getters.get_individual_ticket_data,
            my_user_id: this.$store.getters.get_user_id,
            role: this.$store.getters.get_user_role,
            reply_text: "",
            all_replies: [],
            d_id: 0,
        };
    },
    methods: {
        go_home() {
            this.$router.go(-1)
        },
        post_reply() {
            fetch(common.REPLY_API + "/", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    web_token: this.$store.getters.get_web_token,
                    user_id: this.$store.getters.get_user_id,
                },
                body: JSON.stringify({
                    "user_id": this.$store.getters.get_user_id,
                    "rtext": this.reply_text,
                    "ticket_id": this.ticket_object.ticket_id
                })
            })
                .then((response) => response.json())
                .then((data) => {
                    if (data == null) {
                        this.reply_text = "";
                        this.get_replies();
                    }
                });
        },
        get_replies() {
            fetch(common.REPLY_API + `/all-replies/${this.ticket_object.ticket_id}`, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    web_token: this.$store.getters.get_web_token,
                    user_id: this.$store.getters.get_user_id,
                },
            })
                .then((response) => response.json())
                .then((data) => {
                    if (data.status == 200) {
                        this.all_replies = data.message;
                    } else {
                        this.all_replies = [];
                    }
                });
        },
        d_modal(id) {
            this.d_id = id;
            this.$bvModal.show("delete-modal-reply");
        },
        delete_reply() {
            fetch(common.REPLY_API + `/${this.my_user_id}/${this.d_id}`, {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json",
                    web_token: this.$store.getters.get_web_token,
                    user_id: this.$store.getters.get_user_id,
                },
            })
                .then((response) => response.json())
                .then((data) => {
                    if (data == null) {
                        this.get_replies();
                    }
                });
        },
        mark_resolved() {
            fetch(common.TICKET_API + "/mark_resolved", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    web_token: this.$store.getters.get_web_token,
                    user_id: this.$store.getters.get_user_id,
                },
                body: JSON.stringify({
                    "ticket_id": this.ticket_object.ticket_id
                })
            })
                .then((response) => response.json())
                .then((data) => {
                    if (data.status == 200) {
                        this.refresh_ticket();
                    }
                });
        },
        refresh_ticket(){
            fetch(common.TICKET_API + `/${this.ticket_object.ticket_id}/${this.my_user_id}`, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    web_token: this.$store.getters.get_web_token,
                    user_id: this.$store.getters.get_user_id,
                },
            })
                .then((response) => response.json())
                .then((data) => {
                    this.$store.dispatch("set_state_for_individual_ticket_page", data.message);
                    this.ticket_object = data.message;
                });
        },
        escalate(){
            fetch(common.TICKET_API + "/escalate", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    web_token: this.$store.getters.get_web_token,
                    user_id: this.$store.getters.get_user_id,
                },
                body: JSON.stringify({
                    "ticket_id": this.ticket_object.ticket_id,
                    "user_id": this.$store.getters.get_user_id
                })
            });
        },
    },
    mounted() {
        this.get_replies();
    },
    computed: {
        myTicket: function () {
            if (this.my_user_id == this.ticket_object.created_by)
                return true;
            else return false;
        },
    }
};
</script>