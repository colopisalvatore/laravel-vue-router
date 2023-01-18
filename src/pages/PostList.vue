<template>
    <section>
        <h1>Lista dei Post</h1>
        <div class="row">
            <div class="col-12 col-md-4" v-for="(post, index) in posts" :key="index">
                <div class="card" style="width: 18rem;">
                    <img :src="`${store.imageBasePath}${post.cover_image}`" class="card-img-top" :alt="post.title">
                    <div class="card-body">
                        <h5 class="card-title">{{ post.title }}</h5>
                        <p class="card-text">{{ truncateContent(post.content) }}</p>
                        <router-link class="btn btn-primary" :to="{name: 'single-post', params:{slug: post.slug}}">Vai al Post</router-link>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
    import axios from 'axios';
    import { store } from '../store';
    export default {
        name: 'PostList',
        Data(){
            return {
                store,
                posts: [],
                contentMaxLen: 100
            }
        },
        methods:{
            getPosts(){
                axios.get(`${this.store.apiBaseUrl}/posts`).then((response)=>{
                    // console.log(response.data);
                    this.posts = response.data.results.data;
                })
            },
            truncateContent(text){
                if(text.length > this.contentMaxLen){
                    return text.substr(0,this.contentMaxLen) + '...'
                }
                return text;
            }
        },
        mounted(){
            this.getPosts();
        }
    }
</script>

<style lang="scss" scoped>

</style>