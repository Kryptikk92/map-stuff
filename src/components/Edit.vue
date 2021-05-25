<template>
    <textarea v-model="geoJsonEdit"></textarea>
</template>

<script>
export default {
    name: 'Edit',
    props: {
        geoJson: Object
    },
    computed: {
        geoJsonEdit: {
            set(value) {
                this.$emit('change', JSON.parse(value))
            },
            get() {
                return JSON.stringify(this.geoJson, null, ' ')
            }
        }
    }
}
</script>

<style>
textarea {
    width: 100%;
    height: 100%;
    resize: none;
}
</style>