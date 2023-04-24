<script setup lang="ts"></script>

<template>
  <div class="d-flex flex-column align-items-center">
    <input type="text" v-model="textInput" class="form-control input-grande" placeholder="Write your prompt..." />
    <br>
    <button @click="sendText" class="btn btn-primary">Send prompt</button>
    <div class="table-responsive" v-if="apiResponse">
      <table class="table table-striped">
        <thead>
          <tr>
            <th v-for="key in keys" :key="key">{{ key }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in apiResponse" :key="index">
            <td v-for="key in keys" :key="key">{{ item[key] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface ApiResponse {
  [key: string]: any;
}

export default defineComponent({
  data() {
    return {
      textInput: "",
      apiResponse: null as ApiResponse[] | null,
      keys: [] as string[]
    };
  },
  methods: {
    async sendText(): Promise<void> {
      try {
        const response = await fetch("https://aitosql.azurewebsites.net/AskTheDatabase", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({ prompt: this.textInput })
        });
        const data = await response.json();
        this.apiResponse = data;
        this.keys = Object.keys(data[0]);
      } catch (error) {
        console.error(error);
      }
    }
  }
});
</script>
