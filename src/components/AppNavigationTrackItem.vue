<template>
	<NcAppNavigationItem
		:title="track.file_name"
		:class="{ 'subitem-disabled': !track.enabled }"
		:allow-collapse="false"
		:force-menu="false"
		@click="$emit('click', track)">
		<template slot="icon">
			<div v-if="track.loading"
				class="app-navigation-entry-icon icon-loading-small " />
			<div v-else-if="track.color"
				:class="{
					'icon-in-picker': true,
					'icon-road-thin': !track.enabled,
					'icon-road': track.enabled,
				}"
				:style="'background-color: ' + track.color + ';'" />
			<div v-else
				:class="{
					'icon-in-picker': true,
					'icon-road-thin': !track.enabled,
					'icon-road': track.enabled,
					'no-color': true,
				}" />
			<input v-show="false"
				ref="col"
				type="color"
				class="color-input"
				:value="track.color || '#0082c9'"
				@change="updateTrackColor"
				@click.stop="">
		</template>
		<template slot="counter">
			&nbsp;
		</template>
		<template slot="actions">
			<NcActionButton v-if="parentEnabled && track.enabled"
				icon="icon-search"
				:close-after-click="true"
				@click="$emit('zoom', track)">
				{{ t('maps', 'Zoom') }}
			</NcActionButton>
			<NcActionButton v-if="parentEnabled && track.enabled"
				icon="icon-category-monitoring"
				:close-after-click="true"
				@click="$emit('elevation', track)">
				{{ t('maps', 'Show track elevation') }}
			</NcActionButton>
			<NcActionButton v-if="parentEnabled && track.enabled && track.isUpdateable"
				:close-after-click="false"
				@click="onChangeColorClick">
				<template #icon>
					<div class="icon-colorpicker" />
				</template>
				{{ t('maps', 'Change color') }}
			</NcActionButton>
			<NcActionButton v-if="parentEnabled && track.enabled && track.isShareable && !isPublic()"
				icon="icon-share"
				:close-after-click="true"
				@click="$emit('add-to-map-track', track)">
				{{ t('maps', 'Copy to map') }}
			</NcActionButton>
		</template>
	</NcAppNavigationItem>
</template>

<script>
import NcAppNavigationItem from '@nextcloud/vue/dist/Components/NcAppNavigationItem'
import NcActionButton from '@nextcloud/vue/dist/Components/NcActionButton'
import {isPublic} from "../utils/common";

export default {
	name: 'AppNavigationTrackItem',

	components: {
		NcAppNavigationItem,
		NcActionButton,
	},

	props: {
		track: {
			type: Object,
			required: true,
		},
		parentEnabled: {
			type: Boolean,
			default: true,
		},
	},

	data() {
		return {
		}
	},

	computed: {
	},

	methods: {
		onChangeColorClick() {
			this.$refs.col.click()
		},
		updateTrackColor(e) {
			this.$emit('color', { track: this.track, color: e.target.value })
		},
		isPublic() {
			return isPublic()
		},
	},
}
</script>

<style lang="scss" scoped>
.subitem-disabled {
	opacity: 0.5;
}

.icon-colorpicker {
	opacity: 1;
	mask: url('../../img/color_picker.svg') no-repeat;
	-webkit-mask: url('../../img/color_picker.svg') no-repeat;
	background-color: var(--color-main-text);
	padding: 0 !important;
	mask-size: 16px auto;
	mask-position: center;
	-webkit-mask-size: 16px auto;
	-webkit-mask-position: center;
	width: 44px;
	height: 44px;
}
</style>
