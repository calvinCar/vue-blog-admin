<template>
    <div>
      <Form ref="formInline" inline>
          <FormItem>
          <Button type="primary" icon="plus" @click="add">添加分类</Button>
          </FormItem>
      </Form>
      <Table border :loading="loading" :columns="columns" :data="data"></Table>
    </div>
</template>
<script>
import { category } from "@/api";
import { Button, Table, Form, FormItem } from 'iview';
export default {
  components: {
    Button,
    Table,
    Form,
    FormItem
  },
  data() {
    return {
      loading: true,
      columns: [
        {
          title: "分类名称",
          key: "name"
        },
        {
          title: "缩略名",
          key: "slug"
        },
        {
          title: "分类描述",
          key: "description"
        },
        {
          title: "排序",
          width: 100,
          align: "center",
          key: "sort"
        },
        {
          title: "文章数量",
          key: "count",
          width: 100,
          align: "center"
        },
        {
          title: "操作",
          key: "action",
          width: 150,
          align: "center",
          render: (h, params) => {
            return h("div", [
              h(
                "Button",
                {
                  props: {
                    type: "primary",
                    size: "small",
                    icon:'edit'
                  },
                  style: {
                    marginRight: "5px"
                  },
                  on: {
                    click: () => {
                      this.$router.push({
                        path: "/category/save",
                        query: {
                          id: params.row.id
                        }
                      });
                    }
                  }
                }
              ),
              h(
                "Button",
                {
                  props: {
                    type: "error",
                    size: "small",
                    icon:'trash-a'
                  },
                  on: {
                    click: () => {
                      if (confirm("确定要删除吗？")) {
                        this.delete(params.row.id, params.index);
                      }
                    }
                  }
                }
              )
            ]);
          }
        }
      ],
      data: []
    };
  },
  methods: {
    get() {
      category.getList().then(res => {
        this.data = res.data;
        this.loading = false;
      });
    },
    delete(id,index) {
      category.delete(id).then(res => {
   
        this.data.splice(index, 1);
      });
    },
    add(){
      this.$router.push('/category/save');
    }
  },
  mounted() {
    this.get();
  }
};
</script>