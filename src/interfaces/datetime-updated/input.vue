<template>
	<div v-if="value" class="datetime-updated">
		<v-icon name="today" color="--input-icon-color" />
		{{ displayValue }}
	</div>
	<v-input
		v-else
		:placeholder="
			$t(newItem ? 'interfaces.datetime-updated.now' : 'interfaces.datetime-updated.unknown')
		"
		icon-right="today"
		readonly
	/>
</template>

<script lang="ts">
import { createComponent, computed, PropType } from '@vue/composition-api';
import { DateTimeUpdatedOptions } from './types';
import useTimeFromNow from '@/compositions/time-from-now';

const { i18n } = require('@/lang/');

export default createComponent({
	props: {
		value: {
			type: String,
			default: null
		},
		newItem: {
			type: Boolean,
			required: true
		},
		options: {
			type: Object as PropType<DateTimeUpdatedOptions>,
			required: true
		}
	},
	setup(props) {
		const displayValue = computed<string | null>(() => {
			if (!props.value) return null;

			const date = new Date(props.value.replace(' ', 'T') + 'Z');

			if (props.options.showRelative) {
				return useTimeFromNow(date).value;
			}

			return i18n.d(date, 'long') + ' GMT';
		});

		return { displayValue };
	}
});
</script>

<style lang="scss" scoped>
.datetime-updated {
	position: relative;
	border: var(--input-border-width) solid var(--input-border-color);
	border-radius: var(--border-radius);
	height: var(--input-height);
	font-size: var(--input-font-size);
	padding: 4px 10px;
	display: flex;
	align-items: center;
	background-color: var(--input-background-color-disabled);

	.v-icon {
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		right: 10px;
	}
}
</style>
