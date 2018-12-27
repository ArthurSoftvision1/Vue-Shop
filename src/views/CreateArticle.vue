<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 offset-md-2">
                <div class="card my-5">
                    <div class="card-body">
                        <picture-input 
                            accept="image/jpeg,image/png" 
                            size="10" 
                            button-class="btn btn-danger"
                            @change="onChange">
                        </picture-input>

                        <input type="text" placeholder="Title" class="form-control mb-3">
                        <wysiwyg v-model="content" />
                        <div class="text-center">
                            <button @click="createArticle()" class="btn-success btn btn-lg mt-3">Create Article</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import Axios from 'axios';
import PictureInput from 'vue-picture-input'

export default {

    components: {
        PictureInput
    },

    data() {
        return {
            content: "",
            image: null
        }
    },

    methods: {
        onChange(image) {
            this.image = image;
        },

        createArticle() {
            const form = new FormData();
            form.append('file', this.image);
            form.append('upload_preset', 'e2yuiszb')
            form.append('api_key', '729294993196428')

            Axios.post('https://api.cloudinary.com/v1_1/dghkkmsuy/image/upload', form)
                .then(res => console.log(res))
        }
    }
}
</script>