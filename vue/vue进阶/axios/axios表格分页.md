今天来写一个分页，表格分页在实际项目中经常用到，之前也写过关与bootstrap table 里面的表格分页，道理都差不多一样的，Element UI也有自己的组件可以用，话不多说，直接上代码了。

接着之前的项目继续写，打开一个vue界面，在里面写如下代码：

```
<template>
    <div>
        <el-table :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)" style="width: 100%">
            <el-table-column prop="id" label="日期" width="180">
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="180">
            </el-table-column>
            <el-table-column prop="price" label="地址">
            </el-table-column>
        </el-table>
        <div class="pagination">
            <el-pagination 
                @size-change="handleSizeChange" 
                @current-change="handleCurrentChange" 
                :current-page="currentPage" 
                :page-sizes="[5, 10, 20, 40]" 
                :page-size="pagesize" 
                layout="total, sizes,prev, pager, next" 
                :total="tableData.length" 
                prev-text="上一页" 
                next-text="下一页">
            </el-pagination>
        </div>
    </div>
</template>
<script>
    import axios from "axios";
    export default {
        name: "app",
        data() {
            return {        
                currentPage: 1, //默认显示页面为1
                pagesize: 5, //    每页的数据条数
                tableData: [], //需要data定义一些，tableData定义一个空数组，请求的数据都是存放这里面
            }
        },
        mounted() {
            this.getData();
        },
        methods: {
            getData() {
                axios.get('http://localhost:8080/api/test').then(response => {
                    console.log(response.data);
                    this.tableData = response.data;
                }, response => {
                    console.log("error");
                });
            },
            //每页下拉显示数据
            handleSizeChange: function(size) {
                this.pagesize = size;
                /*console.log(this.pagesize) */
            },
            //点击第几页
            handleCurrentChange: function(currentPage) {
                this.currentPage = currentPage;
                /*console.log(this.currentPage) */
            },

        }
    }
</script>
```
test.json
```
[  
        {  
            "id": 0,  
            "name": "Item 0",  
            "price": "徐家汇"  
        },  
        {  
            "id": 1,  
            "name": "Item 1",  
            "price": "$1"  
        },  
        {  
            "id": 2,  
            "name": "Item 2",  
            "price": "$2"  
        },  
        {  
            "id": 3,  
            "name": "Item 3",  
            "price": "徐家汇"  
        },  
        {  
            "id": 4,  
            "name": "Item 4",  
            "price": "徐家汇"  
        },  
        {  
            "id": 5,  
            "name": "Item 5",  
            "price": "$5"  
        },  
        {  
            "id": 6,  
            "name": "Item 6",  
            "price": "$6"  
        },  
        {  
            "id": 7,  
            "name": "Item 7",  
            "price": "$7"  
        },  
        {  
            "id": 8,  
            "name": "Item 8",  
            "price": "徐家汇"  
        },  
        {  
            "id": 9,  
            "name": "Item 9",  
            "price": "$9"  
        },  
        {  
            "id": 10,  
            "name": "Item 10",  
            "price": "$10"  
        },  
        {  
            "id": 11,  
            "name": "Item 11",  
            "price": "$11"  
        },  
        {  
            "id": 12,  
            "name": "Item 12",  
            "price": "徐家汇"  
        },  
        {  
            "id": 13,  
            "name": "Item 13",  
            "price": "$13"  
        },  
        {  
            "id": 14,  
            "name": "Item 14",  
            "price": "$14"  
        },  
        {  
            "id": 15,  
            "name": "Item 15",  
            "price": "$15"  
        },  
        {  
            "id": 16,  
            "name": "Item 16",  
            "price": "徐家汇"  
        },  
        {  
            "id": 17,  
            "name": "Item 17",  
            "price": "$17"  
        },  
        {  
            "id": 18,  
            "name": "Item 18",  
            "price": "$18"  
        },  
        {  
            "id": 19,  
            "name": "Item 19",  
            "price": "徐家汇"  
        },
        
        {  
            "id": 20,  
            "name": "Item 20",  
            "price": "$20"  
        }  
    ]
```

参考文档:

https://www.jianshu.com/p/afdfbec53ded   Vue框架Element UI教程-axios表格分页（九）
