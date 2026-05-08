<script lang="ts">
	// Get user gelocation

	let userLatitude = $state(0);
	let userLongitude = $state(0);

	let sharedLocation = $state(true);

	function getLocation() {
		navigator.geolocation.getCurrentPosition(
			function (position) {
				userLatitude = +position.coords.latitude.toFixed(7);
				userLongitude = +position.coords.longitude.toFixed(7);
				sharedLocation = true;
			},
			function (error) {
				console.log(error);
				sharedLocation = false;
			},
			{
				enableHighAccuracy: false
			}
		);
	}

	$effect(() => {
		getLocation();
	});

	$effect(() => {
		navigator.permissions.query({ name: 'geolocation' }).then(function (result) {
			result.onchange = function () {
				if (result.state === 'granted') {
					sharedLocation = true;
					getLocation();
				} else {
					sharedLocation = false;
				}
			};
		});
	});
</script>

<div class="flex flex-col items-center justify-center gap-4 px-4 py-16 sm:px-6 lg:px-8">
	<div class="flex flex-col items-center justify-center gap-4">
		<h1 class="text-4xl font-bold">VegiBytes</h1>
		<p class="text-lg">A platform for vegans to find vegan restaurants near you.</p>
	</div>
	{#if sharedLocation}
		<div class="flex flex-col items-center justify-center gap-4">
			<h1 class="text-4xl font-bold">Your location</h1>
			<p class="text-lg">
				Your current location is {userLatitude}, {userLongitude}
			</p>
		</div>
	{:else}
		<div class="flex flex-col items-center justify-center gap-4">
			<h1 class="text-4xl font-bold">Get your location</h1>
			<p class="text-lg">To find vegan restaurants near you, we need your location.</p>
			<button
				class="rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700"
				onclick={() => {
					getLocation();
				}}>Get my location</button
			>
		</div>
	{/if}
</div>
