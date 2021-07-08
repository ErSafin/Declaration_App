<template>
        <h5 class="mx-5 text">Исполнитель</h5>
        <select id="inputUser" v-model="id" class="form-control col-md-4 mb-1 mx-5 " autofocus 
                @change="this.$emit('create', this.id)" selected>
            <option v-for="(value,key) in users" v-bind:key="key" v-bind:value="value.id">
                    {{value.name}}
            </option>
        </select>

</template>

<script>
    import newuser from "/menuApp.json"
    export default {
        data() {
            return {
                users: newuser,
                id: 0
            }
        }
    }
</script>
