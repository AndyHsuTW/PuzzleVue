<template>
  <view class="containerVert">
    <view class="containerVert">
      <text class="text-color-primary">{{msg}}</text>
      <text>Screen:{{Resolution.Width.toFixed(2)}}x{{Resolution.Height.toFixed(2)}} </text>
    </view>

    <view style="flex:4;">
      <!-- rows -->
      <view class="containerHoriz" v-for="(puzzleBoxRow,i) in PuzzleBoxes" v-bind:key="i" v-bind:style="{height:PuzzleBox.Size}">
        <!-- puzzle boxes -->
        <touchableOpacity v-for="(puzzleBox,j) in puzzleBoxRow" v-bind:key="j"
          v-bind:class="[puzzleBox.IsSelected?'puzzleBoxSelected':'puzzleBox','border']"
          v-bind:style="{width:PuzzleBox.Size,height:PuzzleBox.Size}"
          v-on:press="OnPuzzleBoxPressed(i,j)">
          <text>
            {{i}} {{j}}
          </text>
        </touchableOpacity>
      </view>

    </view>

  </view>
</template>

<script>
  import {
    Dimensions
  } from 'react-native';
  import {
    Accelerometer
  } from "expo";
  import {
    widthPercentageToDP as WidthPercentage,
    heightPercentageToDP as HeightPercentage
  } from 'react-native-responsive-screen';


  export default {
    data: function () {
      return {
        msg:"hello VUE v05300000",
        Resolution: {// Screen size
          Width: -1, 
          Height: -1
        },
        PuzzleBox: {
          Size: 0 // box width and height
        },
        PuzzleBoxes:[], // properties of each puzzle box
        PuzzleDimension: 9, // 9x9
        SelectedPuzzleBox:null,
      }
    },
    methods: {
      OnPuzzleBoxPressed: function (rowIndex, columnIndex) {
        if(this.SelectedPuzzleBox){
          this.SelectedPuzzleBox.IsSelected=false;
        }
        this.SelectedPuzzleBox = this.PuzzleBoxes[rowIndex][columnIndex];
        this.SelectedPuzzleBox.IsSelected = true;
      }
    },
    created:function(){
      // prepare puzzle boxes
      console.log(this.PuzzleDimension);
      for(var i=0;i<this.PuzzleDimension;i++){
        var boxRow = [];
        console.log(i);
        for (var j=0;j<this.PuzzleDimension;j++){
          console.log(j);
          var boxProperties={
            IsSelected:false
          };
          boxRow.push(boxProperties);
        }
          this.PuzzleBoxes.push(boxRow);
      }
    },
    mounted: function () {
      this.Resolution.Width = Dimensions.get('window').width;
      this.Resolution.Height = Dimensions.get('window').height;
      this.PuzzleBox.Size = WidthPercentage((90.0 / this.PuzzleDimension) + "%");
     
    }
  }
</script>
<style>
  .containerVert {
    background-color: white;
    align-items: center;
    justify-content: center;
    flex: 1;
    flex-direction: column;
    padding-top: 5px;
  }

  .border {
    border-width: 1;
    border-color: black;
    border-radius: 5;
  }

  .containerHoriz {
    flex-direction: row;
  }

  .text-color-primary {
    color: blue;
  }

  .puzzleBox {
    background-color: lightblue;
  }
  .puzzleBoxSelected{
    background-color: lightsalmon;
  }
</style>