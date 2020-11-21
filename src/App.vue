<template>
  <div class="container grid-lg my-2 py-2">
    <div class="card mb-2" v-if="listenQuotes.length > 0">
      <div class="card-header">
        <div class="h4">Acompanhando</div>
      </div>
      <div class="card-body">
        <WatchListQuotes :listen-quotes="listenQuotes" @unlisten="onUnlisten" />
      </div>
    </div>

    <div class="card">
      <div class="card-header">
        <div class="h4">Todas as moedas</div>
      </div>
      <div class="card-body">
        <ListQuotes
          :quotes="quotes"
          :listen-quotes="listenQuotes"
          @listen="onListen"
          @unlisten="onUnlisten"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, toRefs } from "vue";
import api from "@/services/api";
import ListQuotes from "./components/ListQuotes";
import WatchListQuotes from "./components/WatchListQuotes";

export default {
  name: "App",
  components: { ListQuotes, WatchListQuotes },
  setup() {
    const data = reactive({
      quotes: {},
      listenQuotes: [],
    });

    onMounted(async () => {
      const response = await api.all();
      data.quotes = response.data;
    });

    function onListen(code) {
      data.listenQuotes.push(code);
    }

    function onUnlisten(code) {
      data.listenQuotes = data.listenQuotes.filter((key) => key !== code);
    }

    return {
      ...toRefs(data),
      onListen,
      onUnlisten,
    };
  },
};
</script>
