<script setup>

import ServerComponent from "@/components/ServerComponent.vue";
import {ref} from "vue";

const data = ref(
    [
      {
        customer_id: 'user1',
        server_name: 'server7',
        server_type: 'vds'
      },
      {
        customer_id: 'user5',
        server_name: 'server2',
        server_type: 'dedicated'
      },
      {
        customer_id: 'user3',
        server_name: 'server4',
        server_type: 'hosting'
      }
    ]
)
const server = ref(null)
const form = ref(null)

const selectServer = (el) => {
  const item = data.value.find(item => {
    return item.customer_id === el
  })
  server.value = {
    ...item
  }
  if (form.value) {
    form.value.resetForm()
  }
}

const save = (event) => {
  data.value = data.value.map(el => {
    if (el.customer_id === event.customer_id) {
      el = event
    }
    return el
  })
}

const cancel = () => {
  server.value = null
}
</script>
<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">
        <el-col :span="24">
          <h4 class="mb-2">Список серверов</h4>
          <el-menu
              class="el-menu-vertical-demo"
              @select="selectServer"
          >
            <el-menu-item
                v-for="item in data"
                :key="item.customer_id"
                :index="item.customer_id">
              <span>{{item.server_name}}</span>
            </el-menu-item>
          </el-menu>
        </el-col>
      </el-aside>
      <el-main>
        <ServerComponent ref="form" v-if="server" v-model:server="server" @save="save" @cancel="cancel" />
      </el-main>
    </el-container>
  </div>
</template>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
