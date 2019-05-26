    <template>
        <touchableOpacity
          v-bind:class="[SelectedPuzzleIndex!=((i-1)*PuzzleDimension+j)?'puzzleBox':'puzzleBoxSelected','border']"
          v-bind:style="{width:PuzzleBox.Size,height:PuzzleBox.Size}"
          v-on:press="OnPuzzleBoxPressed(((i-1)*PuzzleDimension+j))">
          <text>
            {{(i-1)*PuzzleDimension+j}}
          </text>
        </touchableOpacity>
    </template>
    
    <script>
    export default {
        
    }
    </script>
