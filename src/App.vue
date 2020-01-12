<template>
    <div class="container">
        <SearchBar @termChange="onTermChange"></SearchBar>
        <div class="row">
            <VideoDetail :video="selectedVideo"></VideoDetail>
            <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';
const API_KEY = 'AIzaSyCW8_mcuCQ6akWp5cI1uF8w9TB7LfDlLxU';

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
        return { videos: [], selectedVideo: null };
    },
    methods: {
        onVideoSelect(video) {
            console.log(video);
            this.selectedVideo = video;
        },
        onTermChange(searchTerm) {
            //console.log(searchTerm);
            axios.get('https://www.googleapis.com/youtube/v3/search', {
                params: {
                    key: API_KEY,
                    type: 'video',
                    part: 'snippet',
                    q: searchTerm
                }
            }).then(response => {
                console.log(response.data.items)
                this.videos = response.data.items
            });
        }
    }
};
</script>