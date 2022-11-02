<template>
  <span style="margin-left:20px">
    <!-- <p :data-source="dataSource">Total Attending: {{ dataSource.filter((d) => d.attending === 1).length }} /
      {{ dataSource.length }} </p>
    <p :data-source="dataSource">Total Responded: {{ dataSource.filter((d) => d.hasResponded === 1).length }} /
      {{ dataSource.length }}</p> -->
    <a-input v-model:value="search" placeholder="Search a name..." style="width:15%;" />
  </span>
<div style="margin-top:20px">


  <a-table :columns="columns" :row-key="record => record.id" :data-source="dataSource" :pagination="pagination"
    :loading="loading" @change="handleTableChange">
    <template #bodyCell="{ column, text }">
      <template v-if="column.dataIndex === 'name'">{{ text }}</template>
      <template v-if="column.dataIndex === 'hasResponded'">{{ text === 1 ? 'Yes' : 'No' }}</template>
      <template v-if="column.dataIndex === 'attending'">{{ text === 1 ? 'Yes' : 'No' }}</template>
      <template v-if="column.dataIndex === 'hasChildren'">{{ text === 1 ? 'Yes' : 'No' }}</template>
      <template v-if="column.dataIndex === 'partner'">{{ text ? text : 'N/A' }}</template>
      <template v-if="column.dataIndex === 'partnerAttending'">{{ text === 1 ? 'Yes' : 'No' }}</template>
      <template v-if="column.title === 'Edit/Delete'"><a href="#">Edit</a>
        <a href="#">Delete</a>
      </template>

  
    </template>
  </a-table>
</div>
</template>
<script>
import { usePagination } from 'vue-request';
import { computed, defineComponent, useCssModule } from 'vue';
import axios from 'axios';
const columns = [{
  title: 'Name',
  dataIndex:'name',
  sorter: true,
  width: '20%',
}, {
  title: 'Responded',
  dataIndex: 'hasResponded',
  filters: [{
    text: 'Yes',
    value: 1,
  }, {
    text: 'No',
    value: 0,
  }],
  width: '20%',
}, {
  title: 'Attending',
  dataIndex: 'attending',
  width: '20%',
  filters: [{
    text: 'Yes',
    value: 1,
  }, {
    text: 'No',
    value: 0,
  }]
},
{
  title: 'Partner',
  dataIndex: 'partner',
  width: '20%',
},
{
  title: 'Partner Attending',
  dataIndex: 'partnerAttending',
  width: '20%',
  filters: [{
    text: 'Yes',
    value: 1,
  }, {
    text: 'No',
    value: 0,
  }]
},
{
  title: 'Has Children',
  dataIndex: 'hasChildren',
  filters: [{
    text: 'Yes',
    value: 1,
  }, {
    text: 'No',
    value: 0,
  }],
  width: '20%',
},
{
  title: 'Total Children Attending',
  dataIndex: 'totalChildrenAttending',
  width: '20%',
},
{
  title: 'Edit',
  width: '20%',
}];

const queryData = params => {
  return axios.get('http://localhost:3000/invites/', {
    params,
  });
};

export default defineComponent({
  setup() {
    const {
      data: allData,
      run,
      loading,
      current,
      pageSize,
    } = usePagination(queryData, {
      formatResult: res => res.data,
     
      pagination: {
        currentKey: 'page',
        pageSizeKey: 'results',
      },
   
    });
    const pagination = computed(() => ({
      total: allData.length,
      current: current.value,
      pageSize: pageSize.value,
    }));

    const handleTableChange = (pag, filters, sorter) => {
      run({
        results: pag.pageSize,
        page: pag?.current,
        sortField: sorter.field,
        sortOrder: sorter.order,
        ...filters,
      });
    };

    return {
      allData,
      dataSource: Object.create(allData),
      pagination,
      loading,
      columns,
      handleTableChange,
    };
  },

  data() {
    return {
      search: null,
    }
  },
  watch: {
    search: function (value) {
      console.log(this.allData)
      if(value){
        this.dataSource = this.dataSource.filter((d) => d.name.toUpperCase().includes( value.toUpperCase()))
      } else {
        this.dataSource = this.allData;
      }

    },
  }


});


</script>