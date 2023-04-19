<template>
  <nav>
    <ul>
      <li><a href="#" @click.prevent="handleNewBoard">Create board</a></li>
    </ul>
  </nav>

  <div class="boards-container">
    <div class="boards">
      <div class="board" 
           v-for="board in boards" 
           :key="board.id"
           @drop="onDrop($event, board)"
           @dragover.prevent
           @dragenter.prevent
      >
        <div>{{ board.name }}</div>
        <Input @on-new-item="(text) => handleNewItem(text, board)" class= "inputComponent"/>
        <div class="items">
          <div class="item" 
               v-for="item in board.items" 
               :key="item.id"
               draggable="true"
               @dragstart="onDrag($event, board, item)"
              >
            {{ item.title }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Input from "./Input.vue";

  let boards = ref([
    {
      id: crypto.randomUUID(),
      name: 'Tablero 1',
      items: [
        {
          id: crypto.randomUUID(),
          title: 'Feature de archivos'
        },
        {
          id: crypto.randomUUID(),
          title: 'Resolver bug'
        },
      ]
    },
    {
      id: crypto.randomUUID(),
      name: 'Tablero 2',
      items: [
        {
          id: crypto.randomUUID(),
          title: 'Mandar reporte'
        },
        {
          id: crypto.randomUUID(),
          title: 'Code Review'
        },
      ]
    },
    

  ])
    const handleNewItem = (text, board) => {
      board.items.push({
        id: crypto.randomUUID,
        title: text.value
      })
    }

    const handleNewBoard = () => {
      const name = prompt('Name of the board')
      if (!!name) {
        console.log(name)
        boards.value.push({
          id: crypto.randomUUID(),
          name: name,
          items: []
        })
      }
    }

    const onDrop = (event, board) => {
      const {boardID, itemID} = JSON.parse(
        event.dataTransfer.getData('text/plain')
      )

      const originBoard = boards.value.find(item => item.id === boardID)
      const originItem = originBoard.items.find(item => item.id === itemID)

      board.items.push({...originItem})
      originBoard.items = originBoard.items.filter(item => item !== originItem)
    }

    const onDrag = (event, board, item) => {
      event.dataTransfer.setData(
        'text/plain',
        JSON.stringify({boardID: board.id, itemID: item.id})
      )
    }

</script>

<style  scoped>
  .boards {
    display: flex;
    gap: 10px;
    
  }

  .board {
    background: #afababf4;
    padding: 10px;
    border-radius: 3px;
  }

  .items {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .item {
    background-color: rgba(40, 222, 77, 0.41);
    padding: 10px;
    box-sizing: border-box;
    border-radius: 3px;
    box-shadow: 4px 4px 18px  rgba(28, 30, 29, 0.236);
  }

  
</style>