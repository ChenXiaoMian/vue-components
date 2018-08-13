# pagination
分页组件

```
<page v-model="currentPage"
  :total-items="30"
  :items-per-page="4"
/>
```

```
export default {
  data() {
    return  {
      currentPage: 1
    }
  }
}
```