<template>
    <Page class="page">
        <ActionBar class="action-bar" :title="title()"></ActionBar>
        <BottomNavigation>
            <TabStrip>
                <TabStripItem>
                    <Label text="Person"></Label>
                </TabStripItem>
                <TabStripItem>
                    <Label text="Year"></Label>
                </TabStripItem>
            </TabStrip>
            <TabContentItem>
                <StackLayout class="form" row="0" col="0">
                    <Button text="Add Person"  @tap="add" class="-outline" />
                    <StackLayout>
                        <StackLayout class="input-field">
                            <TextField class="-border" hint="First name" v-model="input.firstname" />
                        </StackLayout>
                        <StackLayout class="input-field">
                            <TextField class="-border" hint="Last name" v-model="input.lastname" />
                        </StackLayout>
                        <GridLayout rows="auto, auto" columns="*, *">
                            <Button text="Save" @tap="save" class="-outline" row="0" col="0" />
                            <Button text="Cancel" class="-outline" row="0" col="1" colSpan="2"  />
                        </GridLayout>
                    </StackLayout>
                    <PeopleList :list="list" />
                </StackLayout>
            </TabContentItem>
            <TabContentItem>
                <GridLayout>
                    <Label text="Content2" class="p-5"></Label>
                </GridLayout>
            </TabContentItem>
        </BottomNavigation>
    </Page>
</template>

<script lang="ts">
  
  import PeopleList from '@/components/People/List';

  export default {
    components: {
        PeopleList,
    },
    data() {
        return {
            input: {
                firstname: "",
                lastname: "",
            },
            list:[],
        }
    },
    mounted() {
        this.load();
        alert('hi123');
    },
    methods: {
        save() {
            if(!this.input.firstname) {
                alert('First name required.');
                return;
            }
            this.$store.dispatch("insert", this.input);
            this.clear();
            this.load();
        },
        load() {
            this.$store.dispatch("query");
            this.list = this.$store.state.data;
        },
        clear() {
            this.input.firstname = "";
            this.input.lastname = "";
        },
        add() {
            this.clear();
        },
        title() {
            return 'WCB';
            if(this.isAdd)
                return 'Add Person';
            else if(this.isEdit)
                return 'Edit Person';
            else
                return 'Person';
        }
    }
  }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }
</style>
