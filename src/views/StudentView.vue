<template>
    <v-container>
        <v-row>
            <v-col cols="12">
                <StudentData/>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
  components: {
    StudentData: () => import('../components/StudentData.vue')
  }
}
</script>

<style>

</style>
