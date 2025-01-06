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
          v-for="(item, index) in dummyMessages1"
          :key="item.message_id"
          :class="[
            'q-message-wrapper',
            'column',
            item.type === 'query' ? 'sent' : 'received',
          ]"
          style="position: relative"
          :data-message-id="item.message_id"
          draggable="true"
          @dragstart="onDragStart(item, index, 'dummyMessages1')"
          :draggable="isDraggable(index, dummyMessages1)"
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
                v-if="index === dummyMessages1.length - 1"
                dense
                flat
                size="sm"
                class="btn-actions-msg rotate-90 handle"
                icon="unfold_more"
                @mousedown.prevent
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
        @drop="onDrop('dummyMessages2')"
      >
        <div
          v-for="(item, index) in dummyMessages2"
          :key="item.message_id"
          :class="[
            'q-message-wrapper',
            'column',
            item.type === 'query' ? 'sent' : 'received',
          ]"
          style="position: relative"
          :data-message-id="item.message_id"
          draggable="true"
          @dragstart="onDragStart(item, index, 'dummyMessages2')"
          :draggable="isDraggable(index, dummyMessages2)"
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
                v-if="index === dummyMessages2.length - 1"
                dense
                flat
                size="sm"
                class="btn-actions-msg rotate-90 handle"
                icon="unfold_more"
                @mousedown.prevent
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

const dummyMessages1 = ref([
  {
    message_id: "001",
    type: "query",
    message: "What is the capital of France?",
  },
  {
    message_id: "002",
    type: "response",
    message: "The capital of France is Paris.",
  },
  {
    message_id: "003",
    type: "query",
    message: "Can you translate this text into Spanish?",
  },
]);

const dummyMessages2 = ref([
  {
    message_id: "011",
    type: "query",
    message: "What is the population of Earth in 2025?",
  },
  {
    message_id: "012",
    type: "response",
    message: "The estimated population of Earth in 2025 is 8.1 billion.",
  },
]);

let draggedItem = null;
let draggedFromArray = null;

const onDragStart = (item, index, arrayName) => {
  if (index === eval(arrayName).value.length - 1 && item.type === "query") {
    draggedItem = item;
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
  }
};

const isDraggable = (index, array) => {
  return index === array.length - 1 && array[index].type === "query";
};
</script>
