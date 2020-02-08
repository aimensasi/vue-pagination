# VuePagination is a vue component that provides core pagination functionality

# Installation

```
	npm install --save-dev vue-pagination
```

Then you can use it in your component...

```
	<template>
		<div>
			...

			<pagination-component :total="total" @paginate="onPaginate"/>
		</div>
	</template>
	<script>
		import VuePagination from 'vue-pagination';

		export default {
			...
			components: {
				'v-pagination': VuePagination,
			},
			...
		}
	</script>
```
