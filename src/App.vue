<template>
    <div class="container">
    <!-- @ = v-on 
         : = v-bind. Passing props down to child component   
    -->
        <SearchBar @termChange="onTermChange"/>
        <VideoDetail :video="selectedVideo" />
        <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>

    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';
const API_KEY = 'AIzaSyDwarhGVRxeByF9PsXKrHs8L3XO_VhN1HI';

export default {
    name: 'App',
    components: {
        // Tell component it has access to child component
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
        // Vue component - data object must be function that returns an object.
        return { 
            videos: [],
            selectedVideo: null
        };
    },
    methods: {
        onTermChange: function(searchTerm) {
            axios.get('https://www.googleapis.com/youtube/v3/search', {
                params: {
                    key: API_KEY,
                    type: 'video',
                    part: 'snippet',
                    q: searchTerm
                }
            }).then(response => {
                // No need to go data.videos. Directly access properties within data object
                this.videos = response.data.items
            });
        },
        onVideoSelect(video) {
            this.selectedVideo = video;
        }
    }
};
</script>