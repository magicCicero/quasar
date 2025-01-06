<template>
  <div class="q-pa-md row">
    <!-- First Array -->
    <div class="col">
      <q-scroll-area
        class="q-px-sm"
        style="
          width: 100%;
          flex-grow: 1;
          display: flex;
          flex-direction: column;
          position: relative;
        "
      >
        <div
          v-for="(item, index) in messages1"
          :key="item.message_id"
          :class="[
            'q-message-wrapper',
            'column',
            item.type === 'query' ? 'sent' : 'received',
          ]"
          style="position: relative"
          :data-message-id="item.message_id"
          :draggable="isDraggable(index, messages1)"
          @dragstart="onDragStart(item, index, 'messages1')"
          @dragover.prevent
          @drop="onDrop('messages1')"
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
                v-if="index === messages1.length - 1"
                dense
                flat
                size="sm"
                class="btn-actions-msg rotate-90 handle"
                icon="unfold_more"
                @mousedown.prevent
                @click="startDrag('messages1')"
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

    <!-- Second Array -->
    <div class="col">
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
        @drop="onDrop('messages2')"
        @dragenter="showPlaceholder = true"
        @dragleave="showPlaceholder = false"
      >
        <div
          v-if="showPlaceholder"
          class="placeholder"
        ></div>
        <div
          v-for="(item, index) in messages2"
          :key="item.message_id"
          :class="[
            'q-message-wrapper',
            'column',
            item.type === 'query' ? 'sent' : 'received',
          ]"
          style="position: relative"
          :data-message-id="item.message_id"
          :draggable="isDraggable(index, messages2)"
          @dragstart="onDragStart(item, index, 'messages2')"
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
                v-if="index === messages2.length - 1"
                dense
                flat
                size="sm"
                class="btn-actions-msg rotate-90 handle"
                icon="unfold_more"
                @mousedown.prevent
                @click="startDrag('messages2')"
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
  </div>
</template>

<script setup>
import { ref } from "vue";
import { defineProps } from 'vue';

const props = defineProps({
  tab: {
    type: Object,
    required: true,
  },
});

const messages1 = ref(props.tab.messages[0]); // Assuming the first array of messages
const messages2 = ref(props.tab.messages[1]); // Assuming the second array of messages

let draggedItem = null;
let draggedFromArray = null;
const showPlaceholder = ref(false);

const startDrag = (arrayName) => {
  const array = eval(arrayName);
  const lastIndex = array.value.length - 1;
  if (array.value[lastIndex].type === "query") {
    draggedItem = array.value[lastIndex];
    draggedFromArray = arrayName;
  }
};

const onDrop = (targetArrayName) => {
  if (draggedItem && draggedFromArray !== targetArrayName) {
    const fromArray = eval(draggedFromArray).value;
    const toArray = eval(targetArrayName).value;

    // Remove from original array
    fromArray.splice(fromArray.length - 1, 1);

    // Add to the target array
    toArray.push(draggedItem);

    // Reset dragged item
    draggedItem = null;
    draggedFromArray = null;
    showPlaceholder.value = false; // Hide placeholder after drop
  }
};

const isDraggable = (index, array) => {
  return index === array.length - 1 && array[index].type === "query";
};
</script>

<style scoped>
.placeholder {
  border: 2px dotted #007bff;
  height: 50px;
  margin: 10px 0;
  background-color: rgba(0, 123, 255, 0.1);
}
</style>
