<template>
    <div class="form">
        <form v-if="!isSend">
            <div class="form-card">
                <div class="form-section">
                    <div class="form-title">Данные о пациенте</div>
                    <FormInput
                            v-for="(a, index) in attr"
                            :key="`attr-${index}`"
                            :type="a.type"
                            :error="a.error"
                            :required="a.isRequired"
                            :field="a.alt"
                            :options="a.options"
                            :name="a.name"
                            v-model="a.value" />
                </div>
                <div class="form-section">
                    <div class="form-title">Адрес</div>
                    <FormInput
                            v-for="(a, index) in address"
                            :key="`address-${index}`"
                            :type="a.type"
                            :error="a.error"
                            :required="a.isRequired"
                            :field="a.alt"
                            :options="a.options"
                            :name="a.name"
                            v-model="a.value" />
                </div>
                <div class="form-section">
                    <div class="form-title">Паспортные данные</div>
                    <FormInput
                            v-for="(a, index) in personId"
                            :key="`personId-${index}`"
                            :type="a.type"
                            :error="a.error"
                            :required="a.isRequired"
                            :field="a.alt"
                            :options="a.options"
                            :name="a.name"
                            v-model="a.value" />
                </div>
            </div>
            <div class="form-actions">
                <input class="submit" type="submit" value="Отправить" @click="send($event)">
            </div>
        </form>
        <TextBlock v-else>Данные успешно сохранены! (нет, ведь это тест)</TextBlock>
    </div>
</template>

<script>
    import FormInput from "@/components/FormInput";
    import TextBlock from "@/components/TextBlock";
    export default {
        name: "Form",
        components: {TextBlock, FormInput},
        data() {
            return {
                attr: [
                    { id: 1, name: "lastName", type: 'text', value: '', isRequired: true, alt: "Фамилия", error: null, options: [] },
                    { id: 2, name: "firstName", type: 'text', value: '', isRequired: true, alt: "Имя", error: null, options: [] },
                    { id: 3, name: "patronymic", type: 'text', value: '', isRequired: false, alt: "Отчество", error: null, options: [] },
                    { id: 4, name: "birthDay", type: 'date', value: '', isRequired: true, alt: "Дата Рождения", error: null, options: [] },
                    { id: 5, name: "phone", type: 'text', value: '', isRequired: true, alt: "Номер телефона", error: null, options: [] },
                    { id: 6, name: "sex", type: 'select', value: '', isRequired: false, alt: "Пол", error: null, options: this.options('genders') },
                    { id: 7, name: "group", type: 'multi-select', value: [], isRequired: true, alt: "Группа", error: null, options: this.options('groups') },
                    { id: 8, name: "doctor", type: 'select', value: '', isRequired: false, alt: "Лечащий врач", error: null, options: this.options('doctors') },
                    { id: 9, name: "isSms", type: 'checkbox', value: false, isRequired: false, alt: "Не отправлять СМС", error: null, options: [] },
                ],
                address: [
                    { id: 1, name: "index", type: 'text', value: '', isRequired: false, alt: "Индекс", error: null, options: [] },
                    { id: 2, name: "country", type: 'text', value: '', isRequired: false, alt: "Страна", error: null, options: [] },
                    { id: 3, name: "region", type: 'text', value: '', isRequired: false, alt: "Область", error: null, options: [] },
                    { id: 4, name: "city", type: 'text', value: '', isRequired: true, alt: "Город", error: null, options: [] },
                    { id: 5, name: "street", type: 'text', value: '', isRequired: false, alt: "Улица", error: null, options: [] },
                    { id: 6, name: "houseNumber", type: 'text', value: '', isRequired: false, alt: "Номер дома", error: null, options: [] },
                ],
                personId: [
                    { id: 1, name: "type", type: 'select', value: '', isRequired: true, alt: "Тип документа", error: null, options: this.options('docTypes')},
                    { id: 2, name: "series", type: 'text', value: '', isRequired: false, alt: "Серия", error: null, options: [] },
                    { id: 3, name: "number", type: 'text', value: '', isRequired: false, alt: "Номер", error: null, options: [] },
                    { id: 4, name: "issuedBy", type: 'text', value: '', isRequired: false, alt: "Кем выдан", error: null, options: [] },
                    { id: 5, name: "issuedDate", type: 'date', value: '', isRequired: true, alt: "Дата выдачи", error: null, options: [] },
                ],
                isSend: false,
            }
        },
        methods: {
            validate(d) {
                return d.map(
                    m => m.map(
                        n => {
                            if (n.isRequired && !n.value) {
                                n.error = 'Данное поле должно быть заполнено';
                                return false;
                            } else {
                                switch (n.type) {
                                    case "text":
                                        if (n.value.length >= 32) {
                                            n.error = 'Текст в форме не должен превышать 32 символов';
                                            return false;
                                        } else if (n.isRequired && n.value.length < 2) {
                                            n.error = 'Текст в поле не может быть менее 2 символов';
                                            return false;
                                        }
                                        break;
                                    case "select":
                                        break;
                                    case "multi-select":
                                        if (n.value.length < 1) {
                                            n.error = 'Выберите хотя бы одно поле'
                                            return false;
                                        }
                                        break;
                                    case "date":
                                        break;
                                    case "checkbox":
                                        break;
                                }
                            }

                            n.error = '';
                            return true;
                        }
                    )
                );
            },
            send(e) {
                e.preventDefault();
                let v = this.validate([this.attr, this.address, this.personId]).map(k => k.every(l => l === true));
                if (v.every(o => o === true))
                    this.isSend = true;
            },
            options(o) {
                switch(o) {
                    case "groups":
                        return [
                            {id: 1, name: "VIP"},
                            {id: 2, name: "Проблемные"},
                            {id: 3, name: "ОМС"}
                        ]
                    case "doctors":
                        return [
                            {id: 1, name: "Иванов"},
                            {id: 2, name: "Захаров"},
                            {id: 3, name: "Чернышева"}
                        ]
                    case "docTypes":
                        return [
                            {id: 1, name: "Паспорт"},
                            {id: 2, name: "Свидетельство о рождении"},
                            {id: 3, name: "Вод. удостоверение"}
                        ]
                    case "genders":
                        return [
                            {id: 1, name: "М"},
                            {id: 2, name: "Ж"}
                        ]
                    default:
                        return []
                }
            }
        }
    }
</script>

<style scoped lang="scss">
    .form {
        max-width: 800px;
        margin: 0 auto;
    }

    .form form {
        border: 1px solid #ccc;
        border-radius: 4px;
        padding: 8px 16px 16px 16px;
        display: flex;
        flex-direction: column;

        .submit {
            padding: 15px;
            background: rgb(0,52,255);
            background: linear-gradient(40deg, rgba(0,52,255,1) 0%, rgba(0,211,255,1) 100%);
            color: #fff;
            border: none;
            border-radius: 4px;
            margin-top: 4px;
            cursor: pointer;
            width: 100%;
        }
    }

    .form-title {
        font-size: 24px;
        color: #0090bb;
        padding-bottom: 16px;
        padding-top: 8px;
    }
</style>