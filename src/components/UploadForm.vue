<template>
    <form @submit.prevent="uploadPhoto" id="uploadForm" class="d-flex flex-column justify-content-center mb-5 col-6">
        <span class="my-3"/>
        <div id="messages" :class="msgClass" :v-if="msgClass">
            <ul class="msg__list">
                <li v-for="(message, index) in messages" :key="index" class="msg__item">  
                    {{ message }}
                </li>
            </ul>
        </div>

        <div class="form-control d-flex flex-column mx-sm-3 mb-2">
            <label class="" for="description">Description</label>
            <textarea name="description" id="description" rows="2" class="my-4 mx-4"></textarea>
        </div>
        <div class="form-control d-flex flex-column mx-sm-3 mb-2">
            <label class="" for="photo">Image Upload</label>
            <input type="file" name="photo" class="my-4 mx-4" />
        </div>
        <div class="mx-sm-3 mb-2">
            <button class="btn btn-primary mb-2">Upload</button>
        </div>
    </form>

</template>

<script>
    export default {
        data() {
            return {
                csrf_token: '',
                messages: [],
                msgClass: ''
            }
        },
        created() {
            this.getCsrfToken();
        },
        methods: {
            uploadPhoto() {
                let self = this;

                self.messages = [];
                self.msgClass = "";

                let uploadForm = document.getElementById('uploadForm');
                let form_data = new FormData(uploadForm);

                fetch("/api/upload", {
                    method: 'POST',
                    body: form_data,
                    headers: {
                    'X-CSRFToken': this.csrf_token
                    }
                })
                .then(function (response) {
                    return response.json();
                })
                .then(function (data) {
                    // display a success message
                    console.log(data);

                    if ("errors" in data){
                        self.messages = data.errors;
                        self.msgClass = "alert alert-danger";
                        console.log(self.messages);
                    } else {
                        self.messages = ["File Upload Successful"];
                        self.msgClass = "alert alert-success";               
                        console.log(self.messages);
                    }
                })
                .catch(function (error) {
                    console.log(error);
                });

            },
            getCsrfToken() {
                let self = this;

                fetch("/api/csrf-token")
                .then((response) => response.json())
                .then((data) => {
                console.log(data);
                self.csrf_token = data.csrf_token;
                })
            }
        },
    };
</script>

<style scoped>
.msg__list {
    list-style: none;
}
.msg__item {
  display: inline-block;
  vertical-align: top;
}
</style>