<template>
    <Page class="page">
        <ActionBar class="action-bar" title="Add Person"></ActionBar>
        <StackLayout class="form" row="0" col="0">
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
                    <Button text="Clear" @tap="clear" class="btn btn-primary" row="0" col="1" colSpan="2"  />
                </GridLayout>

                <ScrollView orientation="vertical" height="100%">
                <ListView for="person in $store.state.data" row="1" col="0">
                    <v-template>
                        <StackLayout class="list-group-item">
                            <Label v-bind:text="person.firstname + ' ' + person.lastname" />
                        </StackLayout>
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
                lastname: ""
            }
        }
    },
    mounted() {
        this.load();
    },
    methods: {
        save() {
            this.$store.dispatch("insert", this.input);
            this.clear();
            this.load();
        },
        load() {
            this.$store.dispatch("query");
        },
        clear() {
            this.input.firstname = "";
            this.input.lastname = "";
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
