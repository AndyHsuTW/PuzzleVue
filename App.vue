<template>
  <view class="containerVert" style="flex: 1;" v-cloak v-bind:style="{opacity:IsInitialed?1:0}">
    <view name="InfoView" class="containerVert">
      <text class="text-color-primary">{{msg}}</text>
      <text>Screen:{{Resolution.Width.toFixed(2)}}x{{Resolution.Height.toFixed(2)}}</text>
    </view>

    <view name="PuzzleMain" class="puzzleMainArea" style="flex:4;">
      <!-- rows -->
      <view
        class="containerHoriz"
        v-for="(puzzleBoxRow,i) in PuzzleBoxes"
        v-bind:key="i"
        v-bind:style="{height:PuzzleBoxSettings.Size}"
      >
        <!-- puzzle boxes -->
        <touchableOpacity
          v-for="(puzzleBox,j) in puzzleBoxRow"
          v-bind:key="j"
          v-bind:class="[puzzleBox.IsSelected?'puzzleBoxSelected':'puzzleBox','border']"
          v-bind:style="{width:PuzzleBoxSettings.Size,height:PuzzleBoxSettings.Size}"
          v-on:press="OnPuzzleBoxPressed(puzzleBox)"
        >
          <text>{{puzzleBox.Text}}</text>
        </touchableOpacity>
      </view>
    </view>
    <view class="containerVert" style="flex: 2;">
      <!-- rows -->
      <view
        class="containerHoriz"
        v-for="(puzzleOptionsRow,i) in PuzzleBoxOptions"
        v-bind:key="i"
        v-bind:style="{height:PuzzleBoxSettings.Size}"
      >
        <!-- puzzle boxes -->
        <touchableOpacity
          v-for="(puzzleOption,j) in puzzleOptionsRow"
          v-bind:key="j"
          v-bind:class="[puzzleOption.IsSelected?'puzzleBoxSelected':'puzzleBox','border']"
          v-bind:style="{width:PuzzleBoxSettings.Size,height:PuzzleBoxSettings.Size}"
          v-on:press="OnPuzzleBoxOptionPressed(puzzleOption)"
        >
          <text>{{puzzleOption.Text}}</text>
        </touchableOpacity>
      </view>
    </view>
  </view>
</template>

<script>
import { Dimensions } from "react-native";
import { Accelerometer } from "expo";
import {
  widthPercentageToDP as WidthPercentage,
  heightPercentageToDP as HeightPercentage
} from "react-native-responsive-screen";

export default {
  data: function() {
    return {
      msg: "hello VUE v05312300",
      IsInitialed: false,
      Resolution: {
        // Screen size
        Width: -1,
        Height: -1
      },
      PuzzleBoxSettings: {
        Size: 0 // box width and height
      },
      PuzzleBoxes: [], // properties of each puzzle box
      PuzzleBoxOptions: [],
      PuzzleDimension: 9, // 9x9
      SelectedPuzzleBox: null
    };
  },
  methods: {
    OnPuzzleBoxPressed: function(puzzleBox) {
      if (this.SelectedPuzzleBox) {
        this.SelectedPuzzleBox.IsSelected = false;
      }
      this.SelectedPuzzleBox = puzzleBox;
      this.SelectedPuzzleBox.IsSelected = true;
    },
    OnPuzzleBoxOptionPressed: function(puzzleOption) {
      if (!this.SelectedPuzzleBox) {
        return;
      }
      this.SelectedPuzzleBox.Text = puzzleOption.Text;
    }
  },
  created: function() {
    // prepare puzzle boxes
    for (var i = 0; i < this.PuzzleDimension; i++) {
      var boxRow = [];
      for (var j = 0; j < this.PuzzleDimension; j++) {
        var boxProperties = {
          IsSelected: false,
          Text: `${i},${j}`,
          IsShow: true
        };
        boxRow.push(boxProperties);
      }
      this.PuzzleBoxes.push(boxRow);
    }

    //puzzle options
    var optionIndex = 0;
    for (var i = 0; i < 2; i++) {
      var boxRow = [];
      for (var j = 0; j < this.PuzzleDimension; j++) {
        var boxProperties = {
          IsSelected: false,
          Text: `${++optionIndex}`,
          IsShow: true
        };
        boxRow.push(boxProperties);
      }
      this.PuzzleBoxOptions.push(boxRow);
    }
  },
  mounted: function() {
    this.Resolution.Width = Dimensions.get("window").width;
    this.Resolution.Height = Dimensions.get("window").height;
    this.PuzzleBoxSettings.Size = WidthPercentage(
      90.0 / this.PuzzleDimension + "%"
    );
    this.IsInitialed = true;
  }
};
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
.puzzleBoxSelected {
  background-color: lightsalmon;
}
.puzzleMainArea {
  background-color: gray;
  padding: 5px;
}
[v-cloak] {
  opacity: 0;
}
</style>