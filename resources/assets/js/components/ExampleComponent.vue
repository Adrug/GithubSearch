<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
				<h1>Github search</h1>
			</div>
		</div>
		<div class="row justify-content-center">
		 <div class="col-md-8">
				<div class="input-group mb-3">
				  <div class="input-group-append">
						<span class="input-group-text" id="basic-addon1">🔎</span>
					</div>
				  <input autofocus 
						autocomplete="off" 
						placeholder="You insterested repos" 
						v-model="request" 
						type="text"
						class="form-control"
						@keyup.enter="search">
				</div>
			</div>
		</div>
		<div class="row justify-content-center">
			<ul class="list-unstyled">
			  <li class="media"  v-for="(item,index) in page">
				<img class="mr-3 resize" v-bind:src="item.owner.avatar_url" v-bind:alt='item.owner.login'>
				<div class="media-body">
				  <h5 class="mt-0 mb-1"><a  v-bind:href='item.html_url'> {{ item.full_name }}</a></h5>
					{{ item.description }} <button class="btn btn-outline-dark">{{ item.language }}</button>
				</div>
			  </li>
			</ul>
		</div>
		<div v-if="total > 0" class="row justify-content-center">
			<nav>
                <ul class="pagination justify-content-center">
                    <li :class="'page-item' + (currentPage == 1 ? ' disabled' : '')">
                        <a class="page-link" href="#" tabindex="-1" @click="loadPage(currentPage - 1)">Previous</a>
                    </li>
                    <li :class="'page-item' + (n == currentPage ? ' active' : '')" v-for="(n, index) in totalPages" :key="index">
                        <a class="page-link" href="#" @click="loadPage(n)">{{n}}</a>
                    </li>
                    <li :class="'page-item' + (currentPage < totalPages ? '' : ' disabled')">
                        <a class="page-link" href="#" @click="loadPage(currentPage + 1)">Next</a>
                    </li>
                </ul>
            </nav>
		</div>

    </div>
</template>
<style>
img.resize {
  width:64px;
  height:64px;
}
</style>

<script>
	import axios from 'axios';
    export default {
		data() { return {
			page: null,
			repos: null,
			request: '',
			total: 0,
			pageSize: 5,
			currentPage: 1
		}},
		 computed: {
			totalPages: function (){return  Math.ceil(this.total / this.pageSize)}

		},

		methods: {
			async search() {
					let response = await axios.get(`https://api.github.com/search/repositories?q=${this.request}+language:php+language:javascript&sort=stars&order=desc`);
					this.repos = response.data.items;
					this.total = this.repos.length;
					this.loadPage(1);
					},
			loadPage(page) { 
				this.currentPage = page;
				let skip = this.pageSize * (page - 1);
				this.page =  this.repos.slice(skip, skip + this.pageSize)}
		}
		
				 
		
		
    }
</script>
