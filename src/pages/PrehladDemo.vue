<script setup lang="ts">
import { ref, onMounted, watchEffect  } from 'vue';
import { FilterMatchMode, FilterOperator } from "primevue/api";

// data
let state = ref()
const selectedState = ref();

const filters = ref({
	global: { value: null, matchMode: FilterMatchMode.CONTAINS },
	zozPzs: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.STARTS_WITH }],
	}, 
	zozNazov: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.STARTS_WITH }],
	}, 
	tzzDruh: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.STARTS_WITH }],
	}, 
	zozIco: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.STARTS_WITH }],
	}, 
	zozBeginDat: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.DATE_IS }],
	}, 
	zozEndDat: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.DATE_IS }],
	},
	pzsCisPob: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.EQUALS }],
	},
	sektorId: {
		operator: FilterOperator.AND,
		constraints: [{ value: null, matchMode: FilterMatchMode.EQUALS }],
	},
})

const loading = ref(true);

function clearFilter() {}

onMounted(() => {
	fetch('src/assets/layout/data/prehlad.json')
		.then(res => res.json())
		.then(d => {
			state.value = d.value
		});

	// setTimeout(() => {
		loading.value = false
	// }, 1000);
})
</script>

<template>
  <div class="grid">

    <div class="col-12">
			<div class="card ">
				<h5>Prehľad</h5>
				<DataTable :value="state" :paginator="true" class="p-datatable-gridlines mb-5" :rows="10"
            dataKey="id" :rowHover="true" v-model:selection="selectedState" v-model:filters="filters" filterDisplay="menu" :loading="loading"
            paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown" :rowsPerPageOptions="[10,25,50]"
            currentPageReportTemplate="Showing {first} to {last} of {totalRecords} entries"
            :globalFilterFields="['zozPzs','zozNazov']" responsiveLayout="scroll">
            
						<template #header>
							<div class="flex justify-content-between flex-column sm:flex-row">
								<span class="p-input-icon-left">
									<i class="pi pi-search" />
									<InputText v-model="filters['global']?.value" placeholder="Keyword Search" />

									<Button type="button" icon="pi pi-filter-slash" label="Clear" class="p-button-outlined ml-2" @click="clearFilter()"/>
								</span>

								<span >
									<Button type="button" label="New" icon="pi pi-plus" class="p-button p-button-primary mr-2" />
									<Button type="button" label="Export" icon="pi pi-upload" class="p-button-outlined p-button-primary" />
								</span>

							</div>
            </template>

            <template #empty>
                No customers found.
            </template>
            <template #loading>
                Loading customers data. Please wait.
            </template>

            <Column selectionMode="multiple" headerStyle="width: 3rem"></Column>

            <Column field="zozPzs" header="PZS" :sortable="true" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.zozPzs}}
                </template>
                <template #filter="{filterModel}">
                    <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Search by PZS"/>
                </template>
            </Column>

						<Column field="zozNazov" header="Nazov" :sortable="true" filterMatchMode="contains" style="min-width: 20rem">
                <template #body="{data}">
                    {{data.zozNazov}}
                </template>
                <template #filter="{filterModel}">
                    <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Search by Nazov"/>
                </template>
            </Column>

						<Column field="tzzDruh" header="Druh" :sortable="true" filterMatchMode="contains" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.tzzDruh}}
                </template>
                <template #filter="{filterModel}">
                    <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Search by Nazov"/>
                </template>
            </Column>

						<Column field="zozIco" header="IČO" :sortable="true" filterMatchMode="contains" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.zozIco}}
                </template>
                <template #filter="{filterModel}">
                    <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Search by Nazov"/>
                </template>
            </Column>

						<Column field="zozBeginDat" header="Začiatok" :sortable="true" dataType="date" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.zozBeginDat}}
                </template>
                <template #filter="{filterModel}">
                    <Calendar dateFormat="dd/mm/yy" placeholder="dd/mm/yyyy" v-model="filterModel.value" class="p-column-filter"/>
                </template>
            </Column>

						<Column field="zozEndDat" header="Koniec" :sortable="true" dataType="date" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.zozEndDat}}
                </template>
                <template #filter="{filterModel}">
                    <Calendar dateFormat="dd/mm/yy" placeholder="dd/mm/yyyy" v-model="filterModel.value" class="p-column-filter"/>
                </template>
            </Column>

						<Column field="pzsCisPob" header="Číslo pobočky" :sortable="true" filterMatchMode="contains" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.pzsCisPob}}
                </template>
                <template #filter="{filterModel}">
                    <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Search by Nazov"/>
                </template>
            </Column>

						<Column field="sektorId" header="Sektor id" :sortable="true" filterMatchMode="contains" style="min-width: 8rem">
                <template #body="{data}">
                    {{data.sektorId}}
                </template>
                <template #filter="{filterModel}">
                    <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Search by Nazov"/>
                </template>
            </Column>

						<Column header="Edit" style="min-width: 3rem">
							<template #body="{data}">
								<Button icon="pi pi-pencil" class="p-button-rounded p-button-success mr-2" />
								<Button icon="pi pi-trash" class="p-button-rounded p-button-danger mr-2" />
								<Button icon="pi pi-search" class="p-button-rounded p-button-info" />
							</template>
						</Column>
				</DataTable>

				<span class="block md:flex">
						<Button label="Primary btn" class="flex-1 p-button-primary  mr-1" /> 
						<template v-for="n in 9">
							<Button :label="'Button ' + n" class="flex-1 p-button-primary p-button-outlined  mr-1"/>
						</template>
						<Button label="Disabled btn" class="flex-1 p-button-primary p-button-outlined  mr-1" disabled /> 
						<Button icon="pi pi-angle-right" class="p-button-primary p-button-outlined" />
        </span>
			</div>
		</div>

  </div>
</template>

<style scoped>
.p-button.p-button-icon-only.p-button-rounded {
	width: 2rem;
	height: 2rem;

}

.p-inputtext {
	padding-top: .7rem;
	padding-bottom: .7rem;
}
</style>