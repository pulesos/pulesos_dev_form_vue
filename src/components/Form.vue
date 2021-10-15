<template>
    <form class="form" @submit.prevent="checkForm">
        <h1 class="form__header">Регистрация</h1>
        <h2 class="form__subheader">Уже есть аккаунт? <a href="#">Войти</a></h2>
        <div class="form__group">
            <label for="name">Имя</label><br>
            <input 
                id="name" 
                class="form__group__control"
                :class="$v.form.name.$error ? 'is-invalid' : ''"
                v-model.trim='form.name'
                @blur='validateForm'
            >
            <span v-if="$v.form.name.$dirty && !$v.form.name.required" class="invalid-feedback">
                Обязательное поле
            </span>
        </div>
        <div class="form__group">
            <label for="email">Email</label><br>
            <input 
                id="email" 
                class="form__group__control"
                :class="$v.form.email.$error ? 'is-invalid' : ''"
                v-model.trim='form.email'
                @blur='validateForm'
            >
            <span v-if="$v.form.email.$dirty && !$v.form.email.required" class="invalid-feedback">
                Обязательное поле
            </span>
            <span v-if="$v.form.email.$dirty && !$v.form.email.email" class="invalid-feedback">
                Некорректный email
            </span>
        </div>
        <div class="form__group">
            <label for="tel">Телефон</label><br>
            <input 
                id="tel" 
                class="form__group__control"
                :class="$v.form.tel.$error ? 'is-invalid' : ''"
                v-model.trim='form.tel'
                @blur='validateForm'
                
            >
            <span v-if="$v.form.tel.$dirty && !$v.form.tel.required" class="invalid-feedback">
                Обязательное поле
            </span>
            <span v-if="$v.form.tel.$dirty && !$v.form.tel.numeric" class="invalid-feedback">
                В этом поле должны быть только числа
            </span>
            <span v-if="$v.form.tel.$dirty && !$v.form.tel.minLength" class="invalid-feedback">
                Не менее 11 символов!
            </span>
        </div>
        <div class="form__group">
            <label for="language">Язык</label><br>
            <select 
                id="language" 
                class="form__group__control" 
                v-model="form.country" 
                @blur='validateForm'>
                <option value="" disabled selected>Выберите язык</option>
                <option
                    v-for="(country, index) in countries"
                    :value="country.value"
                    :key="index"
                >
                {{ country.label }}
                </option>
            </select>
            <span v-if="$v.form.country.$dirty && !$v.form.country.required" class="invalid-feedback">
                Обязательное поле
            </span>
        </div>
        <div class="form__group check">
            <input 
                type="checkbox" 
                class="form-check-input" 
                id="agreeWithTerms"
                v-model="form.agreeWithTerms"
                @blur='validateForm'
            >
            <label class="form__check__label" :class="$v.form.agreeWithTerms.$error ? 'is-invalid' : ''" for="agreeWithTerms">Принимаю <a href="#">условия</a> использования</label><br>
            <span v-if="$v.form.agreeWithTerms.$dirty && !$v.form.agreeWithTerms.mustBeTrue" class="invalid-feedback">
                Почитайте условия!
            </span>
        </div>
    <button type="submit" class="btn" :disabled="disabledBtn">Сохранить</button>
    </form>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, email, numeric } from 'vuelidate/lib/validators'
export default {
    mixins: [validationMixin],
    data() {
        return {
            form: {
                name: '',
                email: '',
                tel: '',
                country: '',
                agreeWithTerms: false,
            },
            countries: [
                {
                    label: 'Русский',
                    value: 'Russian'
                },
                {
                    label: 'Английский',
                    value: 'English'
                },
                {
                    label: 'Китайский',
                    value: 'Chinese'
                },
                {
                    label: 'Испанский',
                    value: 'Spanish'
                }
            ]
        }
    },
    validations: {
        form: {
            name: {
                required
            },
            email: {
                required,
                email
            },
            tel: {
                required,
                minLength: minLength(10),
                numeric
            },
            country: {
                required
            },
            agreeWithTerms: {
                mustBeTrue(value) {
                    return value
                }
            }
        }
    },
    methods: {
        validateForm() {
            this.$v.form.$touch()
        },
        checkForm() {
            if (!this.$v.form.$error) {
                alert('Регистрация прошла успешно')
            }
        }
    },
    computed: {
        disabledBtn() {
            return this.$v.form.name.$invalid ||
                    this.$v.form.email.$invalid ||
                    this.$v.form.tel.$invalid ||
                    this.$v.form.country.$invalid ||
                    this.$v.form.agreeWithTerms.$invalid 
        }
    }
    
}
</script>

<style lang="scss" scoped>
label {
    width: 400px;
    height: 52px;
    margin-top: 5px;
    font-weight: 500;
    font-size: 16px;
    color: #756F86;
}
span {
    display: block;
    font-size: 14px;
    color: #FF7171;
}

input[type="checkbox"]:checked, 
input[type="checkbox"]:not(:checked) {
    position: absolute;
    left: -9999px;
}

input[type="checkbox"]:checked + label, 
input[type="checkbox"]:not(:checked) + label {
    position: relative;
    padding-left: 28px;
    line-height: 20px;
    cursor: pointer;
}

input[type="checkbox"]:checked + label:before, 
input[type="checkbox"]:not(:checked) + label:before {
    content: "";
    position: absolute;
    left: 0px;
    top: 0px;
    width: 18px;
    height: 18px;
    border: 1px solid #dddddd;
    background-color: #ffffff;
}

input[type="checkbox"]:checked + label:before, 
input[type="checkbox"]:not(:checked) + label:before {
    border-radius: 2px;
}



input[type="checkbox"]:checked + label:after, 
input[type="checkbox"]:not(:checked) + label:after {
    content: "";
    position: absolute;
    -webkit-transition: all 0.2s ease;
    -moz-transition: all 0.2s ease;
    -o-transition: all 0.2s ease;
    transition: all 0.2s ease;
}

input[type="checkbox"]:checked + label:after, 
input[type="checkbox"]:not(:checked) + label:after {
    left: 3px;
    top: 4px;
    width: 10px;
    height: 5px;
    border-radius: 1px;
    border-left: 4px solid #0880AE;
    border-bottom: 4px solid #0880AE;
    -webkit-transform: rotate(-45deg);
    -moz-transform: rotate(-45deg);
    -o-transform: rotate(-45deg);
    -ms-transform: rotate(-45deg);
    transform: rotate(-45deg);
}


input[type="checkbox"]:not(:checked) + label:after {
    opacity: 0;
}

input[type="checkbox"]:checked + label:after {
    opacity: 1;
}

.check {
    margin-top: 27px;
}
.btn {
    display: block;
    margin-top: 15px;
    width: 400px;
    height: 56px;
    background: #0880AE;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
    text-align: center;
    color: #EBF4F8;
}
.btn[disabled] {
    background: #a9bfd8;
}
.form {
    margin-left: 25px;
    width: 420px;
    min-height: 630px;
    &__header {
        font-size: 34px;
        line-height: 33px;
        color: #2C2738;
    }
    &__subheader {
        margin-top: -11px;
        font-weight: normal;
        font-size: 16px;
        line-height: 22px;
        color: #2C2738;
    }
    &__group {
        margin-top: 10px;
        &__control {
            width: 400px;
            height: 52px;
            margin-top: 20px;
            padding-left: 10px;
            background: #FFFFFF;
            border: 1px solid #DBE2EA;
            box-sizing: border-box;
            box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
            border-radius: 6px;
            font-size: 16px;
            line-height: 21px;
            color: black;
        }
    }
}



</style>