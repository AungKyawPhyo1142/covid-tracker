<template>
    <select @change="onChange()" class="form-select block w-full border p-3 rounded mt-10" v-model="selected">
        <option value="0">Select a Country</option>
        <option v-for="(country,index) in countries" :key="index" :value="country.ID">
            {{country.Country}}
        </option>
    </select>
</template>

<script>
    export default {
        name:'CountrySelect',
        props: ['countries'],
        data(){
            return {
                selected:0
            }
        },
        methods: {
            onChange () {
                const country = this.countries.find((item)=>item.ID===this.selected);
                this.$emit('get-country',country);
            }
        }
    }
</script>

<style lang="stylus" scoped>

</style>
