<template>
    <select id="spWork" v-model="work" class="form-control col-md-10 mb-3 mx-4" size="3" @change="this.$emit('work', work)" disabled>
        <option v-for="work in outWork" v-bind:key="work" v-bind:value="work">
            {{work.data}}  |  {{work.statusApp}}  |  {{work.typeApp}} :  {{work.txtApp}}
        </option>
    </select>
</template>

<script>
    export default {
        props: {
            outWork: {
                type: Array,
                required: true
            }
        }
        
    }
</script>