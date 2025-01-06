<template>
  <div class="q-pa-md">
    <q-scroll-area
      class="q-px-sm"
      style="
        width: 100%;
        flex-grow: 1;
        display: flex;
        flex-direction: column;
        position: relative;
      "
      @dragover.prevent
      @drop="onDrop"
      @dragenter="showPlaceholder = true"
      @dragleave="showPlaceholder = false"
    >
      <div v-if="showPlaceholder" class="placeholder"></div>
      
      <div
        v-for="(item, index) in allMessages"
        :key="item.message_id"
        :class="[
          'q-message-wrapper',
          'column',
          item.type === 'query' ? 'sent' : 'received',
        ]"
        style="position: relative"
        :data-message-id="item.message_id"
        :draggable="isDraggable(index)"
        @dragstart="onDragStart(item, index)"
      >
        <div
          class="bubble"
          :class="
            item.type === 'query'
              ? 'right-bottom --sent'
              : 'left-bottom --received'
          "
          :style="
            item.type === 'query' ? 'margin-right: 8px;' : 'margin-left: 8px;'
          "
          style="position: relative"
          :data-message-id="item.message_id"
        >
          <div
            style="
              text-align: justify;
              font-size: 16px;
              font-family: ui-sans-serif, -apple-system, system-ui, Segoe UI,
                Helvetica, Apple Color Emoji, Arial, sans-serif;
              line-height: 27px;
            "
            :style="{
              justifyContent: `${item.type === 'query' ? 'end' : 'start'}`,
            }"
          >
            {{ item.message }}
          </div>
        </div>
        <div
          class="btn-actions-msg-container"
          :class="item.type === 'query' ? 'q-mr-sm' : 'q-ml-sm'"
        >
          <div v-if="item.type === 'query'">
            <q-btn
              v-if="isLastQuery(index)"
              dense
              flat
              size="sm"
              class="btn-actions-msg rotate-90 handle"
              icon="unfold_more"
              @mousedown.prevent
              @click="startDrag"
            >
              <q-tooltip>Move query to another conversation.</q-tooltip>
            </q-btn>
          </div>
          <div v-else>
            <q-btn
              dense
              flat
              size="sm"
              class="btn-actions-msg"
              icon="content_copy"
            />
          </div>
        </div>
      </div>
    </q-scroll-area>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { defineProps } from 'vue';

const props = defineProps({
  tab: {
    type: Object,
    required: true,
  },
});

const allMessages = computed(() => {
  return [...props.tab.messages[0], ...props.tab.messages[1]];
});

let draggedItem = null;
let draggedFromArray = null;
const showPlaceholder = ref(false);

const startDrag = () => {
  const lastIndex = allMessages.value.length - 1;
  if (allMessages.value[lastIndex].type === "query") {
    draggedItem = allMessages.value[lastIndex];
    draggedFromArray = 'allMessages';
  }
};

const onDrop = () => {
  if (draggedItem) {
    // Logic to handle the drop
    // You can implement your logic here to move the item to the desired location
    console.log('Dropped item:', draggedItem);
    
    // Reset dragged item
    draggedItem = null;
    draggedFromArray = null;
    showPlaceholder.value = false; // Hide placeholder after drop
  }
};

const isDraggable = (index) => {
  return index === allMessages.value.length - 1 && allMessages.value[index].type === "query";
};

const isLastQuery = (index) => {
  return index === allMessages.value.length - 1 && allMessages.value[index].type === "query";
};
</script>

<style scoped>
.placeholder {
  border: 2px dotted #007bff; /* Change color as needed */
  height: 50px; /* Adjust height as needed */
  margin: 10px 0; /* Space around the placeholder */
  background-color: rgba(0, 123, 255, 0.1); /* Light background for visibility */
}
</style>
