<template>
  <view class="containerVert">
    <view class="containerVert">
      <text class="text-color-primary">{{msg}}</text>
      <text>Screen:{{Resolution.Width.toFixed(2)}}x{{Resolution.Height.toFixed(2)}} </text>
    </view>

    <view style="flex:4;">
      <!-- rows -->
      <view class="containerHoriz" v-for="i in PuzzleDimension" v-bind:key="i" v-bind:style="{height:PuzzleBox.Size}">
        <!-- puzzle boxes -->
        <touchableOpacity v-for="j in PuzzleDimension" v-bind:key="j"
          v-bind:class="[SelectedPuzzleIndex!=((i-1)*PuzzleDimension+j)?'puzzleBox':'puzzleBoxSelected','border']"
          v-bind:style="{width:PuzzleBox.Size,height:PuzzleBox.Size}"
          v-on:press="OnPuzzleBoxPressed(((i-1)*PuzzleDimension+j))">
          <text>
            {{(i-1)*PuzzleDimension+j}}
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
        msg: "Hello World",
        Resolution: {
          Width: -1,
          Height: -1
        },
        PuzzleBox: {
          Size: 0
        },
        PuzzleDimension: 9, // 9x9
        SelectedPuzzleIndex:-1,
      }
    },
    methods: {
      OnPuzzleBoxPressed: function (pressedIndex) {
        this.SelectedPuzzleIndex = pressedIndex;
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