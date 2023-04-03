<template>
    <v-container>
      <v-row>
        <v-col>
          <v-text-field v-model="state.sizeX" label="Size X" type="number"></v-text-field>
        </v-col>
        <v-col>
          <v-text-field v-model="state.sizeY" label="Size Y" type="number"></v-text-field>
        </v-col>
        <v-col>
          <v-btn color="primary" @click="createGrid">Create Grid</v-btn>
        </v-col>
      </v-row>
  
      <v-row>
        <v-col cols="12">
          <div class="grid-container" >
            <div v-for="(row, y) in state.grid" :key="y" class="grid-row">
              <div v-for="(col, x) in row" :key="x"
                class="grid-square"
                :style="{ backgroundColor: col.color }"
                @mouseenter="changeColor(y,x)"
              ></div>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import { reactive } from 'vue';
  import _ from "lodash";
  //  @mouseover="toggleCell(rowIndex, cellIndex)">
  export default {
    name: 'Grid',
    setup() {
      const initialState = {
        sizeX: null,
        sizeY: null,
        grid:[]
      }
      const state = reactive({
        ...initialState
      });

      const createGrid = () => {
        const newGrid = [];
        for (let y = 0; y < state.sizeY; y++) {
            const row = [];
            for (let x = 0; x < state.sizeX; x++) {
                row.push({ color: 'white' });
            }
            newGrid.push(row);
        }
        state.grid = newGrid;
      };

      const changeColor = (y,x) =>{
        if(state.grid[y][x].color == "white"){
            state.grid[y][x].color = "blue"
        }else{
            state.grid[y][x].color = "white"
        }
      }
      return {
        state,
        createGrid,
        changeColor
      };
    },
  };
  </script>
  
<style>
    .grid-container {
        display: flex;
        flex-wrap: wrap;
        background: #000;
    }
    .grid-square {
        width: 36px;
        height: 36px;
        background-color: white;
        border: 1px solid black;
    }
</style>
  