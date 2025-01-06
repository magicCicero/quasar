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
            @dragover.prevent
            @drop="onDrop('dummyMessages1')"
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
            @dragover.prevent
            @drop="onDrop('dummyMessages2')"
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

  
<style scoped>
/* .floating-copy-btn {
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s ease-in-out;
} */

/* .floating-copy-btn:hover {
  transform: scale(1.1);
}
 */
/* source: https://w2ui.com/web/blog/2/Speech-Bubble-in-Pure-CSS */
div.bubble {
  background: #fffffff2;
  display: inline-block;
  border: 1px solid #aaa;
  border-radius: 4px;
  color: black;
  /* box-shadow: 2px 2px 8px #ddd; */
  padding: 5px 10px;
  line-height: 130%;
  font-size: 13px;
  white-space: pre-line;
  max-width: 90%;
}
div.bubble.--received {
  background: #81c784;
  border: 1px solid #81c784;
}
div.bubble.--sent {
  background: #e0e0e0;
  border: 1px solid #e0e0e0;
}
body.body--dark div.bubble {
  box-shadow: none;
  background: #1a1a1a;
  color: lightgray;
}
body.body--dark div.bubble.--received {
  border: 1px solid #008f39;
}
body.body--dark div.bubble.--sent {
  border: 1px solid #55555594;
}

/* Left bubble indicators */
div.bubble.left-top:before {
  content: "";
  position: absolute;
  width: 8px;
  height: 8px;
  border-left: 1px solid #aaa;
  border-left-color: inherit;
  border-top: 1px solid #aaa;
  border-top-color: inherit;
  background-color: inherit;
}
div.bubble.left-top:before {
  margin: 1px 0 0 -15px;
  transform: rotate(-45deg);
  -moz-transform: rotate(-45deg);
  -ms-transform: rotate(-45deg);
  -o-transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}
div.bubble.left-bottom:after {
  content: "";
  position: absolute;
  width: 8px;
  height: 8px;
  border-left: 1px solid #aaa;
  border-left-color: inherit;
  border-top: 1px solid #aaa;
  border-top-color: inherit;
  background-color: inherit;
}
div.bubble.left-bottom:after {
  display: block;
  margin: -11px 0 0 -15px;
  transform: rotate(-45deg);
  -moz-transform: rotate(-45deg);
  -ms-transform: rotate(-45deg);
  -o-transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}

/* Right bubble indicators */
div.bubble.right-top:before {
  content: "";
  position: absolute;
  width: 8px;
  height: 8px;
  border-right: 1px solid #aaa;
  border-right-color: inherit;
  border-top: 1px solid #aaa;
  border-top-color: inherit;
  background-color: inherit;
  right: 4px;
}
div.bubble.right-top:before {
  margin: 1px 0 0 15px;
  transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}
div.bubble.right-bottom:after {
  content: "";
  position: absolute;
  width: 8px;
  height: 8px;
  border-right: 1px solid #aaa;
  border-right-color: inherit;
  border-top: 1px solid #aaa;
  border-top-color: inherit;
  background-color: inherit;
  right: -4px;
}
div.bubble.right-bottom::after {
  display: block;
  margin: -11px 0 0 15px;
  transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}

/* Top and bottom bubble indicators */
div.bubble.top:before {
  content: "";
  position: absolute;
  width: 8px;
  height: 8px;
  border-left: 1px solid #aaa;
  border-left-color: inherit;
  border-top: 1px solid #aaa;
  border-top-color: inherit;
  background-color: inherit;
}
div.bubble.top:before {
  margin: -10px 0 0 0px;
  transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}
div.bubble.bottom:after {
  display: block;
  margin: 2px 0 0 0px;
  transform: rotate(-135deg);
  -moz-transform: rotate(-135deg);
  -ms-transform: rotate(-135deg);
  -o-transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
}
div.bubble.bottom:after {
  content: "";
  position: absolute;
  width: 8px;
  height: 8px;
  border-left: 1px solid #aaa;
  border-left-color: inherit;
  border-top: 1px solid #aaa;
  border-top-color: inherit;
  background-color: inherit;
}

/* ****************** */

.scroll {
  flex-grow: 1;
}
/* .animated-text .line {
  display: block;
  opacity: 0;
  animation: fadeIn 0.5s forwards ease-in-out;
}

@keyframes fadeIn {
  to {
    opacity: 1;
  }
} */

.btn-actions-msg {
  color: #666;
}
.btn-actions-msg:hover {
  color: #051531;
}
.q-message-wrapper {
  display: flex;
  justify-content: flex-start; /* Alineación por defecto (izquierda) */
  margin-bottom: 0px !important;
}
.q-message-wrapper.column.sent {
  align-items: flex-end;
}
.q-message-wrapper.column.received {
  align-items: flex-start;
}
.q-chat-message {
  max-width: 90%; /* Limitar el ancho del mensaje */
}
.q-chat-message .q-message-text {
  word-wrap: break-word;
}
.q-message {
  margin-bottom: 0px;
}
.q-message-container {
  margin-bottom: 0px;
}
.q-message-text:last-child {
  min-height: 0px;
}
</style>
