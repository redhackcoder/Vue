<template>
  <div id="app">
    <button @click="getSelectedRows()">Get Selected Rows</button>
        <ag-grid-vue style="width: 33%; height: 500px;"
                     class="ag-theme-alpine"
                     :columnDefs="columnDefs"
                     :rowData="rowData"
                     rowSelection="multiple"
                     :sideBar="true"
                     @grid-ready="onGridReady">
        </ag-grid-vue>
  </div>
</template>

<script>
    import {AgGridVue} from "ag-grid-vue";

    export default {
        name: 'App',
        data() {
            return {
                columnDefs: null,
                rowData: null,
                gridApi: null,
                columnApi: null,
                autoGroupColumnDef: null
            }
        },
        components: {
            AgGridVue
        },
        methods: {
            onGridReady(params) {
                this.gridApi = params.api;
                this.columnApi = params.columnApi;
            },
            getSelectedRows() {
                const selectedNodes = this.gridApi.getSelectedNodes();
                const selectedData = selectedNodes.map(node => node.data);
                const selectedDataStringPresentation = selectedData.map(node => node.make + ' ' + node.model).join(', ');
                alert(`Selected nodes: ${selectedDataStringPresentation}`);
            }
        },
        beforeMount() {
            this.columnDefs = [
                {headerName: 'Make', field: 'make', checkboxSelection:true, sortable:true, filter: true},
                {headerName: 'Model', field: 'model', sortable:true, filter: true},
                {headerName: 'Price', field: 'price', sortable:true, filter: true}
            ];

            this.autoGroupColumnDef = {
                headerName: 'Model',
                field: 'model',
                cellRenderer: 'agGroupCellRenderer',
                cellRendererParams: {
                    checkbox: true
                }
            };

            fetch('https://raw.githubusercontent.com/ag-grid/ag-grid/master/grid-packages/ag-grid-docs/src/sample-data/rowData.json')
                .then(result => result.json())
                .then(rowData => this.rowData = rowData);
        }
    }
</script>

<style >
  @import "../node_modules/ag-grid-community/dist/styles/ag-grid.css";
  @import "../node_modules/ag-grid-community/dist/styles/ag-theme-alpine.css";
 
</style>
