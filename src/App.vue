<template>
<div id="app">
    <el-tree
            default-expand-all
            :props="defaultProps"
            :data="data"
            ref="tree"
            show-checkbox
            node-key="id"
            @check="check"
    >
        <div slot-scope="{node,data}">
            {{data.value}}-{{data.id}}---{{data.pid}}
        </div>

    </el-tree>
</div>
</template>

<script>

export default {
    name: 'App',
    components: {},
    data() {
        return {
            defaultProps: {
                children: 'children',
                label: 'value',
            },
            data: [],
            selected: [],
        };
    },
    mounted() {
        let data = [
            {
                'value': 'Accessories',
                'children': [
                    {
                        'value': 'green', 'children': [
                            {value: 'Canada'},
                        ],
                    },
                    {'value': 'red', 'children': []},
                    {'value': 'white', 'children': []},
                    {'value': 'yellow', 'children': []}],
            },
            {
                'value': 'Bikes',
                'children': [
                    {'value': 'blue', 'children': []},
                    {'value': 'green', 'children': []},
                    {'value': 'purple', 'children': []},
                    {'value': 'red', 'children': []},
                    {'value': 'white', 'children': []},
                    {'value': 'yellow', 'children': []}],
            },
            {
                'value': 'Cars',
                'children': [
                    {'value': 'blue', 'children': []},
                    {'value': 'green', 'children': []},
                    {'value': 'red', 'children': []},
                    {'value': 'white', 'children': []}],
            },
            {
                'value': 'Clothing',
                'children': [
                    {'value': 'blue', 'children': []},
                    {'value': 'green', 'children': []},
                    {'value': 'purple', 'children': []},
                    {'value': 'red', 'children': []},
                    {'value': 'white', 'children': []},
                    {'value': 'yellow', 'children': []}],
            },
            {
                'value': 'Components',
                'children': [
                    {'value': 'blue', 'children': []},
                    {'value': 'green', 'children': []},
                    {'value': 'red', 'children': []},
                    {'value': 'white', 'children': []}],
            }];
        let index = 0;
        function setId(data, pid) {
            data.forEach(item => {
                item.pid = pid - 1 >= 0 ? (pid - 1) : 'null';
                item.id = index;
                // 如果有子级
                index++;
                if (item.children?.length > 0) {
                    setId(item.children, index);
                }
            });
        }
        this.data = data
        // 递归设置id pid
        setId(data, null, index);
    },
    methods: {
        // 过滤树结构
        getSelected(data) {
            // 每个循环都要生成一个新得数组
            let newData = [];
            data.forEach(item => {
                // 如果满足条件
                if (this.selected.includes(item.id)) {
                    // 把item添加到新数组中，并取得索引
                    let index = newData.push(JSON.parse(JSON.stringify(item))) - 1;
                    // 如果改项有子级
                    if (item.children && item.children.length > 0) {
                        // 递归改项子级数据并且赋值给新数组的children
                        newData[index].children = this.getSelected(item.children);
                    }

                }
            });
            return newData;
        },
        check() {
            this.selected = [...this.$refs.tree.getCheckedKeys(false), ...this.$refs.tree.getHalfCheckedKeys()];
            console.log(this.selected);
            console.log(JSON.parse(JSON.stringify(this.getSelected(this.data))));
        },
    },
};
</script>

<style>

</style>
