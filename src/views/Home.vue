<template>
	<main v-if="!loading">
		<DataTitle :text="title" :dataDate="dataDate" />

		<DataBoxes :stats="stats" />

		<CountrySelect :countries="countries" />
	</main>

	<main class="flex flex-col align-center justify-center text-center" v-else>
		<div class="text-gray-500 text-3xl mt-10 mb-6">
			Fetching Data...
		</div>
	</main>
</template>

<script>
import DataTitle from "@/components/DataTitle"
import DataBoxes from "@/components/DataBoxes"
import CountrySelect from "@/components/CountrySelect";

export default {
	name: 'Home',
	components: {
		DataTitle,
		DataBoxes,
		CountrySelect
	},
	data() {
		return {
			loading: true,
			title: 'Global',
			dataDate: '',
			stats: {},
			countries: []
		}
	},
	methods: {
		async fetchCovidData() {
			const res = await fetch('https://api.covid19api.com/summary')
			const data = await res.json()
			return data
		}
	},
	async created() {
		const data = await this.fetchCovidData()
		
		this.dataDate = data.Date
		this.stats = data.Global
		this.countries = data.Countries
		this.loading = false
	}
}
</script>