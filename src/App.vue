<script setup lang="ts">
import { ref } from 'vue'
import 'devextreme/dist/css/dx.light.css';
import DxTreeView from 'devextreme-vue/tree-view';
import { ItemClickEvent } from 'devextreme/ui/tree_view';
interface TreeItem {
    id: string
    nodeType: string
    name: string
    parentId: string
    projectId?: string
}
const rawData: TreeItem[] = [
    {
        id: '1',
        nodeType: 'Modal',
        parentId: '5',
        name: '模型A',
        projectId: '1c'
    },
    {
        id: '2',
        nodeType: 'Modal',
        parentId: '5',
        name: '模型B',
        projectId: '1b'
    },
    {
        id: '3',
        nodeType: 'Modal',
        parentId: '5',
        name: '模型C',
        projectId: '1a'
    },
    {
        id: '6',
        nodeType: 'GasField',
        parentId: '-1',
        name: '气田A'
    },
    {
        id: '5',
        nodeType: 'WellArea',
        parentId: '6',
        name: '工区A'
    }
]

const data = ref(rawData.map(item => ({
    ...item,
    isGroup: item.nodeType !== 'Modal',
    expanded: item.nodeType !== 'Modal',
    parentId: item.parentId === '-1' ? void 0 : item.parentId
})))
const selectedItem = ref('')
const selectItem = ({itemData}: ItemClickEvent) => {
    const item = data.value.find(item => item.id === itemData?.id)
    if (!item || item.isGroup) {
        return
    }
    selectedItem.value = item.id === selectedItem.value ? '' : item.id
    console.log(selectedItem.value);
}

</script>

<template>
    <main class="container">
        <DxTreeView
            :items="data"
            data-structure="plain"
            parent-id-expr="parentId"
            :focus-state-enabled="false"
            :search-enabled="true"
            search-mode="contains"
            :search-editor-options="{
                placeholder: '井名模糊搜索',
            }"
            item-template="item"
            display-expr="name"
            @item-click="selectItem"
        >
            <template #item="{data}">
                <div v-if="data.isGroup">{{data.name}}</div>
                <div class="tree-item" v-else>
                    <input type="radio" :value="data.id" v-model="selectedItem" />
                    <span>{{data.name}}</span>
                </div>
            </template>
        </DxTreeView>
    </main>
</template>

<style>
.container {
    width: 300px;
    margin: 50px auto 0;
}
.tree-item {
    display: flex;
    align-items: center;
    column-gap: 6px;
}
.tree-item input[type="radio"] {
    margin: 0;
}
</style>
