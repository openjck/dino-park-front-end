<template>
  <div v-if="this.related.show" class="reporting-structure">
    <LoadingSpinner v-if="loading"></LoadingSpinner>
    <div v-if="this.related.manager" class="reporting-structure__reports-to">
      <h3>Reports to:</h3>
      <div>
        <Person modifier="person--borderless" v-bind="this.related.manager" />
      </div>
    </div>
    <div
      v-if="this.related.directs.length > 0"
      class="reporting-structure__manages"
    >
      <h3>Manages:</h3>
      <div>
        <Person
          v-for="(direct, index) in this.related.directs"
          :modifier="directsView"
          :key="`direct-${index}`"
          v-bind="direct"
        />
      </div>
    </div>
  </div>
</template>

<script>
import LoadingSpinner from '@/components/ui/LoadingSpinner.vue';
import Person from '@/components/ui/Person.vue';
import Related from '@/assets/js/related';

export default {
  name: 'ReportingStructure',
  props: {
    username: String,
    loading: false,
  },
  components: {
    LoadingSpinner,
    Person,
  },
  computed: {
    directsView() {
      return this.related.directs.length > 1
        ? 'person--borderless person--avatar-only'
        : 'person--borderless';
    },
  },
  watch: {
    username(u) {
      this.update_user(u);
    },
  },
  methods: {
    update_user(u = this.username) {
      this.related.update(u);
    },
  },
  created() {
    this.update_user();
  },
  data() {
    return {
      related: new Related(),
    };
  },
};
</script>

<style>
.reporting-structure__manages {
  margin-left: -1.5em;
  margin-right: -1.5em;
  padding: 0 1.5em 1.5em;
}
.reporting-structure__reports-to + .reporting-structure__manages {
  margin-top: 2em;
  border-top: 1px solid var(--gray-30);
  padding-top: 1.5em;
}
@media (min-width: 50em) {
  .reporting-structure__reports-to,
  .reporting-structure__manages {
    display: grid;
    grid-gap: 2em;
    grid-template-columns: 7em 1fr;
  }
}
</style>
