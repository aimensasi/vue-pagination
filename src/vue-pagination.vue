<template>
	<div class="row d-flex">
		<div class="fixed-table-pagination ml-auto mr-3">
			<div class="pagination">
				<ul class="pagination">

					<li class="page-first" :class="{ disabled: currentPage == firstPage }" @click="onNavigate(firstPage)">
						<a>«</a>
					</li>
					<li class="page-pre" :class="{ disabled: currentPage == firstPage }" @click="onPrevious">
						<a>‹</a>
					</li>

					<li class="page-number" v-for="page in pages" @click="onNavigate(page)" :class="{ active: page == currentPage}">
						<a>{{ page }}</a>
					</li>

					<li class="page-next" :class="{ disabled: currentPage == lastPage }" @click="onNext">
						<a>›</a>
					</li>
					<li class="page-last" :class="{ disabled: currentPage == lastPage }" @click="onNavigate(lastPage)">
						<a>»</a>
					</li>

				</ul>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			total: {
				required: true,
				type: Number,
			},
			offset: {
				type: Number,
				default: 10,
			},
			limit: {
				type: Number,
				default: 10,
			}
		},
		data: function(){
			return {
				lastPage: 0,
				firstPage: 1,
				currentPage: 1,
				next: 0,
				previous: 0,
				from: 0,
				to: 0,
				pages: [],
			};
		},
		mounted(){
			this.setDefaults();
		},
		watch: {
			total: function(){
				this.setDefaults();
			}
		},
		methods: {
			onReset: function(){
				this.currentPage = 1;
				this.setDefaults();
			},
			setDefaults(){
				this.lastPage = Math.ceil(this.total / this.limit);

				if (this.lastPage < 5) {
					this.from = 1;
					this.to = this.lastPage;
				}else{
					this.from = this.currentPage - 2;
					this.to = this.currentPage + 2;
					if (this.from < 1) {
						this.from = 1;
						this.to = 5;
					}
					if (this.to >= this.lastPage) {
						this.to = this.lastPage;
						this.from = this.to - 5;
					}
				}

				this.pages = [];
				for(let i = this.from; i <= this.to; i++){
					this.pages.push(i);
				}
			},
			onNavigate: function(page){
				if (this.currentPage === page) {
					return ;
				}
				this.currentPage = page;


				let offset = this.currentPage - 1;
				offset = offset * this.limit;

				let query = { limit: this.limit, offset: offset};

				this.$emit('paginate', query);
				this.setDefaults();
			},
			onPrevious: function(){
				let prev = this.currentPage - 1;
				if (prev >= this.firstPage) {
					let page = prev;
					if ((prev - 1) <= this.firstPage) {
						prev = this.firstPage;
					}
					this.previous = prev;
					this.onNavigate(page);
				}
			},
			onNext: function(){
				let next = this.currentPage + 1;
				if (next <= this.lastPage) {
					let page = next;
					if ((next + 1) >= this.lastPage) {
						next = this.lastPage;
					}
					this.next = next;
					this.onNavigate(page);
				}
			}
		}
	}
</script>

<style lang="scss" scoped>

</style>
