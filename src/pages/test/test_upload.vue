<template>
    <section>
        <table class="table">
            <thead>
                <tr>
                    <td>组件名</td>
                    <td>组件值</td>
                    <td>组件实例</td>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>field_file</td>
                    <td>{{field_file}}</td>
                    <td>
                        <field_file 
                            v-model="field_file"
                            :uri="uri"
                            valuefield="uri"
                        ></field_file>
                    </td>
                </tr>
                <tr>
                    <td>field_images</td>
                    <td>{{field_images}}</td>
                    <td>
                        <field_images
                            v-model="field_images"
                            :uri="uri"
                            valuefield="uri"
                        ></field_images>
                    </td>
                </tr>
                <tr>
                    <td>field_image</td>
                    <td>{{field_image}}</td>
                    <td>
                        <field_image
                            v-model="field_image"
                            :uri="uri"
                            valuefield="uri"
                        ></field_image>
                    </td>
                </tr>
            </tbody>
        </table>

    </section>
</template>

<script>
import field_file from "@/editor/field_file.vue"
import field_images from "@/editor/field_images.vue"
import field_image from "@/editor/field_image"
export default{
    config:{
        uri:"http://www.ci.com/index.php/index/upload",
    },
    components:{
        field_file,
        field_images,
        field_image,
    },
    data(){
        return {
            field_file:[{name:"abc.jpg",uri:"http://www.ci.com/uploads/1.jpg"}],
            field_images:[],
            field_image:[],
        }
    }
}

</script>