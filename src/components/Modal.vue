<template>
    <div ref="modalEle" class="modal fade modal-lg" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Detail Country</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <div class="row">
                    <div class="col-4">
                        <div class="card-img-top image-bg" >
                            <img  :src="item.flags.png" alt="">
                        </div>
                    </div>
                    <div class="col-8">
                        <h5 class="card-title" style="cursor: pointer;">
                            {{ item.name.official }}
                        </h5>
                        <ul>
                            <li>2 character Country Code: {{ item.cca2 }}</li>
                            <li>3 character Country Code: {{ item.cca3 }}</li>
                            <li>Native Country Name: {{ (item.name.nativeName[item.cca3.toLowerCase()])?.official ?? item.name.nativeName?.eng?.official }}</li>
                            <li>Alternative Country Name: 
                                <ul>
                                    <li v-for="(row,i) in item.altSpellings" :key="i">
                                    {{ row }}
                                    </li>
                                </ul>
                            </li>
                            <li>Country Calling Codes: {{ item.idd.root }}</li>
                           
                        </ul>
                    </div>
                </div>
            </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import { onMounted, ref  } from 'vue'
import { Modal } from "bootstrap";
const props = defineProps({
    item: {
        type: Object,
        default: {}
    }
})
let modalEle = ref(null);
let thisModalObj = null;

onMounted(() => {
  thisModalObj = new Modal(modalEle.value);
});

const showModalAction = () => {
    thisModalObj.show()
}

defineExpose({
    showModalAction
})

</script>