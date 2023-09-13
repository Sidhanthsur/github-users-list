<script setup>
import { ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import userResults from './mock/userResult.json'
import UserCard from './components/UserCard.vue';
import userDetails from './mock/userDetails.json'

const skills = ref()
const location = ref()
const fetchedUsers = ref([])
const selectedUser = ref(null)
const isLoading = ref(false)

// props , parsingFun
// endpoint

const onFindClicked = async () => {

  if(!(skills?.value && location?.value)) {
    // alert the user
    return
  } 
  try {
    isLoading.value = true
    const response = await fetch(`https://api.github.com/search/users?q=type:user+language:${skills.value}+location:${location.value}&page=1`)
    const data = await response.json()
    fetchedUsers.value = data.items
  } catch (e) {
    console.log(e)
  } finally {
    isLoading.value = false
  }


}

const onUserSelected = (id) => {
  console.log(id)
  // mocked for now swap with real api
  selectedUser.value = userDetails
}
</script>

<template>
<div class="app__container">
  <!--Left side-->
  <div>
    <input
      placeholder="skills"
      v-model="skills" />
      <input
      placeholder="location"
      v-model="location" />

      <button @click="onFindClicked">
        Find Candidates
      </button>

      <!--List-->
      <UserCard
        @select-user="onUserSelected"
        v-for="(user, index) in fetchedUsers" :key="index" :user="user" />
  </div>

  <!-- Right side -->
<div v-if="selectedUser" class="app__right-container">
  <img class="app__user-image"  :src="selectedUser.avatar_url" />
  <a :href="selectedUser.html_url" target="_blank">{{  selectedUser.name }}</a>

  <div>
    <span v-if="selectedUser.company">Company: {{ selectedUser.company }}</span>
    <span v-if="selectedUser.email">Email: {{ selectedUser.email }}</span>
    Bio: {{ selectedUser.bio }}
    Location: {{  selectedUser.location }}

  </div>
</div>
</div>


</template>

<style scoped>
.app__container {
  display: flex;
  align-content: center;
  justify-content: center;
  flex-direction: row;
}

.app__right-container {
  width: 300px;
  border: 1px solid grey;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 400px;
}
.app__user-image {
  height: 32px;
  width: 32px;
  border-radius: 50%;
}
</style>
