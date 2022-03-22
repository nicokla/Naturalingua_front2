<script setup>
import { ref, computed, unref, watch } from 'vue';
import { useRouter } from 'vue-router'
const router = useRouter()
import listOfFiles from '../listOfFiles.json'

let languages = ['Arabic', 'Chinese', 'English', 'French','Greek','Hebrew','Hindi','Italian','Japanese','Korean','Persian','Portuguese','Russian','Spanish','Thai','Turkish','Vietnamese']
let language=ref(languages[0])
let languageKnown=ref(languages[0])

let email=ref('')
let url=ref('')
let videoIds=ref([])

let isDisabled=computed(()=>
	!(email.value!='' && unref(videoIds).length != 0)
)

let buttonClass=computed(()=>{
	if(!isDisabled.value)
		return 'my-button'
	else
		return 'my-button-disabled'
})

function deleteId(videoId){
	const index = videoIds.value.indexOf(videoId);
	if (index > -1) {
		videoIds.value.splice(index, 1); // 2nd parameter means remove one item only
	}
}

const move = () => {
  if (!isDisabled.value)
    router.push({ 
			name:'Suite',
      params: {
				videoIds: unref(videoIds),
				email: unref(email),
				language: unref(language),
				languageKnown: unref(languageKnown),
      }
    })
	else{
		window.alert('Please write your email adress so that we can send you the document, and add at least one youtube video.')
	}
}

function youtube_parser(url){
	var regExp = /^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#&?]*).*/;
	var match = url.match(regExp);
	return (match&&match[7].length==11)? match[7] : false;
}

function addVideo() {
	let myId = youtube_parser(url.value)
	if(myId){
    url.value = ""
		videoIds.value.push(myId)
	}
  else{
		window.alert('Please write a valid youtube url before adding it to the list. To copy a youtube url from the Youtube app, click on the "Share" button next to the like and dislike buttons, and then click on "Copy link". You can then paste the video url on our website and add the video to the list with the "Add video" button.')
    url.value = ""
	}
}
</script>


<template>

  <div class="liste">
		<img src="../assets/languages2.jpeg" alt="" width="120" height="120">
		<h1 class="text-3xl mb-3 mt-2">
			NaturaLingua
		</h1>
		<div class="text-base">
			Using our website, you can download youtube videos subtitles as bilingual pdf documents, to learn new languages easily. Our documents use the latin alphabet to make reading new languages easy and natural.<br> <p class="important">Before choosing a youtube video to transform into a bilingual pdf, please make sure that this youtube video is subtitled both in english and in your target language (else we will not be able to generate the pdf).</p>Please pick the language you are studying below, and add the youtube video urls whose subtitles you want to download. <br>The price of a pdf document is 1.20 euro.
		</div> 
		
		<div>
			Language you already know : 
			<select v-model="languageKnown" class="m-2 rounded border-1 border-black">
				<option v-for="machin in languages" :id="machin">{{machin}}</option>
			</select>
		</div>

		<div>
			Language you learn : 
			<select v-model="language" class="m-2 rounded border-1 border-black">
				<option v-for="bidule in languages" :id="bidule">{{bidule}}</option>
			</select>
		</div>

		<div>
			Youtube videos:

			<input type="text" class="m-2 rounded border-1 border-black" placeholder="copy paste youtube video url here" v-model="url">
			<button class="bg-blue-400 m-2 p-2 rounded border-1 border-black" @click="addVideo()">
				<!-- <i class="fas fa-plus-square"></i> -->
				Add video
			</button>

			<div class="todo-container">
				<ul class="todo-list">
					<div class="todo" v-for="videoId in videoIds" :id="videoId">
						<li class="todo-item">{{videoId}}</li>
						<button class="trash-btn" @click="deleteId(videoId)" > 
							<i class="fas fa-trash"></i>
						</button>
					</div>
				</ul>
			</div>
		</div>

		<div>
			Email :
			<input autocomplete="email" type="text" class="m-2 rounded border-1 border-black m-2" v-model="email" placeholder="type your email here"/>
		</div>

    <button :class="buttonClass" @click="move"> 
      Go to purchase summary
    </button>
    <br><br><br>
    <p>Other websites by NaturaLingua:
      <ul>
        <li>
          <a href="https://naturalingua.netlify.app">https://naturalingua.netlify.app</a>
        </li>
        <li>
          <a href="https://getmoviessubtitles.netlify.app">https://getmoviessubtitles.netlify.app</a>
        </li>
      </ul>
    </p>
		<br><br><br>
		<a href="https://www.flaticon.com/free-icons/languages" title="languages icons">Languages icons created by Freepik - Flaticon</a>
  </div>

</template>

<style scoped>

.important {
  color: black;
  text-decoration: underline black;
  /* font-weight: bold; */
}

a {
  color: blue;
  text-decoration: underline blue;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.liste{
	padding: 0.8em;
	display: flex;
	flex-direction: column;
	flex-wrap: nowrap;
	justify-content: flex-start;
	align-items: center;
	align-content: space-between;
}
.my-button{
  @apply bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-4
}
.my-button-disabled{
	/* pointer-events:none; */
	cursor : default !important;
  @apply bg-gray-500 text-white font-bold py-2 px-4 rounded mt-4
}


/* ------------------- */


header {
  font-size: 2rem;
}

header,
form {
  min-height: 20vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
form input,
form button {
  padding: 0.5rem;
  font-size: 2rem;
  /* border: none; */
  /* background: white; */
}
form button {
  color: #ff6f47;
  background: #f7fffe;
  cursor: pointer;
  transition: all 0.3s ease;
}
form button:hover {
  background: #ff6f47;
  color: white;
}
.todo-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.todo-list {
  min-width: 30%;
  list-style: none;
}

.todo {
  margin: 0.5rem;
  background: orange;
  /* font-size: 1.5rem; */
  color: black;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 1s ease;
}
.filter-todo {
  padding: 1rem;
}
.todo li {
  flex: 1;
}

.trash-btn,
.complete-btn {
  background: #ff6f47;
  color: white;
  /* border: none; */
  padding: 0.5rem;
  cursor: pointer;
  /* font-size: 1rem; */
}
.complete-btn {
  background: rgb(11, 212, 162);
}
.todo-item {
  padding: 0rem 0.5rem;
}

.fa-trash,
.fa-check {
  pointer-events: none;
}

.fall {
  transform: translateY(10rem) rotateZ(20deg);
  opacity: 0;
}

.completed {
  text-decoration: line-through;
  opacity: 0.5;
}

/* Custom Select */
/* .select {
  margin: 1rem;
  position: relative;
  overflow: hidden;
}
select {
  color: #ff6f47;
  font-family: "Poppins", sans-serif;
  cursor: pointer;
  width: 12rem;
} */

/* Arrow */
.select::after {
  content: "\25BC";
  position: absolute;
  top: 0;
  right: 0;
  padding: 1rem;
  background: #ff6f47;
  cursor: pointer;
  pointer-events: none;
} 

/* Transition */

.select:hover::after {
  transform: scale(1.5);
}

</style>
