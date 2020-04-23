<template>
  <div v-if="trigger">
    
    <v-layout
      v-for="indexRow in size"
      :key="'fila-'+indexRow">

      <v-card
        v-for="indexColumn in size"
        :key="'celda-'+indexColumn"
        raised
        width="7em"
        height="7em"
        style="text-align:center;   	vertical-align:middle;"
        :color="getColor(getCurrentCellDrawing(indexRow, indexColumn))">

        <br>
        <br>
        <h2>{{ getValueToDisplay(getCurrentCellDrawing(indexRow, indexColumn)) }}</h2>

      </v-card>
    </v-layout>

    <h1>Movements: {{ movements }}</h1>
    <h1>Score: {{ score }}</h1>

    <v-btn
      dark
      color="rgb(0, 125, 0)"
      @click="reset">
      Resetear
      <v-icon right dark>mdi-refresh</v-icon>
    </v-btn>

    <h2>Use "WASD" to move!</h2>
    <h2>You know the rules and so do I</h2>

  </div>
</template>

<script>
//Next line allows you to make console.log as you wish
/* eslint-disable */

export default {
  name: 'App',

  data: () => ({
    // You don't need to change anything here~
    trigger: true,
    score: 0,
    movements: 0,
    size: 4,

    // I will put this here for your testing, but remember to DISABLE generateTable in created(), line 65
    table: [
      2, 2, 128, 128,
      2, 2, 64, 64,
      0, 8, 128, 128,
      0, 16, 512, 512
    ]
    
  }),

  created(){
    this.generateTable();
    this.generateRandomNumber();

    var vueInstance = this;
    import ('./utils/shortcut.js')
    .then((shortcut) => {
      shortcut.default.add("W",function() {
        vueInstance.moveUp();
      });

      shortcut.default.add("S",function() {
        vueInstance.moveDown();
      });

      shortcut.default.add("A",function() {
        vueInstance.moveLeft();
      });

      shortcut.default.add("D",function() {
        vueInstance.moveRight();
      });

    });
  },

  methods:{
    // You don't need to change anything here~
    generateTable(){
      let totalSize = this.size * this.size;
      this.table = [];

      for(let index=0; index<totalSize; index++){
        this.table.push(0);
      }
    },

    repaint(){
      this.trigger = false;
      this.trigger = true;
    },

    getCurrentCellDrawing(indexRow, indexColumn){
      return (indexRow - 1) * this.size + indexColumn - 1;
    },

    getValueToDisplay(cellIndex){
      if(this.table[cellIndex] == 0){
        return ""; //So we display 'nothing' or looks empty
      }
      else{
        
        return this.table[cellIndex];
      }
    },

    generateRandomNumber(){
      // Each time I found a cell empty I add that index here
      let emptytable = [];

      for(let index=0; index<this.table.length; index++){
        if(this.table[index] == 0){
          emptytable.push(index);
        }
      }

      let randomEmptyIndex = Math.floor(Math.random() * Math.floor(emptytable.length));
      randomEmptyIndex = emptytable[randomEmptyIndex];

      let generate2or4 = Math.floor(Math.random() * Math.floor(2));
      if(generate2or4 == 0){
        let numberToAdd = 2;
        this.table[randomEmptyIndex] = numberToAdd;
      }
      else{
        let numberToAdd = 4;
        this.table[randomEmptyIndex] = numberToAdd;
      }
    },

    getColor(cellIndex){
      switch (this.table[cellIndex]) {
        case 0:
          return "#aaaaaa";
          break;
        case 2:
          return "#baaaaa";
          break;
        case 4:
          return "#bbaaaa";
          break;
        case 8:
          return "#cbaaaa";
          break;
        case 16:
          return "#ccaaaa";
          break;
        case 32:
          return "#dcaaaa";
          break;
        case 64:
          return "#ddaaaa";
          break;
        case 128:
          return "#edaaaa";
          break;
        case 256:
          return "#eeaaaa";
          break;
        case 512:
          return "#feaaaa";
          break;
        case 1024:
          return "#ffaaaa";
          break;
        case 2048:
          return "#ffffff";
          break;
        default:
          return "#aaaaaa";
          break;
      }
    },

    ///////////////////////////////////////////////////////////
    //////////     Your playground starts here       //////////
    ///////////////////////////////////////////////////////////

    //Remember add a comma after each function

    // What do you need do to restart the game?
    // 5 points
    reset(){
      location.reload(true);
    },

    // You need to show an alert when the user wins or looses the game
    // What triggers these functions is up to you!
    // 15 points
    victory(){
      alert("Congrats! You have completed 2048");
    },

    defeat(){
      alert("YOU LOOSE!");
      this.reset();
    },

    // You need to recreate the game!
    // 20 points
   moveUp(){
      for(var indexRow = 0; indexRow < 3; indexRow++){
        for(var indexColumn=0; indexColumn<4; indexColumn++){
          var moveToIndex = indexRow * 4 + indexColumn;
          var moveFromIndex = moveToIndex + 4;

          var moveToValue = this.table[moveToIndex];
          var moveFromValue = this.table[moveFromIndex];
          

          //Ahora, si los dos nÃºmeros, moveFromValue y moveToValue son iguales, quÃ© deberÃ­a pasar?
          if(moveFromValue == moveToValue && moveFromValue != 0){
            this.table[moveToIndex] = moveFromValue + moveFromValue;
            this.table[moveFromIndex] = 0;
          }
          //Si vas a moverte hacia una celda 'vacÃ­a' y eres diferente de 0
          else if(moveToValue == 0 && moveFromValue != 0){
            this.table[moveToIndex] = moveFromValue;
            this.table[moveFromIndex] = 0;
            indexRow = -1;
          }
        }
      }
      //this.generateRandomNumber();
      this.repaint();
      this.movements = this.movements + 1
    },

    // You need to recreate the game!
    // 20 points
    moveDown(){
  
      
      //this.generateRandomNumber();
      this.repaint();
      this.movements = this.movements + 1
    },

    // You need to recreate the game!
    // 20 points
    moveLeft(){
       for(var indexRow=0;indexRow<this.size;indexRow++){
        for(var indexColumn = 3; indexColumn > 0; indexColumn--){
          var moveToIndex = indexRow*this.size+indexColumn;
          var moveFromIndex = moveToIndex + 1;

          var moveToValue = this.table[moveToIndex];
          var moveFromValue = this.table[moveFromIndex];


          
          if(moveFromValue == moveToValue && moveFromValue != 0){
            this.table[moveToIndex] = moveFromValue * 2;
            this.table[moveFromIndex] = 0;
          }
          
          else if(moveToValue == 0 && moveFromValue != 0){
            this.table[moveToIndex] = moveFromValue;
            this.table[moveFromIndex] = 0;
            indexRow = -1;
          }
          
        }
      }
      
      this.generateRandomNumber();
      this.repaint();     
    },

    // You need to recreate the game!
    // 20 points
    moveRight(){
      for(var indexRow=0;indexRow<this.size;indexRow++){
        for(var indexColumn = 3; indexColumn > 0; indexColumn--){
          var moveToIndex = indexRow*this.size+indexColumn;
          var moveFromIndex = moveToIndex - 1;

          var moveToValue = this.table[moveToIndex];
          var moveFromValue = this.table[moveFromIndex];


          
          if(moveFromValue == moveToValue && moveFromValue != 0){
            this.table[moveToIndex] = moveFromValue * 2;
            this.table[moveFromIndex] = 0;
          }
         
          else if(moveToValue == 0 && moveFromValue != 0){
            this.table[moveToIndex] = moveFromValue;
            this.table[moveFromIndex] = 0;
            indexRow = -1;
          }
          
        }
      }
      
      this.generateRandomNumber();
      this.repaint();     
    }


  }
};
</script>
