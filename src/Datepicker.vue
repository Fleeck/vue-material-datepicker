<style lang="scss">
    .datepicker-container {
        position: relative;
    }
</style>

<template>
    <div class="datepicker-container">
        <input type="text" value="{{ date_formatted }}" @click="showDatepicker">
        <input type="hidden" name="{{ name }}" value="{{ date_raw }}">
        <datepicker-agenda :show="isVisible"
                           :doubled="doubled"
                           :lang="lang"
                           @change="selectDate"
                           @cancel="hideDatePicker">
        </datepicker-agenda>
    </div>
</template>

<script>
    import moment from 'moment';

    import DatepickerAgenda from './components/DatepickerAgenda.vue';

    export default {
        components: {
            'datepicker-agenda': DatepickerAgenda
        },
        props: {
            doubled: { type: Boolean, default: false },
            format: { type: String, default: 'YYYY-MM-DD' },
            lang: { type: String, default: 'en' },
            name: { type: String, default: 'datepicker' }
        },
        data() {
            return {
                date: '',
                isVisible: false
            };
        },
        computed: {
            date_formatted() {
                if (this.date) return this.date.format(this.format);
                return '';
            },
            date_raw() {
                if (this.date) return this.date.format('YYYY-MM-DD');
                return '';
            }
        },
        methods: {
            selectDate(date) {
                this.$set('date', date);
                this.isVisible = false;
            },
            showDatepicker() {
                this.isVisible = true;
                setTimeout( () => document.addEventListener('click', this.hideDatePicker), 0);
            },
            hideDatePicker() {
                this.isVisible = false;
                document.removeEventListener('click', this.hideDatePicker);
            }
        }
    };
</script>