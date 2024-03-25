<script>
	import TashMap from './lib/TashMap.svelte';
	import ImgSlider from './lib/ImgSlider.svelte';
	import LanduseBarChart from './lib/LanduseBarChart.svelte';
	import DistanceBarChart from './lib/DistanceBarChart.svelte';
</script>

<main class="bg-[#EEEEEE]">
	<section class="flex justify-center flex-col">
		<section class="Banner p-10 py-36 bg-[#627254] mb-5">
			<h1
				class="text-5xl max-w-[50rem] text-[#EEEEEE] font-bold text-center mx-auto"
			>
				Time-Lapsed Transformation: Monitoring the Evolution of Urban
				Green Spaces in Tashkent (2017-2023)
			</h1>
		</section>
		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			If you are closely following the news from Tashkent like I do,
			you will probably notice frequent reports of public parks being
			targeted for privatization. For example, <a
				class="text-[#627254] hover:underline"
				href="https://landsatlook.usgs.gov/explore?sat=LANDSAT_8&date=2019-10-30%7C2019-11-15"
			>
				the article on OpenDemocracy
			</a>
			mentions the redevelopment of Navoi National Park, a sizeable green
			space in Tashkent, which was designated Central Asia’s first eco-park
			in 2015 by the UN office in Uzbekistan. It also mentions multiple
			property redevelopment of Tashkent city center, which has generated
			significant concern from residents, academics, and civil society.
			Another example is
			<a
				class="text-[#627254] hover:underline"
				href="https://t.me/makarenko_channel">the news</a
			> that Tashkent authorities planned to cut down another park with
			1,000 trees and construct a business center, which has sparked protests
			from residents who were surprised by the ongoing construction, as
			it was not officially coordinated with them.
		</p>
		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			Driven by the concerns for Tashkent's Urban development, I
			(Rinat Musin) and my peers - Amir Sultanbekov and Rana Harris,
			decided to investigate how public green spaces in Tashkent have
			changed in the recent period from 2017 to 2023. However, there
			was one huge obstacle on our path - there was no public
			geographical data about Tashkent's land use. To overcome such an
			issue, we decided to use public satellite imagery from Google
			Earth Pro and then do the land use classification ourselves
			using QGIS Software.
		</p>
		<section
			class="flex justify-center flex-col p-10 pb-1 items-center"
		>
			<ImgSlider />
			<p class="max-w-[60rem] text-lg text-slate-500">
				A fragment of the setellite images of Tashkent in 2017 and
				2023. Drag the sliding bar to see the difference.
			</p>
		</section>

		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			After classifying each satellite image, we calculated the total
			area of lost and gained green space. To make it easier to see
			how Tashkent was affected by the change, we aggregated our
			calculations into the map of the city, where you can see how
			each city district was affected. It showed us that Almazar,
			Shaykhantokhur, Chilanzar, and Sergeli lost the most green space
			in terms of area out of all city districts.
		</p>
		<section class="flex flex-col justify-center items-center p-10">
			<TashMap />
			<p class="max-w-[60rem] text-lg text-slate-500 mx-auto">
				Interactive map of Tashkent districts. Hover over the district
				to see more details.
			</p>
		</section>

		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			But we didn't want to stop there yet. We wanted to find out
			which public sectors lost the most green land and which sectors
			gained the most land in 6 years. To do that we extracted land
			use by public sectors from OpenStreet View API and overlayed it
			with our existing data of lost and gained green land. We
			aggregated the results into the interactive bar chart graph you
			can see below. To make it more observable, we logged the area
			for each land use type.
		</p>

		<section class="flex justify-center flex-col items-center p-10">
			<LanduseBarChart />
			<p
				class="max-w-[60rem] text-lg text-slate-500 text-center mx-auto"
			>
				Interactive bar chart of green area lost/gained (in logged m<sup
					>2</sup
				>) by landuse type. Hover over the bars to see area in m<sup
					>2</sup
				> about each landuse type.
			</p>
		</section>

		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			From this bar chart we can see that almost in all public sectors
			the green area lost exceeds the area gained by 1-2 orders of
			magnitudes (by a factor of 10-100), with the exception of the
			Health sector which increased by 2 order of magnitudes. In terms
			of absolute numbers, the residential sector suffered the most
			with about 13 million square meters lost in total.
		</p>
		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			Last, but not least, we wanted to see if there is a correlation
			between the distance from the center of the city and the green
			area gained/lost. To do that we subdivided all city area into
			concentric rings with varying innter and outer radii. Then we
			devided the total amount of green area lost/gained within the
			ring by the area of that ring to get the percentage of green
			area lost/gained relative to the distance from the city. The bar
			chart below showcases our final findings.
		</p>

		<section class="flex justify-center flex-col items-center p-10">
			<DistanceBarChart />
			<p
				class="max-w-[60rem] text-lg text-slate-500 text-center mx-auto"
			>
				Interactive bar chart that shows percentage of green area
				lost/gained by distance from the center of the city. Hover
				over the bars to see more details.
			</p>
		</section>
		<p class="p-5 max-w-[50rem] mx-auto text-lg text-justify">
			This chart shows us that areas close to the city center
			(0m-1300m) and far away (7900m-9900m) have experienced much less
			green area loss. In fact, the city center area has actually
			gained more green land than it lost. On the other hand, it is
			the area in the middle ranges of 1300m-7900m that faced most of
			the redevelopment of green zones.
		</p>

		<p class="p-5 max-w-[50rem] mx-auto text-lg pb-20 text-justify">
			Due to the data limitations that we faced it is still hard for
			us to comment on the reasons why the redevelopment of green
			spaces took place. Did the government remove an empty green lot
			to build more public housing or did they redevelop the former
			park into a privatized property? Without the focused context of
			each removed green space, we can never answer those questions
			completely. Instead, we want our research to be a starting point
			for the larger discussion of how our public space changes over
			time and whether we think the change is heading in the right
			direction. We hope these interactive infographics will make
			citizens more aware of the changes happening in the city and
			provide them with spatial backing for their arguments, which can
			improve citizen governments. We also wanted to introduce our
			methodology, which can be adopted by other cities worldwide
			where spatial data is not readily available.
		</p>
	</section>
	<footer class="w-full p-10 bg-[#627254]"></footer>
</main>

<style>
	.Banner {
		background-image: linear-gradient(#26311da8, #26311da8),
			url('/forest.jpg');
		background-size: cover;
	}
</style>
