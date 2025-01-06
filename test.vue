<template>
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
    <VueDraggable
      v-model="props.tab.messages"
      :animation="150"
      group="messages"
      handle=".handle"
      filter=".received"
      :sort="false"
    >
      <div
        v-for="(item, index) in props.tab.messages"
        :key="item.message_id"
        :class="[
          'q-message-wrapper',
          'column',
          item.type === 'query' ? 'sent' : 'received',
        ]"
        style="position: relative"
        :data-message-id="item.message_id"
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
              v-if="index === props.tab.messages.length - 1"
              dense
              flat
              size="sm"
              class="btn-actions-msg rotate-90 handle"
              icon="unfold_more"
              style="pointer-events: none !important"
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
    </VueDraggable>
  </q-scroll-area>
</template>

<script setup>
import { ref } from "vue";
import { VueDraggable } from "vue-draggable-plus";

const props = defineProps(["tab"]);

const dummyMessages1 = ref([
  {
    message_id: "001",
    type: "query", // Enviado
    message:
      "What is the capital of France? Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.",
  },
  {
    message_id: "002",
    type: "response", // Recibido
    message: "The capital of France is Paris.",
  },
  {
    message_id: "003",
    type: "query", // Enviado
    message: "Can you translate this text into Spanish?",
  },
  {
    message_id: "004",
    type: "response", // Recibido
    message:
      "Sure, the translation is: '¿Puedes traducir este texto al español?'.",
  },
  {
    message_id: "005",
    type: "query", // Enviado
    message: "Summarize the main points of this article.",
  },
  {
    message_id: "006",
    type: "response", // Recibido
    message:
      "The main points of the article are: 1. Introduction to AI, 2. Benefits of AI, 3. Future of AI.",
  },
  {
    message_id: "007",
    type: "query", // Enviado
    message: "Generate a list of pros and cons for remote work.",
  },
  {
    message_id: "008",
    type: "response", // Recibido
    message:
      "Pros: Flexibility, Reduced commute. Cons: Isolation, Distractions at home.",
  },
  {
    message_id: "009",
    type: "response", // Recibido
    message:
      "This is an additional received message to test alignment and buttons.",
  },
  {
    message_id: "010",
    type: "query", // Enviado
    message: "What are the benefits of learning Vue.js?",
  },
]);
const dummyMessages2 = ref([
  {
    message_id: "011",
    type: "query", // Enviado
    message: "What is the population of Earth in 2025?",
  },
  {
    message_id: "012",
    type: "response", // Recibido
    message:
      "The estimated population of Earth in 2025 is approximately 8.1 billion people.",
  },
  {
    message_id: "013",
    type: "query", // Enviado
    message: "List the key differences between React and Vue.js.",
  },
  {
    message_id: "014",
    type: "response", // Recibido
    message:
      "React is a library focused on building UI, while Vue.js is a framework. React uses JSX, Vue uses templates. Vue is easier for beginners, React has more ecosystem support.",
  },
  {
    message_id: "015",
    type: "query", // Enviado
    message: "Provide an example of a recursive function in JavaScript.",
  },
  {
    message_id: "016",
    type: "response", // Recibido
    message:
      "Here is an example: \n```javascript\nfunction factorial(n) {\n  return n === 0 ? 1 : n * factorial(n - 1);\n}\nconsole.log(factorial(5)); // Output: 120\n```",
  },
  {
    message_id: "017",
    type: "query", // Enviado
    message: "Explain the concept of closure in JavaScript.",
  },
  {
    message_id: "018",
    type: "response", // Recibido
    message:
      "A closure is the combination of a function and its lexical environment within which it was declared. Example: \n```javascript\nfunction outer() {\n  let count = 0;\n  return function inner() {\n    count++;\n    return count;\n  };\n}\nconst counter = outer();\nconsole.log(counter()); // 1\nconsole.log(counter()); // 2\n```",
  },
  {
    message_id: "019",
    type: "response", // Recibido
    message: "Testing a long message alignment to ensure UI consistency.",
  },
  {
    message_id: "020",
    type: "query", // Enviado
    message: "How can I improve my coding skills?",
  },
]);
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
