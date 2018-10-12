<template>
    <div class="filter">
        <div class="filter-head">
            <div class="head-left">
                <span class="header-text">Find the right trails for you</span>
                <span class="header-action" @click="clearFilters">Clear filters</span>
            </div>
            <div class="head-right">
                <v-icon class="v-icon"
                        @click="dropList = !dropList"
                        color="#fff">{{dropList ? 'keyboard_arrow_up' : 'keyboard_arrow_down'}}</v-icon>
            </div>
        </div>
        <transition name="list-out" mode="out-in">
            <div class="filter-row" v-if="dropList">
                <div class="filter-column">
                    <div class="column-title"><span>Your role</span></div>
                    <div class="list">
                        <span v-for="(role, i) in rolesObj"
                              :class="'column-variant ' + (role.selected?'selected':'')"
                              :key="i"
                              @click="changeItemStatus(rolesObj, i)">
                            <span>{{role.label}}</span>
                        </span>
                    </div>
                </div>
                <div class="filter-column">
                    <div class="column-title"><span>Your level</span></div>
                    <div class="list">
                        <span v-for="(level, i) in levelsObj"
                              :class="'column-variant ' + (level.selected?'selected':'')"
                              :key="i"
                              @click="changeItemStatus(levelsObj, i)">
                            <span>{{level.label}}</span>
                        </span>
                    </div>
                </div>
                <div class="filter-column products-column fat-column">
                    <div class="column-title"><span>Products</span></div>
                    <div class="list">
                        <span v-for="(product, i) in productsObj"
                              :class="'column-variant ' + (product.selected?'selected':'')"
                              :key="i"
                              @click="changeItemStatus(productsObj, i)">
                            <span>{{product.label}}</span>
                        </span>
                    </div>
                </div>
                <div class="filter-column tags-column fat-column">
                    <div class="column-title"><span>Tags ({{tagsObj.length}})</span></div>
                    <div class="list">
                        <span v-for="(tag, i) in tagsObj"
                              :class="'column-variant ' + (tag.selected?'selected':'')"
                              :key="i"
                              @click="changeItemStatus(tagsObj, i)">
                            <span>{{tag.label}}</span>
                        </span>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
    export default {
        name: "AppFilter",
        props: {
            roles: {
                type: Array,
                required: true
            },
            levels: {
                type: Array,
                required: true
            },
            products: {
                type: Array,
                required: true
            },
            tags: {
                type: Array,
                required: true
            }
        },
        data() {
            return {
                dropList: false,
                rolesObj: this.roles.map(role => {return {label: role, selected: false}}),
                levelsObj: this.levels.map(level => {return {label: level, selected: false}}),
                productsObj: this.products.map(product => {return {label: product, selected: false}}),
                tagsObj: this.tags.map(tag => {return {label: tag, selected: false}}),
            }
        },
        methods: {
            clearFilters() {
                [this.rolesObj, this.levelsObj, this.productsObj, this.tagsObj] = [this.rolesObj, this.levelsObj, this.productsObj, this.tagsObj].map(inpArr => inpArr.map(el => {return {...el, selected: false};}));
                this.emitOnFilterChange();
            },
            changeItemStatus(arr, i) {
                let newValue = arr[i];
                newValue.selected = !newValue.selected;
                this.$set(arr, i, newValue);
                this.emitOnFilterChange();
            },
            emitOnFilterChange() {
                this.$emit('onFilterChange', [this.rolesObj, this.levelsObj, this.productsObj, this.tagsObj].map(arr => arr.filter(a => a.selected).map(a => a.label)));
            }
        }
    }
</script>

<style scoped>
    span, .v-icon {
        user-select: none;
        margin: 4px;
    }

    .column-title, .header-text {
        font-weight: bold;
    }

    .filter {
        /*padding: 0.5vw 1vw;*/
        color: #fff;
        /*background-color: #3235ff;*/
    }

    .filter-head {
        display: flex;
        justify-content: space-between;

        background-color: #3235ff;
        padding: 0.5vw;
    }

    .filter-head>div {
        align-items: center;
        display: flex;
    }

    .column-title {
        text-align: left;
    }

    .filter-row {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;

        background-color: #3235ff;
        padding: 0 0.5vw 0.5vw 0.5vw;
    }

    .filter-column>.list {
        display: flex;
        flex-direction: column;
        text-align: left;
        width: 13.8vw;
    }

    .filter-column.fat-column>.list {
        width: 33vw;
        flex-direction: row;
        flex-wrap: wrap;
    }

    .filter-column.fat-column>.list>span {
        display: inline-block;
        width: calc(16vw - 8px);
    }

    .header-action, .list>span {
        cursor: pointer;
    }

    .header-action, .list>span>span {
        padding: 0.1vw 0.5vw;
    }

    .header-action {
        text-decoration: underline;
    }

    .list>span:hover {
        text-decoration: underline;
    }

    .list>span.selected>span {
        background-color: rgba(255, 255, 255, 0.2);
        border-radius: 0.1vw;
    }

    .list-out-enter-active {
        animation: flipInX 0.3s linear;
    }
    .list-out-leave-active {
        animation: flipOutX 0.3s linear;
    }

    @keyframes flipInX {
        from {
            transform: translateY(-100%);
            /*margin-bottom: 0;*/
            opacity: 0;
        }
        to {
            transform: translateY(0);
            /*margin-bottom: 100%;*/
            opacity: 1;
        }
    }

    @keyframes flipOutX {
        from {
            transform: translateY(0);
            /*margin-bottom: 100%;*/
            opacity: 1;
        }
        to {
            transform: translateY(-100%);
            /*margin-bottom: 0;*/
            opacity: 0;
        }
    }
</style>