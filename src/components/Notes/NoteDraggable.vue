<template>
  <div
    class="draggable note"
    draggable="true"
    @dragstart="dragstart($event, item)"
    @click="$emit('click')"
  >
    {{ item.name }}
    <div class="buttons">
      <div
        class="btn delete"
        @click="$store.commit('deleteNote', item.id)"
      >
        ❌
      </div>
    </div>
  </div>
  

</template>

<script lang="ts">
import Vue from 'vue';
import { Note } from '../../store/index';

export default Vue.extend({
  name: 'NoteDraggable',
  props: {
    item: { type: Object as () => Note, required: true },
  },
  data () {
    return {
      dragging: false,
    };
  },
  methods: {
    dragstart (e:DragEvent, item:Element) {
      if (e.dataTransfer) {
        e.dataTransfer.dropEffect = "move"
        e.dataTransfer.effectAllowed = "move"
        e.dataTransfer.setData('itemID', item.id)
      }
    },
  },
});
</script>

<style scoped lang="scss">
  .draggable {
    position: relative;
    background-color: rgba(0, 0, 0, 0.1);
    cursor: move;
    padding: 0.2em;
    border-radius: 0.2em;
    &:hover{  
      & .btn{
        visibility: visible;
        opacity: 1;
      }    
    }
  }
  .buttons {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0;
    right: 0.2em;
    top: 0;
    height: 95%;
  }
  .btn {
    border-radius: 0.2em;
    height: 1.5em;
    width: 1.5em;
    font-size: 0.6em;
    cursor: pointer;
    visibility: hidden;
    opacity: 0;
    transition: 0.3s ease-in;
    &.delete:hover{
      background: rgba(0, 0, 0, 0.1);
    }
  }
</style>
