<script setup>

    // import SideBar from './components/SideBar.vue'
    import axios from 'axios'
    import {ref} from 'vue'

    const videos = ref([])
    const query = ref('batman')
    const currentVideo = ref(null)

    async function getVideos() {
        await axios.get(`https://youtube.googleapis.com/youtube/v3/search?part=snippet&maxResults=5&q=${query.value}&key=AIzaSyC9dPlfG7B7lZs59bTsGJq_eEUm-jcNERc`)
        .then(response => {
            console.log(response.data.items)
            videos.value = response.data.items
        })
        .catch(error => {
            console.log(error)
        })
    }

    window.onload = async () => {
        await getVideos()
        currentVideo.value = videos.value[0] // HOW DO I SET THE CURRENTVIDEO TO THE FIRST ELEMENT OF VIDEOS
        query.value = ''
        
    } 
    

</script>

<template>
    <v-app>
        <v-main>
            <v-container>
                <v-text-field label="Search" @keyup.enter="getVideos" v-model="query">

                </v-text-field>
                <v-row>
                    <v-col cols="8" v-if="currentVideo">
                        <iframe width="100%" height="450" :src="'https://www.youtube.com/embed/'+currentVideo.id.videoId">
                        
                            <template v-slot:placeholder>
                                <v-row
                                    class="fill-height ma-0"
                                    align="center"
                                    justify="center"
                                    >
                                    <v-progress-circular
                                    indeterminate
                                    color="grey-lighten-5"
                                    ></v-progress-circular>
                                </v-row>
                            </template>

                        </iframe>
                        <v-row>
                            <v-col cols="12" class="mt-2">
                                <strong>{{ currentVideo.snippet.title }}</strong>
                            </v-col>
                            <v-col cols="12">
                                {{ currentVideo.snippet.description }}
                            </v-col>
                        </v-row>
                    </v-col>
                    <v-col cols="4" class="mt-4" >
                        <v-row v-for="video in videos" :key="video" @click="currentVideo = video">
                            <v-col cols="6">
                                <v-img :src="video.snippet.thumbnails.medium.url">
                                
                                    <template v-slot:placeholder>
                                        <v-row
                                            class="fill-height ma-0"
                                            align="center"
                                            justify="center"
                                        >
                                            <v-progress-circular
                                                indeterminate
                                                color="grey-lighten-5"
                                            ></v-progress-circular>
                                        </v-row>
                                    </template>
                                
                                </v-img>
                            </v-col>
                            <v-col cols="6">
                                <strong>{{video.snippet.title}}</strong>
                                <br>
                                {{video.snippet.channelTitle}}
                            </v-col>
                            <v-divider/>
                        </v-row>
                        <v-divider thickness="4" v-show="currentVideo" />
                    </v-col>
                </v-row>
            </v-container>
        </v-main>
    </v-app>
</template>

