<template>
    <div style="background: #ececec; padding: 30px">
      <a-row :gutter="16">
        <a-col :span="12">
          <a-card>
            <a-statistic
              title="Invites"
      
              style="margin-right: 50px"
              :value="info.totalInvites"
            >
              <template #prefix>
              </template>
            </a-statistic>
          </a-card>
        </a-col>
        <a-col :span="12">
          <a-card>
            <a-statistic
              title="Responses"
              :value="info.totalResponded"
            
              class="demo-class"
            >
              <template #prefix>
              </template>
            </a-statistic>
          </a-card>
        </a-col>
      </a-row>
      <a-row :gutter="16">
        <a-col :span="12">
          <a-card>
            <a-statistic
              title="Attending"
      
              style="margin-right: 50px"
              :value="info.attending"
            >
              <template #prefix>
              </template>
            </a-statistic>
          </a-card>
        </a-col>
        <a-col :span="12">
          <a-card>
            <a-statistic
              title="Kiddos"
              :value="info.totalChildren"
     
              class="demo-class"
            >
              <template #prefix>
              </template>
            </a-statistic>
          </a-card>
        </a-col>
      </a-row>
    </div>
  </template>
  <script>
  import { ArrowUpOutlined, ArrowDownOutlined } from '@ant-design/icons-vue';
  import axios from 'axios';

  import { defineComponent } from 'vue';
  export default defineComponent({
    components: {
      ArrowUpOutlined,
      ArrowDownOutlined,
    },
    data () {
    return {
      info: null
    }
  },
  mounted () {
    axios
      .get('http://localhost:3000/invites/data/metrics')
      .then(response => (this.info = response.data))
  }

  
  });

  </script>