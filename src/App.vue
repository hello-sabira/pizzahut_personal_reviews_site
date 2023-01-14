<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const reviews = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const reviews_asc = computed(() => reviews.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(reviews, (newVal) => {
	localStorage.setItem('reviews', JSON.stringify(newVal))
}, {
	deep: true
})

const addreview = () => {
	if (input_content.value.trim() === '' || input_category.value === null) {
		return
	}

	reviews.value.push({
		content: input_content.value,
		category: input_category.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}

const removereview = (review) => {
	reviews.value = reviews.value.filter((t) => t !== review)
}

      const current = new Date();
      const date = `${current.getDate()}/${current.getMonth()+1}/${current.getFullYear()}`;
  



onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	reviews.value = JSON.parse(localStorage.getItem('reviews')) || []
})


</script>

<template>
	<main class="app">
		<section>
      <article class="greeting" id="0">
        <nav class="navbar navbar-expand-sm navbar-dark bg-dark mb-4">
    <div class="container">
      <a class="navbar-brand" href="#"> <img src="src/assets/img/inline.png" style="width: 50px;" alt=""> Pizza Hut</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto">
         <li class="nav-item">
              <a class="nav-link" href="#1">Create-Review</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#2"
                >All-Reviews</a
              >
            </li>
           
        </ul>
      </div>
  </div>
</nav>

		</article>

		<article class="create-review" id="1">
      <img src="src/assets/img/logo.png" alt="">
      <br>
			<p>Leave Your-Last-Visit Review for Pizza Hut!</p>
<br>
			<form id="new-review-form" @submit.prevent="addreview" class="p-5 border border-2">
        <p class="txt">Your Review</p>
				<input 
					type="text" 
					name="content" 
					id="content" 
          
					placeholder="Enter Review Here"
					v-model="input_content" />
				
				<p class="txt">Review Topic</p>
				<div class="options">

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category1" 
							value="food"
							v-model="input_category" />
						<span class="bubble food"></span>
						<div>Food</div>
					</label>

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category2" 
							value="atmosphere"
							v-model="input_category" />
						<span class="bubble atmosphere"></span>
						<div>Atmosphere</div>
					</label>

				</div>

				<input type="submit" value="Add review" />
			</form>
		</article>

		<article class="review-list" id="2">
      <div class="all-reviews"> 
        <img src="src/assets/img/inline.png" style="width: 50px;" alt=""> 
        <h4>Here's your past reviews!</h4>
      </div>

			<div class="list" id="review-list">

        <div v-if="reviews.length == 0">
          Oops! No Review Yet :( <br>
          Add one <a href="#0">here</a>!
        </div>
				<div v-for="review in reviews_asc" :class="`review-item ${review.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="review.done" />
						<span :class="`bubble ${
							review.category == 'food' 
								? 'food' 
								: 'atmosphere'
						}`"></span>
					</label>

					<div class="review-content">
						<input type="text" v-model="review.content" />
            <p>{{date}}</p>
					</div>

					<div class="actions">
						<button class="delete" @click="removereview(review)">Delete</button>
					</div>
				</div>

			</div>
		</article>
    </section>
		

	</main>
</template>


