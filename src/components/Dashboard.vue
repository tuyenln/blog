<template>
<div class="container">
		<div class="row">
			<h2>Dashboard</h2>

			<form class="col-8 mx-auto bordered mt-5" @submit.prevent="createPost()">
					<div class="form-group">
						<label for="title">Title</label>
						<input v-model="post.title" type="title" name="title" class="form-control" :class="{'is-invalid': errors.title}" id="title">
						<div v-if="errors.title" class="invalid-feedback">
							{{ errors.title[0] }}
						</div>
					</div>
					<div class="form-group">
						<label for="content">Content</label>
						<textarea v-model="post.content" type="content" name="content" class="form-control" :class="{'is-invalid': errors.content}" id="content" rows="15"></textarea>
						<div v-if="errors.content" class="invalid-feedback">
							{{ errors.content[0] }}
						</div>
					</div>
					<button type="submit" class="btn btn-primary">
						<span v-if="loading" class="spinner-border"></span> Submit
					</button>
			</form>
		</div>
</div>
</template>

<script>

import BaseRequest from '../core/BaseRequest';

export default {
		data() {
				return {
					post: {
						title: '',
						content: ''
					},
					errors: {},
					loading: false,
				}
		},
		mounted(){
			this.checkLoggedIn();
		},
		methods: {
				checkLoggedIn: function() {
					BaseRequest.get('user')
					.then(function (response){
							console.log(response)
					})
					.catch(() =>  {
							this.$router.push({name: 'login'});
					})
				},

				createPost: function() {
					this.loading = true;

					BaseRequest.post('post',
						this.post
					)
					.then(() =>  {
						alert("Dang bai thanh cong");
						this.loading = false;
						this.post = {title: '', content: ''}
					})
					.catch( error =>  {
						this.loading = false;
						this.errors = error.response.data.errors;
					});
				}
		}
}

</script>

<style>

</style>