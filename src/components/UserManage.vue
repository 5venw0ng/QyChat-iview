<style>
    .pageclass
  {
    margin: 15px;
    float: right;
  }
</style>
<template>
    <div>
        <Table border :columns="userList" :data="tableData"></Table>
        <Page :total="total" :page-size="10" @on-change="changePage" class="pageclass" size="small" show-elevator show-sizer></Page>
    </div>
</template>
<script type="es6">
    export default {
    name: 'UserManage',
    data () {
    return {
        total: 0,
        userList: [
          {
            title: '编号',
            key: 'id'
          },
          {
            title: '会话内容存档开启成员列表',
            key: 'name'
          },
          {
            title: '操作',
            key: 'action',
            align: 'center',
            render: (h, params) => {
              return h('div', [
                h('Button', {
                  props: {
                    type: 'error',
                    size: 'small'
                  },
                  on: {
                    click: () => {
                      
                      this.showContext(params.row.id)
                    }
                  }
                }, '查看记录')
              ]);
            }
          }
        ],
        tableData: [],
      }
    },
    mounted(){
      this.request(1);

    },
    methods: {
      handleSubmit(account) {
        this.request(1)
      },
      showContext (userid) {
        this.$parent.$parent.$parent.lookRecord(userid)
      },
      request (currentPage){
        var that=this
        //this.$http.post(that.GLOBAL.serverPath + '/login/getAllowUsers',
        this.$http.post(that.GLOBAL.serverPath + '/login/queryAllUsers',
          {
          },
          {
            emulateJSON: true
          }
        ).then(function (res) {
          that.total=res.body.length
          res.body.res.forEach((e) => {
            let obj={}
            obj.id = e.userid
            obj.name = e.username
            that.tableData.push(obj)
          })
        })
      },
      changePage: function(page){
        this.request(page)
      }
    }
  }
</script>
