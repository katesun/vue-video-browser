<template>
    <div class="container">
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
const API_KEY = 'AIzaSyDWFgIS4_rZ6k4878WXAvoOlS8AQMLZZ24';

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
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
                this.videos = response.data.items
            });
        },
        onVideoSelect(video) {
            this.selectedVideo = video;
        }
    }
};
</script>