<template>
    <Form
    @submit="createContact"
    :validation-schema="contactFormSchema"
    >
        <div class="form-group">
            <label for="name">Tên</label>
            <Field
            name="name"
            type="text"
            class="form-control"
            :value="contactLocal.name"
            @input="contactLocal.name = $event.target.value"
            />
            <ErrorMessage name="name" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="email">E-mail</label>
            <Field
            name="email"
            type="email"
            class="form-control"
            :value="contactLocal.email"
            @input="contactLocal.email = $event.target.value"
            />
            <ErrorMessage name="email" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="address">Địa chỉ</label>
            <Field
            name="address"
            type="text"
            class="form-control"
            :value="contactLocal.address"
            @input="contactLocal.address = $event.target.value"
            />
            <ErrorMessage name="address" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="phone">Điện thoại</label>
            <Field
            name="phone"
            type="tel"
            class="form-control"
            :value="contactLocal.phone"
            @input="contactLocal.phone = $event.target.value"
            />
            <ErrorMessage name="phone" class="error-feedback" />
        </div>
        <div class="form-group form-check">
            <input
            name="favorite"
            type="checkbox"
            class="form-check-input"
            :value="contactLocal.favorite"
            @input="contactLocal.favorite = !contactLocal.favorite"
            />
            <label for="favorite" class="form-check-label">
                <strong>Liên hệ yêu thích</strong>
            </label>
        </div>
        <div class="form-group">
            <button class="btn btn-primary">Add</button>
            
        </div>
    </Form>
</template>
<script>
    import * as yup from "yup";
    import { Form, Field, ErrorMessage } from "vee-validate";
    export default {
        components: {
            Form,
            Field,
            ErrorMessage,
        },
        emits: ["add:contact"],
        props: {
            contact: { type: Object, required: true }
        },
        data() {
            const contactFormSchema = yup.object().shape({
                name: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
                email: yup
                .string()
                .email("E-mail không đúng.")
                .max(50, "E-mail tối đa 50 ký tự."),
                address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
                phone: yup
                .string()
                .matches(
                /((09|03|07|08|05)+([0-9]{8})\b)/g,
                "Số điện thoại không hợp lệ."
                ),
            });
            return {
            // Chúng ta sẽ không muốn hiệu chỉnh props, nên tạo biến cục bộ
            // contactLocal để liên kết với các input trên form
                contactLocal: this.contact,
                contactFormSchema,
            };
        },
        methods: {
            createContact() {
                this.$emit("add:contact", this.contactLocal);
            },
        },
    };
</script>
<style scoped>
    @import "@/assets/form.css";
</style>