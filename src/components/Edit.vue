<template>
    <a-form :model="formState" :label-col="labelCol" :wrapper-col="wrapperCol">
        <a-form-item label="Name">
            <a-input v-model:value="formState.invitee.name" />
        </a-form-item>

        <a-form-item label="Has Responded">
            <a-radio-group v-model:value="formState.invitee.hasResponded">
                <a-radio value="1">Yes</a-radio>
                <a-radio value="0">No</a-radio>
            </a-radio-group>
        </a-form-item>
        <a-form-item label="Attending">
            <a-radio-group v-model:value="formState.invitee.attending">
                <a-radio value="1">Yes</a-radio>
                <a-radio value="0">No</a-radio>
            </a-radio-group>
        </a-form-item>
        <a-form-item label="Partner Attending">
            <a-radio-group v-model:value="formState.invitee.partnerAttending">
                <a-radio value="1">Yes</a-radio>
                <a-radio value="0">No</a-radio>
            </a-radio-group>
        </a-form-item>
        <a-form-item label="Partner">
            <a-input v-model:value="formState.invitee.partner" type="textarea" />
        </a-form-item>

        <a-form-item label="Has Children">
            <a-radio-group v-model:value="formState.invitee.hasChildren">
                <a-radio value="1">Yes</a-radio>
                <a-radio value="0">No</a-radio>
            </a-radio-group>
        </a-form-item>
        <a-form-item label="Total Children">
            <a-input v-model:value="formState.invitee.totalChildrenAttending" type="number" min="0" />
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 14, offset: 4 }">
            <a-button type="primary" @click="onSubmit">Create</a-button>
            <a-button style="margin-left: 10px">Cancel</a-button>
        </a-form-item>
    </a-form>
</template>
<script>
import { defineComponent, reactive, toRaw, ref } from 'vue';
import axios from 'axios';

export default defineComponent({
    data() {
        return {
            info: null
        }
    },
    mounted() {
        axios
            .get('http://localhost:3000/invites/1')
            .then(response => (this.info = response.data))
    },
    name: 'Edit',
    props: {
        fetch_url: {
            type: String,
        }
    },
    setup(props) {

        let info = ''
        const loading = ref(false)
        const formState = reactive({
            info: info,
            delivery: false,
            type: [],
            resource: '',
            invitee: [],
            getinvitee

        });


        async function getinvitee() {
            loading.value = true;
            
            if (props.fetch_url){
                await axios.get("http://localhost:3000/invites/1").then(response => {
                formState.invitee = response.data;
                formState.invitee.hasResponded = formState.invitee.hasResponded.toString()
                formState.invitee.partnerAttending = formState.invitee.partnerAttending.toString()
                formState.invitee.hasChildren = formState.invitee.hasChildren.toString()
                formState.invitee.attending = formState.invitee.attending.toString()
            }).finally(() => {
                loading.value = false;
            })
            }else{
                formState.invitee = {
                    name: 'Enter a name...',
                    hasResponded: '0',
                    partnerAttending:'0',
                    hasChildren:'0',
                    attending:'0',
                    totalChildrenAttending:0,
                    partner:''
                }
            }
           
        }



        const onSubmit = () => {
            console.log(formState)
         
            console.log('submit!', toRaw(formState));
        };

        return {
            labelCol: {
                style: {
                    width: '150px',
                },
            },
            wrapperCol: {
                span: 14,
            },
            formState,
            onSubmit,
            getinvitee,
            loading,
            formState
        };
    },

    async created() {
        await this.getinvitee()
    }




});
</script>