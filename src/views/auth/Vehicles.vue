<template>
	<section class="vehicles">
		<div class="container-awesome">
			<!-- <h1 class="title">
				Vehicles
			</h1> -->

			<div v-if="isLoading && !data">
				<div class="loader is-large"></div>
			</div>

			<b-loading is-full-page :active="isLoading && data ? true : false" />

			<div class="columns is-vcentered is-multiline" v-if="data && !errors">
				<div
					class="column is-full-mobile is-half-tablet is-one-third-widescreen"
					v-for="vehicle in data"
					:key="vehicle.id"
				>
					<div class="vehicle-card">
						<div class="vehicle-content">
							<div class="vehicle-info">
								<div class="icon is-medium">
									<TruckIcon />
								</div>
								<h1 class="title is-4">
									{{vehicle.name}}
								</h1>
									<router-link
										:to="{ name: 'vehicles.detail', params: { id: vehicle.id } }"
									>
										<span v-if="vehicle.broadcast">
											LIVE: {{vehicle.broadcast.createdBy.firstName}} {{vehicle.broadcast.createdBy.lastName}}
										</span>
										<span v-else>
											Not broadcasting
										</span>
									</router-link>
							</div>
							<div class="vehicle-actions">
								<b-tooltip type="is-dark" label="Edit">
									<router-link
										:to="{ name: 'vehicles.edit', params: { id: vehicle.id } }"
									>
										<span class="icon is-medium">
											<PencilIcon />
										</span>
									</router-link>
								</b-tooltip>
								<VehicleDelete :vehicle="vehicle" />
							</div>
						</div>
					</div>
				</div>
				<div class="column is-full-mobile is-half-tablet is-one-third-widescreen">
					<router-link
						class="vehicle-card is-add"
						:to="{ name: 'vehicles.create' }"
					>
						<span class="vehicle-content">
							<span class="icon is-large">
								<PlusIcon />
							</span>
						</span>
					</router-link>
				</div>
			</div>
		</div>
		<router-view />
	</section>
</template>

<script>
import VehicleDelete from '@/components/VehicleDelete.vue';
import PlusIcon from '@/components/icons/Plus.vue';
import PencilIcon from '@/components/icons/Pencil.vue';
import TruckIcon from '@/components/icons/Truck.vue';

export default {
	components: {
		VehicleDelete,
		TruckIcon,
		PencilIcon,
		PlusIcon
	},

	computed: {
		vehicleCreate() {
			return this.$store.state.vehicleCreate.data;
		},
		vehicleUpdate() {
			return this.$store.state.vehicleUpdate.data;
		},
		vehicleDelete() {
			return this.$store.state.vehicleDelete.data;
		},
		base() {
			return this.$store.state.vehicleList;
		},
		data() {
			return this.base.data;
		},
		isLoading() {
			return this.base.inProgress;
		},
		errors() {
			return this.base.error;
		}
	},

	watch: {
		vehicleCreate() {
			this.doLoad();
		},
		vehicleUpdate() {
			this.doLoad();
		},
		vehicleDelete() {
			this.doLoad();
		}
	},

	methods: {
		doLoad() {
			this.$store.dispatch('vehicleList');
		}
	},

	mounted() {
		this.doLoad();
	}
};
</script>
