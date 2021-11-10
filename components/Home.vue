<template>
  <div class="container mx-auto px-4 text-center">
        <div class="flex items-center justify-center mx-auto">
            <h1 class="text-3xl">
                Search by serial number to hear the surah by verse
            </h1>
        </div>
        <input class="border p-3 rounded-md my-4 text-md" placeholder="Search ==>" type="number" v-model="query" @keypress.enter="search">
        <div class="grid grid-cols-2 gap-3 mb-3" v-show="open">
            <div class="card bg-white p-3 rounded-lg cursor-pointer">
                <div class="flex items-center justify-end">
                    <h3 class="card__title text-lg text-right">({{searchResults.data.name.transliteration.id}})   {{searchResults.data.name.long}}</h3>
                    <!-- <img class="ml-3 w-6 h-6" src="../assets/up-arrow.png" alt="arrow"> -->
                </div>
                <div class="flex items-center">
                    <audio :src="searchResults.data.preBismillah.audio.primary" controls class="w-1/2 mb-2"></audio>
                    <p class="w-1/4 text-sm">({{searchResults.data.preBismillah.text.transliteration.en}})   </p>
                    <h3 class="card__title text-sm text-sm w-1/4 text-right">{{searchResults.data.preBismillah.text.arab}}</h3>
                </div>
                <div class="flex items-center" v-for="(item, index) in searchResults.data.verses" :key="index">
                    <audio :src="item.audio.primary" controls class="w-1/2 mb-2"></audio>
                    <p class="w-1/4 text-sm">({{item.text.transliteration.en}})   </p>
                    <h3 class="card__title text-sm w-1/4 text-right">{{item.text.arab}}</h3>
                </div>
                <!-- <div class="mt-3" v-show="openCard">
                    <p>{{item.tafsir.id}}</p>
                </div> -->
            </div>
        </div>
        <div class="grid grid-cols-3 gap-3 text-left">
            <div class="card bg-white p-3 rounded-lg cursor-pointer" v-for="(item, index) in resultsArabic.data" :key="index">
                <div class="flex items-center justify-end">
                    <h3 class="card__title text-lg text-right">(№{{item.number}})({{item.name.transliteration.id}})   {{item.name.long}}</h3>
                    <!-- <img class="ml-3 w-6 h-6" src="../assets/up-arrow.png" alt="arrow"> -->
                </div>
                <!-- <div class="mt-3" v-show="openCard">
                    <p>{{item.tafsir.id}}</p>
                </div> -->
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return{
            openCard: false,
            query: '',
            url_base_arabic: 'https://api.quran.sutanlab.id/surah/',
            // url_base_uzbek: `https://quranenc.com/api/translation/sura/uzbek_mansour/${this.resultsArabic.element.number}`,
            resultsArabic: {
                data: []
            },
            resultsUzbek:{},
            searchResults: {
                data: {
                    name: {
                        long: '',
                        transliteration: {
                            id: "Surah's name"
                        }
                    },
                    preBismillah: {
                        audio: {},
                        text: {
                            transliteration: '',
                            arab: ''
                        }
                    },
                }
            },
            open: false
        };
    },
    created() {
        axios
        .get(`${this.url_base_arabic}`)
        .then((response) => {
        this.resultsArabic = response.data;
        console.log(this.resultsArabic);
        })
        .catch((e) => {
        console.log(e);
        });
    },
    methods: {
        search(){
            axios
            .get(`${this.url_base_arabic}${this.query}`)
            .then((response) => {
            this.searchResults = response.data;
            console.log(this.searchResults);
            })
            .catch((e) => {
            console.log(e);
            });
            this.open = true;
        }
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Dongle:wght@300;400;700&family=Lobster&family=Rubik:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
body{
    font-family: Rubik;
    background-color: #F4CA16;
}
</style>