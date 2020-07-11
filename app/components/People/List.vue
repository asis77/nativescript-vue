<template>
    <ScrollView orientation="vertical" height="100%" v-if="list">
        <ListView for="person in list" row="1" col="0">
            <v-template>
                <GridLayout columns="*, 35, 35" class="-separator">
                    <Label col="0" v-bind:text="person.firstname + ' ' + person.lastname" style="color:#000" />
                    <Image col="1" src="~/assets/images/edit.png"  width="16" height="16" vericalAlignment="center"/>
                    <Image col="2" src="~/assets/images/delete.png" width="16" height="16" vericalAlignment="center"/>
                </GridLayout>
            </v-template>
        </ListView>
    </ScrollView>
</template>

<script lang="ts">
  export default {
    props: {
        list: Array,
    },
    data() {
        return {
        }
    },
    mounted() {
    },
    methods: {
    }
  }
</script>