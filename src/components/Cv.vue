<template>
    <v-container>

        <v-row align="stretch">
            <v-col align="stretch">
                <h2>{{cv.name}}</h2>
                <p>{{cv.bio}}</p>
                <router-link to="/resume"><v-btn color="green darken-4" >Download Resume</v-btn></router-link>
            </v-col>
        </v-row>

        <Experience :data="cv.education" title="Education"></Experience>
        <Experience :data="cv.experience" title="Experience"></Experience>
        <Experience :data="cv.volunteer" title="Volunteer Experience"></Experience>

    </v-container>
</template>

<script>
import Axios from "axios";
import Experience from "./Experience";

export default {
    name: "cv",
    components: {
        Experience
    },
    data: function() {
        return {
            cv: {
                name: "",
                bio: "",
                experience: [],
                education: []
            }
        };
    },
    mounted: function() {
        this.getCv();
    },
    methods: {
        getCv: function() {
            var _this = this;
            Axios.get("/data/cv.json").then(function(response) {
                _this.cv = response.data;
            });
        }
    }
};
</script>
