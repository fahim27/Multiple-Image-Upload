<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Multiple Image Upload</div>

                    <div class="card-body">
                        <div class="uploader"
                             @dragenter="onDragEnter"
                             @dragleave="onDragLeave"
                             @dragover.prevent
                             @drop="drop"
                             :class="{draging : isDraging}">
                            <div class="upload-control" v-show="images.length">
                                <label for="file">Select a file</label>
                            </div>
                            <div v-show="!images.length">
                                <i class="fa fa-cloud-upload"></i>
                                <p>Drag your file here</p>
                                <div>or</div>
                                <div class="file-input">
                                    <label for="file">select a file</label>
                                    <input type="file" id="file" @change="onInputChange" multiple>
                                </div>
                            </div>

                            <div class="images-preview" v-show="images.length">
                                <div class="img-wrapper" v-for="(image, index) in images" :key="index">
                                    <img :src="image" :alt="`Image Uplaoder ${index}`">
                                    <div class="details">
                                        <span class="name" v-text="files[index].name"></span>
                                        <span class="size" v-text="files[index].size"></span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        },
        data() {
            return {

                isDraging: false,
                dragCount: 0,
                files: [],
                images: []


            }
        },
        methods: {
            onDragEnter(e) {
                e.preventDefault();
                this.dragCount++
                this.isDraging = true;
            },
            onDragLeave(e) {

                e.preventDefault();
                this.dragCount--;
                if (this.dragCount <= 0) {
                    this.isDraging = false;

                }
            },
            drop(e) {
                e.preventDefault();
                e.stopPropagation();
                this.isDraging = false;
                const files = e.dataTransfer.files;
                console.log(files);
                Array.from(files).forEach(file => this.addImage(file));

            },

            onInputChange(e) {
                const files = e.target.files;
                Array.from(files).forEach(file => this.addImage(file));
            },
            addImage(file) {
                if (!file.type.match('image.*')) {
                    alert('this is not any kind of image');
                    return;
                }
                this.files.push(file);
                const img = new Image(),
                    reader = new FileReader();
                reader.onload = (e) => this.images.push(e.target.result);
                reader.readAsDataURL(file);
            }
        }
    }
</script>
