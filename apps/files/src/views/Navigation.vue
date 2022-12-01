<!--
  - @copyright Copyright (c) 2019 Gary Kim <gary@garykim.dev>
  -
  - @author Gary Kim <gary@garykim.dev>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
  -
  -->

<template>
	<NcAppNavigation>
		<NcAppNavigationItem v-for="view in parentViews"
			:key="view.id"
			:allow-collapse="true"
			:pinned="view.sticky"
			:title="view.name">
			<NcAppNavigationItem v-for="child in childViews[view.id]"
				:key="child.id"
				:title="child.name" />
		</NcAppNavigationItem>
	</NcAppNavigation>
</template>

<script>
import NcAppNavigation from '@nextcloud/vue/dist/Components/NcAppNavigation.js'
import NcAppNavigationItem from '@nextcloud/vue/dist/Components/NcAppNavigationItem.js'
import Navigation from '../services/Navigation.ts'

export default {
	name: 'Navigation',

	components: {
		NcAppNavigation,
		NcAppNavigationItem,
	},

	props: {
		// eslint-disable-next-line vue/prop-name-casing
		Navigation: {
			type: Navigation,
			required: true,
		},
	},

	data() {
		return {
			key: 'value',
		}
	},

	computed: {
		/** @return {Navigation[]} */
		views() {
			return this.Navigation.views
		},
		parentViews() {
			return this.views.filter(view => !view.parent)
		},
		childViews() {
			return this.views
				// filter parent views
				.filter(view => !!view.parent)
				// create a map of parents and their children
				.reduce((list, view) => {
					list[view.parent] = [...(list[view.parent] || []), view]
					return list
				}, {})
		},
		// sortedViews() {
		// 	return this.views.sort((a, b) => {
		// 		return a.order - b.order
		// 	})
		// },
	},
}
</script>

<style scoped lang="scss">

</style>
