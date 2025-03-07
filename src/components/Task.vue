<template>
    <div id="task">
        <div>
            <input type="checkbox" @change="completed(item.id)" :checked="item.isCompleted">
        </div>
        <div>
            <span :class="classCompleted" @dblclick="completed(item.id)">{{ item.task }}</span>
        </div>
        <div>
            <button @click="remove(item.id)">X</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'ComponentTask',
        props: {
            item: {
                type: Object,
                required: true
            }
        },
        methods: {
            completed(id){
                this.$emit("completed", id);
            },
            
            remove(id){
                this.$emit("remove", id);
            }
        },
        computed: {
            classCompleted () {
                return this.item.isCompleted ? 'isCompleted' : '';
            }
        }
    }
</script>

<style scoped>
    .isCompleted {
        text-decoration: line-through;
    }
    #task {
        width: 100%;
        display: grid;
        grid-template-areas: "div div div";
        grid-template-columns: 10% 80% 10%;
    }

    #task > div {
        display: flex;
    }

    #task > div:nth-of-type(odd) {
        align-items: center;
        justify-content: center;
    }

    #task > div  > span {
        word-break: break-all;
        -webkit-user-select: none; /* Safari */
        -ms-user-select: none; /* IE 10 and IE 11 */
        user-select: none; /* Standard syntax */
    }
</style>