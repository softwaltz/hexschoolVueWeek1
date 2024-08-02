<template>
  <h2>庫存資料區</h2>
  <table>
    <thead>
      <tr>
        <th scope="col"></th>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
        <th scope="col">功能</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in items" v-bind:key="item.id">
        <td>{{ index }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.detail }}</td>
        <td>{{ item.price }}</td>
        <td>
          <button
            type="button"
            @click="--item.remaining < 0 ? (item.remaining = 0) : item.remaining"
          >
            -
          </button>
          {{ item.remaining }}
          <button type="button" @click="++item.remaining">+</button>
        </td>
        <td>
          <button type="button" @click="editItem(item)">修改</button>
          <button type="button" @click="deleteItem(item.id)">刪除</button>
        </td>
      </tr>
      <tr>
        <td>{{ items.length }}</td>
        <td>
          <input type="text" v-model="tempAddItem.name" />
        </td>
        <td>
          <input type="text" v-model="tempAddItem.detail" />
        </td>
        <td>
          <input type="number" v-model="tempAddItem.price" />
        </td>
        <td>
          <input type="number" v-model="tempAddItem.remaining" />
        </td>
        <td>
          <button
            type="button"
            v-if="tempAddItem.name != ''"
            :disabled="tempAddItem.detail == '' || tempAddItem.price <= 0"
            @click="addItem"
          >
            新增
          </button>
        </td>
      </tr>
    </tbody>
  </table>
  <br />
  <hr />
  <br />
  <h2>資料編輯區</h2>
  <table v-if="tempEditItem.name != '' || tempEditItem.detail != ''">
    <thead>
      <tr>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
        <th scope="col">功能</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>
          <input type="text" v-model="tempEditItem.name" />
        </td>
        <td>
          <input type="text" v-model="tempEditItem.detail" />
        </td>
        <td>
          <input type="number" v-model="tempEditItem.price" />
        </td>
        <td>
          <input type="number" v-model="tempEditItem.remaining" />
        </td>
        <td>
          <button type="button" @click="editItemOK">確定修改</button>
          <button type="button" @click="editItemCancel">取消修改</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref } from 'vue'

const items = ref([
  {
    id: 'AAA0',
    name: '珍珠奶茶',
    detail: '香濃奶茶搭配QQ珍珠',
    price: 50,
    remaining: 20
  },
  {
    id: 'AAA01',
    name: '冬瓜檸檬',
    detail: '清新冬瓜配上新鮮檸檬',
    price: 45,
    remaining: 18
  },
  {
    id: 'AAA2',
    name: '翡翠檸檬',
    detail: '綠茶與檸檬的完美結合',
    price: 55,
    remaining: 34
  },
  {
    id: 'AAA3',
    name: '四季春茶',
    detail: '香醇四季春茶，回甘無比',
    price: 45,
    remaining: 10
  },
  {
    id: 'AAA4',
    name: '阿薩姆奶茶',
    detail: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    remaining: 25
  },
  {
    id: 'AAA5',
    name: '檸檬冰茶',
    detail: '檸檬與冰茶的清新組合',
    price: 45,
    remaining: 20
  },
  {
    id: 'AAA6',
    name: '芒果綠茶',
    detail: '芒果與綠茶的獨特風味',
    price: 55,
    remaining: 18
  },
  {
    id: 'AAA7',
    name: '抹茶拿鐵',
    detail: '抹茶與鮮奶的絕配',
    price: 60,
    remaining: 20
  }
])

const tempAddItem = ref({
  id: '',
  name: '',
  detail: '',
  price: 0,
  remaining: 0
})

let tempEditItem = ref({
  id: '',
  name: '',
  detail: '',
  price: 0,
  remaining: 0
})

const addItem = () => {
  tempAddItem.value.id = new Date().getTime()
  items.value.push({ ...tempAddItem.value })
  tempAddItem.value.id = ''
  tempAddItem.value.name = ''
  tempAddItem.value.detail = ''
  tempAddItem.value.price = 0
  tempAddItem.value.remaining = 0
}

const deleteItem = (itemId) => {
  let index = items.value.map((i) => i.id).findIndex((i) => i == itemId)
  items.value.splice(index, 1)
}

const editItem = (item) => {
  tempEditItem.value = { ...item }
}

const editItemCancel = () => {
  tempEditItem.value = {
    id: '',
    name: '',
    detail: '',
    price: 0,
    remaining: 0
  }
}

const editItemOK = () => {
  let index = items.value.map((i) => i.id).findIndex((i) => i == tempEditItem.value.id)
  console.log(tempEditItem.value)
  items.value[index] = { ...tempEditItem.value }
  tempEditItem.value = {
    id: '',
    name: '',
    detail: '',
    price: 0,
    remaining: 0
  }
}
</script>
