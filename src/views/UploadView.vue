<template>
    <div class="container">
        <div class="form-head">
            <h1>Image Upload Form</h1>
        </div>
        <upload-form/>
    </div>
</template>

<script>
    import UploadForm from '../components/UploadForm.vue'

    export default {
        components: {UploadForm}
    };

</script>

<style scoped>

</style>