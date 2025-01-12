<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const schoolList = ref([]);
const schools = ref([]);
const isLoading = ref(false);
const search = ref(null);

const getSchoolList = async () => {
    try {
        isLoading.value = true;
        const response = await axios.get('https://api.devharlemwizardsinabox.com/campaign/campaign_school_list/?search=');
        schoolList.value = response.data.school_list;
        schools.value = response.data.school_list;
        isLoading.value = false;
    } catch (error) {
        console.error('Error fetching school list:', error);
    } finally {
        isLoading.value = false;
    }
};

const handleImageError = (id) => {
    schoolList.value.map((school) => {
        if (school.id === id) {
            school.logo = null;
        }
    })
};

const handleSearch = () => {
    if (search.value !== '') {
        schoolList.value = schools.value.filter((school) =>
            school.school_name.toLowerCase().includes(search.value.toLowerCase())
        );
    } else {
        schoolList.value = schools.value
    }
};

onMounted(async () => {
    await getSchoolList();
});

</script>

<template>
    <div>
        <img src="../assets/images/banner.png" alt="Banner" loading="lazy">
    </div>
    <div class="container">
        <div class="text-center mt-4">
            <div class="d-flex justify-content-center align-items-center gap-3">
                <p class="custom-heading mb-0">What is WizFit Challenge?</p>
                <button class="btn btn-primary" aria-label="Watch Video">
                    <i class="ri-triangle-fill"></i>
                    Watch Video
                </button>
            </div>

            <div class="player mt-4">
                <img src="../assets/images/player.png" alt="Player" loading="lazy">
            </div>

            <div class="challenge card">
                <p class="challenge-text">Are you ready to take the challenge?</p>
                <p class="challenge-download">Download Harlem Wizards App</p>
                <div class="store-buttons d-flex justify-content-center gap-3">
                    <img src="../assets/images/google-store.png" alt="Google Play Store" class="store-icon" loading="lazy">
                    <img src="../assets/images/apple-store.png" alt="Apple App Store" class="store-icon" loading="lazy">
                </div>
                <div class="store-text d-flex">
                    <hr>
                    <p class="signup-text col-3">Or you can sign up right now</p>
                    <hr>
                </div>

                <div class="signup-form mt-2">
                    <div class="search mb-2">
                        <input type="text" placeholder="Search campaign here" class="form-control" v-model="search" @input="handleSearch">
                    </div>
                    <div v-if="isLoading" class="loader-container">
                        <div class="loader"></div>
                    </div>
                    <div v-else class="main-school-list">
                        <template v-if="schoolList.length">
                            <div v-for="(school, index) in schoolList" :key="school.id" class="join-campaign mt-2">
                                <div class="school-campaign">
                                    <div class="d-flex align-items-center">
                                        <template v-if="school.logo">
                                            <img :src="school.logo" alt="School Logo" class="join-icon" loading="lazy" @error="handleImageError(school.id)">
                                        </template>
                                        <template v-else>
                                            <div class="join-icon-placeholder">
                                                {{ school.school_name.charAt(0) }}
                                            </div>
                                        </template>
                                        <p>{{ school.school_name }}</p>
                                    </div>
                                    <button class="btn btn-success" aria-label="Join Campaign">Join Campaign</button>
                                </div>
                            </div>
                        </template>
                        <template v-else>
                            <p class="no-campaign-found">No campaigns found</p>
                        </template>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<style scoped>
.section-loader {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(255, 255, 255, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 100;
    width: 100%;
    height: 100%;
}

.loader {
    border: 5px solid #f3f3f3;
    border-top: 5px solid #3498db;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 1s linear infinite;
}

.loader-container{
    display: flex;
    justify-content: center;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

img {
    width: 100%;
}

.custom-heading {
    font-family: 'Egyptian';
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--text-color);
}

.container {
    padding: 1rem;
}

button {
    padding: 0.5rem 1rem;
    background-color: var(--text-color);
    border: none;
    font-family: 'Egyptian';
    color: #fff;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
}

button i {
    transform: rotate(90deg);
    transition: transform 0.3s ease;
}

button:hover {
    background-color: var(--text-color);
    /* color: #fff; */
}

.player {
    max-width: 280px;
    width: 100%;
    margin: 0 auto;
    text-align: center;
    margin-top: 30px;
}

.challenge {
    margin: -1px auto;
    text-align: center;
    width: 600px;
    box-shadow: 2px 2px 2px;
    color: #d7d7d7;
}

.challenge-download {
    color: black;
    font-size: 0.6rem;
    margin: 0;
    font-weight: 600;
}

.card {
    text-align: center;
}

.card hr {
    color: black;
    width: 12%;
}

.signup-text {
    color: black;
    font-size: 0.5rem;
    margin: 6px 0px;
    font-weight: 600;
}

.challenge-text {
    font-size: 2rem;
    font-weight: 500;
    color: var(--text-color);
    margin: 0.5rem 0;
    text-align: center;
    font-family: 'RoadRange';
}

.store-buttons img {
    max-width: 90px;
    width: 100%;
    transition: transform 0.3s ease;
}

.store-buttons img:hover {
    transform: scale(1.05);
}

.store-text {
    align-items: center;
    justify-content: center;
    margin-top: 10px;
}

.search {
    display: flex;
    justify-content: center;
}


.signup-form {
    text-align: center;

}

.form-control {
    border-radius: 10px;
    font-size: 10px;
    padding-left: 30px;
    height: 30px;
    width: 50%;
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.form-control:focus {
    border-color: #d6d6d6;
    outline: none;
    box-shadow: 0 0 5px rgba(204, 204, 204, 0.4);
}

.school-campaign {
    width: 50%;
    background: #ececec;
    display: flex;
    border-radius: 5px;
    padding: 10px;
    justify-content: space-between;
}

.school-campaign img {
    border-radius: 50%;
    width: 26px;
    height: 26px;
}

.school-campaign p {
    margin: 0;
    padding: 0;
    color: black;
    font-size: 10px;
    text-transform: capitalize;
    font-weight: 600;
    padding: 0 10px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.join-campaign {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    margin-left: 10px;
}

.join-icon {
    max-width: 40px;
    width: 100%;
}

.join-campaign button {
    padding: 0 4px;
    background-color: #fff;
    color: var(--text-color);
    border: none;
    border-radius: 5px;
    font-size: 1rem;
    transition: background-color 0.3s ease;
    justify-content: space-between;
    font-size: 9px;
    border: 1px solid;
}

.main-school-list {
    width: 100%;
    overflow-y: auto;
    max-height: 220px;

    scrollbar-width: thin;
    scrollbar-color: #c0c0c0 #f0f0f0;
    margin-bottom: 40px;
}

.main-school-list::-webkit-scrollbar {
    width: 8px;
}

.main-school-list::-webkit-scrollbar-track {
    background: #f0f0f0;
    border-radius: 10px;
}

.main-school-list::-webkit-scrollbar-thumb {
    background-color: #c0c0c0;
    border-radius: 10px;
    border: 2px solid #f0f0f0;
}

.main-school-list::-webkit-scrollbar-thumb:hover {
    background-color: #a0a0a0;
}

.join-icon-placeholder {
    width: 26px;
    height: 26px;
    border-radius: 50%;
    background-color: #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 10px;
    font-weight: bold;
    color: white;
    font-size: 18px;
}

.no-campaign-found {
    text-align: center;
    color: #888;
    font-size: 1.2rem;
    font-weight: 500;
    margin-top: 20px;
}
</style>
