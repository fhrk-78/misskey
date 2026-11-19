<!--
SPDX-FileCopyrightText:
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<MkContainer>
	<template #icon><i class="ti ti-news"></i></template>
	<template #header>{{ i18n.ts.pages }}</template>
	<div :class="$style.root" class="_gaps">
		<MkPagination v-slot="{items}" :paginator="paginator">
			<div class="_gaps">
				<!--TODO: panelをbgで置き換える暴挙、MkPagePreviewを拡張した方が良いのかしら-->
				<MkPagePreview v-for="page in items" :key="page.id" :page="page" style="--MI_THEME-panel:var(--MI_THEME-bg)"/>
			</div>
		</MkPagination>
		<MkButton rounded full @click="emit('showMore')">{{ i18n.ts.showMore }} <i class="ti ti-arrow-right"></i></MkButton>
	</div>
</MkContainer>
</template>

<script lang="ts" setup>
import { markRaw, computed } from 'vue';
import * as Misskey from 'misskey-js';
import MkPagePreview from '@/components/MkPagePreview.vue';
import MkPagination from '@/components/MkPagination.vue';
import { i18n } from '@/i18n';
import { Paginator } from '@/utility/paginator.js';
import MkButton from '@/components/MkButton.vue';
import MkContainer from '@/components/MkContainer.vue';

const emit = defineEmits<{
	(ev: 'showMore'): void;
}>();

const props = defineProps<{
	user: Misskey.entities.UserDetailed;
}>();

const paginator = markRaw(new Paginator('users/pages', {
	limit: 5,
	computedParams: computed(() => ({
		userId: props.user.id,
	})),
}));
</script>

<style lang="scss" module>
.root {
	padding: 8px;
}
</style>
