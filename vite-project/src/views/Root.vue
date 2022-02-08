<script setup>
import { ref, computed, unref, watch } from 'vue';
import { useRouter } from 'vue-router'
const router = useRouter()
import listOfFiles from '../listOfFiles.json'

let languages = ['French','Spanish','Portuguese','Italian','Vietnamese','Turkish','Japanese','Chinese','Korean','Thai','Russian','Greek','Hebrew','Arabic','Persian','Hindi']
let language=ref(languages[0])

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

const addVideo = (url) => {
	let myId = youtube_parser(url)
	if(myId){
		videoIds.value.push(myId)
	}
  else{
		window.alert('Please write a valid youtube url before adding it to the list')
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
			Using our website, you can download youtube videos subtitles as pdf documents, to learn new languages easily.<br>Please pick the language you are studying, and add the video urls whose subtitles you want to download (they need to have manual subtitles both in the language you study and in english). The price is 1.5 euro per pdf.
		</div> 
		
		<div>
			language: 
			<select v-model="language" class="m-2 rounded border-1 border-black">
				<option v-for="bidule in languages" :id="bidule">{{bidule}}</option>
			</select>
		</div>

		<div>
			youtube videos:

			<input type="text" class="m-2 rounded border-1 border-black" placeholder="copy paste youtube video url here" v-model="url">
			<button class="bg-blue-400 m-2 p-2 rounded border-1 border-black" @click="addVideo(url)">
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
			email :
			<input autocomplete="email" type="text" class="m-2 rounded border-1 border-black m-2" v-model="email" placeholder="type your email here"/>
		</div>

    <button :class="buttonClass" @click="move"> 
      Go to purchase summary
    </button>
		<br><br><br>
		<a href="https://www.flaticon.com/free-icons/languages" title="languages icons">Languages icons created by Freepik - Flaticon</a>
  </div>

</template>

<style scoped>
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

</style>
