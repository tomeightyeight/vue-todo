<template>
  <div>
    <h1>Task List <span v-show="remaining">({{ remaining }})</span></h1>

    <ul v-if="total">
      <transition-group name="fade">
        <li :class="{ 'completed': item.completed }"
            v-for="(item, index) in tasks"
            :key="item.id"
        >
          <span @click="toggleCompleted(item.id)">{{ item.title }}</span>
          <button @click="deleteTask(index)">X</button>
        </li>
      </transition-group>
    </ul>

    <p v-else>There are no tasks to display.</p>
  </div>
</template>

<script>
'use strict';

import {
  DELETE_TASK,
  TOGGLE_COMPLETED
} from '../stores/MutationTypes';

export default {
  name: 'task-list',

  props: ['tasks'],

  computed: {
    remaining () {
      return this.$store.getters.totalCompleted;
    },

    total () {
      return this.tasks.length;
    }
  },

  methods: {
    deleteTask (index) {
      this.$store.commit({
        type: DELETE_TASK,
        index: index
      });
    },

    toggleCompleted (id) {
      this.$store.commit({
        type: TOGGLE_COMPLETED,
        id: id
      });
    },
  }
};
</script>

<style lang="scss" scoped>
h1 {
  margin-top: 0;
}

ul {
  list-style-type: none;
}

.completed {
  text-decoration: line-through;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-active {
  opacity: 0;
}
</style>
