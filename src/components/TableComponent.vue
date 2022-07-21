<template>
    <h3>Table component</h3>
    <!-- <TableDataComponent /> -->
</template>

<script>
import { ref, onMounted } from 'vue';
/* import TableDataComponent from './TableDataComponent.vue'; */
import Exceljs from 'exceljs';

export default {
    name: 'TableDataComponent',
    props: {
    },
    setup() {
        const data = ref([]);

        const excelData = async () => {
            console.log('hello');
            const workbook = new Exceljs.Workbook();
            await workbook.xlsx.readFile('../../lista materiales v5.xlsx').then(() => {
                /* const worksheet = workbook.getWorksheet('resumen'); */
                // read firs row as data keys
                data.value = 'encontrado';
            })
        };

        onMounted(async () => {
            await excelData();
        });
        
        return data;
    }
}
</script>