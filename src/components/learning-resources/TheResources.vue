<template>
  <base-card>
    <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode">
      Stored Resources
    </base-button>
    <base-button @click="setSelectedTab('add-resource')" :mode="addResButtonMode">
      Add Resource
    </base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<!-- click is not a custom event that we emit there in our custom component BaseButton but we are able to
use it as a listener within the component where it is being used. When we add props or event
listeners on custom components then by default they fall through to the root level element in that
custom component's template -->

<script>
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

export default {
  components: {
    StoredResources,
    AddResource
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'official-guide',
          description: 'The official Vue.js documentation.',
          link: 'https://vuejs.org'
        },
        {
          id: 'google',
          title: 'Google',
          description: 'When in doubt.. go to Google',
          link: 'https://www.google.com/'
        }
      ]
    }
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      removeResource: this.removeResource
    }
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab
    },
    addResource(title, description, url) {
      // store a new resource
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url
      }
      // add a new resource
      this.storedResources.unshift(newResource)
      // Switch the tab to stored resources
      this.selectedTab = 'stored-resources'
    },
    removeResource(resourceId) {
      const resourceIndex = this.storedResources.findIndex((res) => res.id === resourceId)
      this.storedResources.splice(resourceIndex, 1)
    }
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat'
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resources' ? null : 'flat'
    }
  }
}
</script>
