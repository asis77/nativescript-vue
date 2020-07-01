<template>
    <Page class="page">
        <ActionBar class="action-bar" :title="title()"></ActionBar>

        <BottomNavigation>
            <TabStrip>
                <TabStripItem>
                    <Label text="Person"></Label>
                    <!-- <Image src="~/assets/images/edit.png"></Image> -->
                </TabStripItem>
                <TabStripItem>
                    <Label text="Year"></Label>
                    <!-- <Image src="~/assets/images/edit.png"></Image> -->
                </TabStripItem>
                <TabStripItem>
                    <Label text="Logout"></Label>
                    <!-- <Image src="~/assets/images/edit.png"></Image> -->
                </TabStripItem>
            </TabStrip>

            <TabContentItem>
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
                                <GridLayout columns="*, 35, 35" class="list-group-item">
                                    <Label col="0" v-bind:text="person.firstname + ' ' + person.lastname" />
                                    <Image col="1" src="~/assets/images/edit.png" @tap="edit(person)" width="16" height="16" vericalAlignment="center"/>
                                    <Image col="2" src="~/assets/images/delete.png" @tap="remove(person)" width="16" height="16" vericalAlignment="center"/>
                                </GridLayout>
                            </v-template>
                        </ListView>
                    </ScrollView>
                </StackLayout>
            </TabContentItem>

            <TabContentItem>
                <GridLayout>
                    <Label text="Content2"></Label>
                </GridLayout>
            </TabContentItem>
        </BottomNavigation>




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
            return 'WCB';
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
