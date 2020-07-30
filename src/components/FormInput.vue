<template>
    <div class="form-block">
        <div class="form-input">
            <div v-if="type === 'text'" :class="['default-input', 'form-el', error && 'form-el_error']">
                <input  type="text" @input="handleInput" v-model="content" :placeholder="field"/>
            </div>
            <div v-else-if="type === 'date'" :class="['date-input', 'form-el', error && 'form-el_error']">
                <span class="date-field">{{ field }}</span>
                <input type="date" :max="new Date().toISOString().split('T')[0]" @change="handleInput" v-model="content"/>
            </div>
            <div v-else-if="type === 'select' || type === 'multi-select'" :class="['select-input', 'form-el', error && 'form-el_error']">
                <select v-model="content" :multiple="type === 'multi-select'" @change="handleInput">
                    <option disabled value="">{{ field }}</option>
                    <option v-for="o in options" :key="o.id">{{ o.name }}</option>
                </select>
            </div>
            <div v-else-if="type === 'checkbox'" :class="['checkbox-input', 'form-el', error && 'form-el_error']">
                <input :id="name" type="checkbox" v-model="content">
                <label :for="name">{{ field }}</label>
            </div>
            <div v-if="required" class="form-isRequired">*</div>
        </div>
        <div v-if="error" class="form-error">{{ error }}</div>
    </div>
</template>

<script>
    export default {
        name: "FormInput",
        props: {
            value: [String, Array, Boolean],
            error: String,
            required: Boolean,
            field: String,
            type: String,
            name: String,
            options: Array
        },
        data() {
            return {
                content: this.value,
            }
        },
        methods: {
            handleInput() {
                this.$emit('input', this.content)
            },
        }
    }
</script>

<style scoped lang="scss">
    .form-block {
        display: flex;
        flex-direction: column;
        position: relative;
    }
    .form-input {
        display: flex;
        flex-direction: row;
        margin-bottom: 16px;
    }
    .form-error {
        position: absolute;
        font-size: 12px;
        bottom: 0;
        color: #c30000;
    }
    .form-el_error {
        input, select {
            border-bottom: 1px solid #c30000 !important;
            background: #fbfbfb;

            &::placeholder {
                color: #956161;
            }
        }
    }
    .form-el {
        display: flex;
        width: 97%;

        input {
            width: 100%;
            height: 24px;
            border: none;
            border-bottom: 1px solid #ccc;
            font-size: 18px;
        }

        option {
            padding: 2px 0;
        }

        select {
            width: 100%;
            padding: 8px 4px;
            border: none;
            border-bottom: 1px solid #ccc;
            font-size: 18px;
        }
    }
    .checkbox-input {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        input {
            height: 16px;
            width: auto;
            margin: 4px;
        }
    }
    .form-isRequired {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 3%;
        color: #dc0000;
        font-size: 20px;
    }
    .date-input {
        display: flex;
        flex-direction: column;
    }
    .date-field {
        font-size: 16px;
        padding: 8px 0 0 0;
    }
</style>