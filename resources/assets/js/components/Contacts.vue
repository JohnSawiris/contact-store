<template>
    <div>
        <h1>Contacts</h1>
        <form action="#" @submit.prevent="edit ? updateContact(contact.id) :createContact()">
            <div class="form-group">
                <label for="name">Name: </label>
                <input v-model="contact.name" type="text" id="name" name="name" class="form-control">
            </div>
            <div class="form-group">
                <label for="email">Email: </label>
                <input v-model="contact.email" type="text" id="email" name="email" class="form-control">
            </div>
            <div class="form-group">
                <label for="phone">Phone: </label>
                <input v-model="contact.phone" type="text" id="phone" name="phone" class="form-control">
            </div>
            <div class="form-group">
                <button type="submit" v-if="!edit" class="btn btn-primary">New Contact</button>
                <button type="submit" v-if="edit" class="btn btn-success">Update Contact</button>
            </div>
        </form>
        <h1>Contact</h1>
        <ul class="list-group">
            <li v-for="contact in list" class="list-group-item" :key="contact.id">
                <strong>{{ contact.name }}</strong> {{ contact.email }} {{ contact.phone }}
                <button @click="deleteContact(contact.id)" class="float-right btn btn-danger btn-xs ml-1">Delete</button>
                <button @click="showContact(contact.id)" class="float-right btn btn-info btn-xs">Edit</button>
                
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "Contacts",
        data: function() {
            return {
                edit: false,
                list: [],
                contact: {
                    id: "",
                    name: "",
                    email: "",
                    phone: ""
                }
            }
        },
        mounted: function() {
            console.log("Contacts component loaded");
            this.fetchContactList();
        },
        methods: {
            fetchContactList: function() {
                console.log("Fetching Contacts...");
                let self = this;
                axios.get('api/contacts/')
                    .then(function(response) {
                        console.log(response.data);
                        
                        self.list = response.data;
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            createContact: function() {
                console.log("Creating Contact...");
                let self = this;
                let params = Object.assign({}, self.contact);
                axios.post("api/contact/store", params)
                    .then(function() {
                        self.contact.name = "";
                        self.contact.email = "";
                        self.contact.phone = "";
                        self.edit = false;
                        self.fetchContactList();
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            updateContact: function(id) {
                console.log("Updating contact" + id + "...");
                let self = this;
                let params = Object.assign({}, self.contact);
                axios.patch("api/contact/" + id, params)
                    .then(function() {
                        self.contact.name = "";
                        self.contact.email = "";
                        self.contact.phone = "";
                        self.edit = false;
                        self.fetchContactList();
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            deleteContact: function(id) {
                let self = this;
                axios.delete("api/contact/" + id)
                    .then(function(response) {
                        self.fetchContactList();
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            showContact: function(id) {
                let self = this;
                axios.get("api/contact/" + id)
                    .then(function(response) {
                        self.contact.id = response.data.id;
                        self.contact.name = response.data.name;
                        self.contact.email = response.data.email;
                        self.contact.phone = response.data.phone;
                    })
                self.edit = true;
            }
        }
    }
</script>

<style lang="sass">

</style>


