<script setup>
import { reactive } from 'vue';
import  InputNew  from './InputNew.vue';


let boards = reactive([
    {
        id: crypto.randomUUID(),
        name: 'Tablero 1',
        items: [
            {
                id: crypto.randomUUID(),
                title: 'Feature de Tasks',
            },
            {
                id: crypto.randomUUID(),
                title: 'Resolver bug',
            }
        ]
    },

    {
        id: crypto.randomUUID(),
        name: 'Tablero 2',
        items: [
            {
                id: crypto.randomUUID(),
                title: 'Mandar Tablero',
            },
            {
                id: crypto.randomUUID(),
                title: 'Code Review',
            }
        ]
    },

    // {
    //     id: crypto.randomUUID(),
    //     name: 'Tablero 3',
    //     items: [
    //         {
    //             id: crypto.randomUUID(),
    //             title: 'Feature de Tasks',
    //         },
    //         {
    //             id: crypto.randomUUID(),
    //             title: 'Resolver bug'
    //         }
    //     ]
    // }
]);

function handleNewItem(text, board){
    board.items.push({
        id: crypto.randomUUID(),
        title: text.value
    })
}

function handleNewBoard(){
    const name = prompt('Name of the board');
    if(!!name){
        boards.push({
            id: crypto.randomUUID(),
        name: name,
        items: [],
        });
    } 
}

const startDrag=(e, board, item)=>{
    e.dataTransfer.setData(
        'text/plain', 
        JSON.stringify({boardID: board.id, itemID:item.id})
    );
}

const onDrop=(e, dest)=>{
    const {boardID, itemID} = JSON.parse(
        e.dataTransfer.getData('text/plain'),
        );
    console.log(boardID, itemID);    
    const originBoard = boards.find(item => item.id === boardID);
    const originItem = originBoard.items.find(item => item.id === itemID);

    dest.items.push({...originItem});
    originBoard.items = originBoard.items.filter(item => item !== originItem);
}

</script>

<template class='body-item'>
<nav>
    <ul>
        <li><a href="#" @click.prevent="handleNewBoard">Create Board</a></li>
    </ul>
</nav>

<div class="boards-container">
    <div class="boards">
        <div class="board" 
        @drop="onDrop($event, board)" 
        @dragover.prevent 
        @dragenter.prevent 
        v-for="board in boards" 
        :key="board.id"
        >
            <div class="view">
                
                {{ board.name }}
            </div>

            
            <div class="items">
                <div class="item" 
                draggable="true"
                @dragstart="startDrag($event, board, item)"
                v-for="item in board.items" 
                :key="item.id"
                >
                {{ item.title }}
            </div>
            <InputNew @on-new-item="(text) => handleNewItem(text, board)" />
        </div>
    </div>
    </div>
</div>
</template>

<style scoped>
.boards-container{
background-image: url(./src/assets/abstract_background_with_flowing_particles_design_2003.jpg);
height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
}
nav{
    background-color: #000;
    
}

nav ul{
    list-style-type: none;
    padding: 0;
    margin: 0;
    display: flex;
}

nav ul li a{
    display: block;
    padding: 10px;
    color: #efefef;
    text-decoration: none;
}
.view{
    color: #efefef;
    margin: 0 0 8px;
}
.boards{
    display: flex;
    gap: 10px;
    padding-top: 8px;
    
}

.board{
    background: #1D2125;
    padding: 10px;
    border-radius: 8px;
    
}

.items{
    display: flex;
    flex-direction: column;
    border-radius: 8px;
    gap: 5px;
    cursor: pointer;
}

.item{
    background-color: white;
    border-radius: 8px;
    padding: 10px;
    box-sizing: border-box;
}

</style>