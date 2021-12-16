<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resources')"
      :mode="storedResButtonMode"
      >All Resources</base-button
    >
    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="addResButtonMode"
      >Add Resource</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      learningResources: [
        {
          id: '1',
          title: 'Vuejs for Noob',
          description: 'Only for Noobs',
          link: 'https://vuejs.org',
        },
        {
          id: '2',
          title: 'Vuejs for Noob level 2',
          description: 'Only for Noobs',
          link: 'https://vuejs.org',
        },
      ],
    };
  },
  provide() {
    return {
      resources: this.learningResources,
      addResource: this.addResource,
      deleteResource: this.removeResource,
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    },
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, url) {
      // muốn gọi method này bên AddResource thì dùng provide - inject
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
      };
      this.learningResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },
    removeResource(ResourceId) {
      const resIndex = this.learningResources.findIndex(
        (res) => res.id === ResourceId
      );
      this.learningResources.splice(resIndex, 1);
    },
  },
};
</script>
