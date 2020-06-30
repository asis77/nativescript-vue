<template>
    <Page class="page">
        <ActionBar class="action-bar" :title="title()"></ActionBar>
        <StackLayout class="form" row="0" col="0">
            <Button text="Add Person" v-if="!isAdd && !isEdit" @tap="add" class="btn btn-primary" />
            <StackLayout v-if="isAdd || isEdit">
                <StackLayout class="input-field">
                    <Label text="First Name" class="label font-weight-bold" />
                    <TextField class="input" v-model="input.firstname" />
                    <StackLayout class="hr-light"></StackLayout>
                </StackLayout>
                <StackLayout class="input-field">
                    <Label text="Last Name" class="label font-weight-bold" />
                    <TextField class="input" v-model="input.lastname" />
                    <StackLayout class="hr-light"></StackLayout>
                </StackLayout>
                <GridLayout rows="auto, auto" columns="*, *">
                    <Button text="Save" @tap="save" class="btn btn-primary" row="0" col="0" />
                    <Button text="Cancel" @tap="cancel" class="btn btn-primary" row="0" col="1" colSpan="2"  />
                </GridLayout>
            </StackLayout>
            <ScrollView orientation="vertical" height="100%" v-if="isList">
                <ListView for="person in $store.state.data" row="1" col="0">
                    <v-template>
                        <GridLayout columns="*, 40, 40" class="list-group-item">
                            <Label col="0" v-bind:text="person.firstname + ' ' + person.lastname" />
                            <Image col="1" src="~/assets/images/edit.png" style="background:green;padding:5;border-radius:5" @tap="edit(person)" width="25" height="25" class="right" vericalAlignment="center"/>
                            <Image col="2" src="~/assets/images/delete.png" style="background:red;padding:5;border-radius:5" @tap="remove(person)" width="25" height="25" class="right" vericalAlignment="center"/>
                        </GridLayout>
                    </v-template>
                </ListView>
            </ScrollView>
        </StackLayout>
    </Page>
</template>

<script lang="ts">
  export default {
    data() {
        return {
            input: {
                firstname: "",
                lastname: "",
            },
            isAdd:false,
            isEdit:false,
            isList:true,
        }
    },
    mounted() {
        this.load();
    },
    methods: {
        save() {
            if(!this.input.firstname || !this.input.lastname) {
                alert('First name and last name required.');
                return;
            }
            this.$store.dispatch("insert", this.input);
            this.clear();
            this.load();
        },
        load() {
            this.isAdd = false;
            this.isEdit = false;
            this.isList = true;
            this.$store.dispatch("query");
        },
        clear() {
            this.input.firstname = "";
            this.input.lastname = "";
        },
        add() {
            this.isAdd = true;
            this.isEdit = true;
            this.isList = false;
            this.clear();
        },
        cancel() {
            this.isAdd = false;
            this.isEdit = false;
            this.isList = true;
        },
        title() {
            if(this.isAdd)
                return 'Add Person';
            else if(this.isEdit)
                return 'Edit Person';
            else
                return 'Person';
        },
        edit(person) {
            this.isAdd = false;
            this.isEdit = true;
            this.isList = false;
            this.input.firstname = person.firstname;
            this.input.lastname = person.lastname;
        },
        remove(person) {
            alert(person.firstname+' '+person.lastname+' deleted');
        }
    }
  }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }
    .form {
        padding: 10;
    }
    .list-group-item {
        padding: 10;
    }
</style>
