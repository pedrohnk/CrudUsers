<template>
	<Header />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "@/components/Header.vue";

export default defineComponent({
	components: {
		Header,
	},
	data() {
		return {};
	},
});
</script>

<style>
.v-enter {
	opacity: 0;
	transform: translate3d(-20px, 0, 0);
}

.v-enter-active {
	transition: all 0.3s;
}
</style>
