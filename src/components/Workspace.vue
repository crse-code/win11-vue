<template>
    <div class="desktop-icons flex-grow p-1">
        <grid-layout
            :layout.sync="items"
            :col-num="cols"
            :max-rows="rows"
            :row-height="112"
            :is-draggable="true"
            :is-resizable="false"
            :is-mirrored="false"
            :vertical-compact="false"
            :margin="[2, 2]"
            :use-css-transforms="true"
            style="width: 100%; height: 100%;">

            <grid-item v-for="item in items" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :i="item.i" :key="item.i">
                <div class="app flex flex-col justify-center items-center w-24 h-28 hover:bg-white hover:bg-opacity-10 focus:bg-blue-500 focus:bg-opacity-10 border border-transparent rounded-sm" :class="{ 'bg-blue-500 hover:bg-blue-500 bg-opacity-10': activeItem === item.i }" @mouseup="activeItem = item.i">
                    <img :src="require(`@/assets/icons/${item.icon}.png`)" width="43" height="43">
                    <span class="text-white text-xs text-center app-name">{{ item.label }}</span>
                </div>
            </grid-item>

        </grid-layout>
    </div>
</template>

<script>
import VueGridLayout from 'vue-grid-layout'

export default {
    name: 'Workspace',
    components: {
        GridLayout: VueGridLayout.GridLayout,
        GridItem: VueGridLayout.GridItem
    },
    computed: {
        darkMode() {
            return this.$store.state.darkMode
        },
        iconTheme() {
            return this.darkMode ? 'light' : 'dark'
        }
    },
    data() {
        return {
            cols: 15,
            rows: 15,
            items: [],
            activeItem: '-1'
        }
    },
    methods: {
        loadItems() {
            this.items = [
                { label: 'This PC', icon: 'computer', x: 0, y: 0, w: 1, h: 1, i: '0' },
                { label: 'Recycle Bin', icon: 'trash-empty', x: 0, y: 1, w: 1, h: 1, i: '1' },
                { label: 'File Explorer', icon: 'explorer', x: 0, y: 2, w: 1, h: 1, i: '2' },
                { label: 'Microsoft Store', icon: 'store-' + this.iconTheme, x: 0, y: 3, w: 1, h: 1, i: '3' },
                { label: 'Microsoft Edge', icon: 'edge', x: 0, y: 4, w: 1, h: 1, i: '4' }
            ]
        },
        updateGrid() {
            this.cols = parseInt(document.querySelector('.desktop-icons').offsetWidth / 96)
            this.rows = parseInt(document.querySelector('.desktop-icons').offsetHeight / 112)
        }
    },
    mounted() {
        this.loadItems()
        this.updateGrid()

        window.onresize = () => {
            this.updateGrid()
        }

        document.addEventListener('click', (event) => {

            let ignoreClickOnMeElements = document.getElementsByClassName('vue-grid-item');
            let isClickInsideElement = false

            for (let element of ignoreClickOnMeElements) {
                if (element.contains(event.target)) {
                    isClickInsideElement = true
                    break
                }
            }

            if (!isClickInsideElement) {
                this.activeItem = '-1'
            }

        })

    }
}
</script>

<style lang="scss">

.desktop-icons {

    .app {
        .app-name {
            color: #fafafa;
            margin: 4px 0;
            text-shadow: 0 0 4px rgba(0, 0, 0, 0.6);
        }
    }

    .vue-grid-item {
        cursor: auto !important;
    }

    .vue-grid-item.vue-grid-placeholder {
        background: #ceceff !important;
    }

}

</style>