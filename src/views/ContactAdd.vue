<template>
    <div v-if="contact" class="page">
        <h4>Thêm Liên hệ</h4>
        <CreateContactForm
        :contact="contact"
        @add:contact="createContact"
        />
        <p>{{ message }}</p>
    </div>
</template>
<script>
    import CreateContactForm from "@/components/CreateContactForm.vue";
    import ContactService from "@/services/contact.service";
    export default {
        components: {
            CreateContactForm,
        },
        // props: {
        //     id: { type: String, required: true },
        // },
        data() {
            return {
                contact: {},
                message: "",
            };
        },
        methods: {
            async createContact(data) {
                try {
                    await ContactService.create(data);
                    this.message = "Thêm liên hệ thành công";
                } catch (error) {
                    console.log(error);
                    
                }
            },
            
        },
        // created() {
        //     this.getContact(this.id);
        //     this.message = "";
        // },
    };
</script>