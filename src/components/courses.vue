<template>
    <div class="cards-wrapper">
        <template v-if="coursesArr.length>0">
            <v-card v-for="(course, i) of coursesArr" :key="i">
                <div class="custom-header" :style="{backgroundColor: '#'+colours[i]}">
                    <div class="left">
                        <v-icon large>
                            {{icons[i]}}
                        </v-icon>
                    </div>
                    <div class="right">
                        <span>{{course.time}}</span>
                        <span>{{course.modulesCounts}} Modules</span>
                    </div>
                </div>

                <v-card-title primary-title>
                    <div>
                        {{course.type}}
                        <p class="headline">{{course.name}}</p>
                        <div>{{course.parameters}}</div>
                    </div>
                </v-card-title>

                <v-card-actions>
                    <div>
                        <v-btn color="white" @click="changeCourceStatus(i)">
                            <v-icon>{{course.liked ? 'favorite' : 'favorite_border'}}</v-icon>
                        </v-btn>
                        <v-btn color="white">
                            <v-icon>add</v-icon>
                        </v-btn>
                    </div>
                    <div>
                        <v-btn color="info">{{course.role}}</v-btn>
                        <v-btn color="light-grey">{{course.level}}</v-btn>
                    </div>
                </v-card-actions>
            </v-card>
        </template>
        <div class="empty-courses" v-else><span>Nothing to show...</span></div>
    </div>
</template>

<script>
    export default {
        name: "AppCourses",
        props: {
            courses: {
                type: Array,
                required: true
            }
        },
        data() {
            return {
                colours: ['7FFFD4', '78DBE2', 'F19CBB', 'E52B50', '44944A', 'A8E4A0'].sort(() => Math.random() > 0.5),
                icons: ['account_balance', 'all_inbox', 'announcement', 'assessment', 'extension', 'language'].sort(() => Math.random() > 0.5),
                // coursesArr: this.courses.map(course => {return {...course, liked: false}})
            }
        },
        computed: {
            coursesArr() {return this.courses.map(course => {return {...course, liked: false}})}
        },
        methods: {
            changeCourceStatus(i) {
                let newValue = this.coursesArr[i];
                newValue.liked = !newValue.liked;
                this.$set(this.coursesArr, i, newValue);
            }
        }
    }
</script>

<style scoped>
    .empty-courses {
        height: 10vw;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .empty-courses>span {
        color: #41a3f1;
        font-size: 2vw;
    }

    .cards-wrapper {
        display: flex;
        flex-wrap: wrap;
    }

    .custom-header {
        height: 30%;

        display: flex;
        justify-content: space-between;
    }

    .custom-header>.left {
        width: 20%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .custom-header>.right {
        margin: 4%;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        font-weight: bold;
        color: white;
    }

    .custom-header>.left>.v-icon {
        color: white;
    }

    .headline {
        color: #41a3f1;
    }

    .v-card {
        width: 500px;
        min-height: 350px;
        margin: 1vw;

        display: flex;
        flex-direction: column;
    }

    .v-card__actions {
        display: flex;
        justify-content: space-between;
        padding: 16px;
    }

    .v-card__actions>div {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        justify-content: center;
    }

    .v-btn {
        min-width: auto;
    }
</style>