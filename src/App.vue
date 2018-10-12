<template>
    <v-app>
        <v-content>
            <AppFilter :roles="roles"
                       :levels="levels"
                       :products="products"
                       :tags="tags"
                       @onFilterChange="onFilterChange"
            />
            <AppCourses :courses="filteredCourses"/>
        </v-content>
    </v-app>
</template>

<script>
    import AppFilter from './components/filter';
    import AppCourses from './components/courses'

    import json from '../courses';

    export default {
        name: 'App',
        components: {
            AppFilter,
            AppCourses
        },
        data () {
            return {
                roles: ['Admin', 'Business User', 'Developer'],
                levels: ['Beginner', 'Intermediate', 'Advanced'],
                products: ['Commerce Cloud', 'Community Cloud', 'Marketing Cloud', 'Quip', 'Einstein', 'Sales Cloud'],
                tags: ['tag1', 'tag2', 'tag3', 'tag4', 'tag5', 'tag6'],
                filterArs: [[],[],[],[]]
            }
        },
        computed: {
            filteredCourses () {
                let courses = json.courses;
                if(this.filterArs.some(arr => arr.length>0)) {
                    courses = courses.filter(course => {
                        return (
                            (this.filterArs[0].length===0 || this.filterArs[0].indexOf(course.role)!==-1) &&
                            (this.filterArs[1].length===0 || this.filterArs[1].indexOf(course.level)!==-1) &&
                            (this.filterArs[2].length===0 || course.products.some(p => this.filterArs[2].indexOf(p)!==-1)) &&
                            (this.filterArs[3].length===0 || course.tags.some(t => this.filterArs[3].indexOf(t)!==-1))
                        );
                    });
                }
                return courses;
            }
        },
        methods: {
            onFilterChange(newArrs) {
                this.filterArs = newArrs;
            }
        }
    }
</script>
