<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>Tip-Tap Demo</v-toolbar-title>
      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/drewalth/tip-tap-starter"
        target="_blank"
        text
      >
        <span class="mr-2">Github</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row justify="center" align="center">
          <v-col sm="12" md="6" lg="6">
            <v-card>
              <div class="d-flex align-center pr-2">
                <v-card-title>
                  ContentEditor.vue
                </v-card-title>
                <v-spacer></v-spacer>
                <v-btn v-if="editMode" class="mr-2" @click.exact="handleCancel"
                  >Cancel</v-btn
                >
                <v-btn depressed color="secondary" @click.exact="handleToggle">
                  {{ toggleText }}
                </v-btn>
              </div>
              <v-divider v-if="!editMode"></v-divider>
              <div v-if="!editMode" v-html="initialCopy" class="py-4 px-4" />
              <div v-if="editMode">
                <ContentEditor
                  showControlBar
                  :content="initialCopy"
                  @content:updated="handleUpdate"
                />
              </div>
            </v-card>
          </v-col>
        </v-row>
        <v-row justify="center" align="center">
          <v-col sm="12" md="6" lg="6" v-if="submittedCopy.length">
            <v-divider class="mb-4"></v-divider>
            <v-card
              v-for="(item, index) in submittedCopy"
              :key="index"
              class="mb-3"
              flat
              outlined
            >
              <div v-text="item" class="py-4 px-4" />
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { copy } from "@/mock-data";
import ContentEditor from "@/components/content-editor/content-editor.vue";
export default {
  name: "App",
  data: () => ({
    editMode: false,
    initialCopy: "",
    updatedCopy: "",
    submittedCopy: []
  }),
  components: {
    ContentEditor
  },
  computed: {
    toggleText() {
      return this.editMode ? "Save" : "Edit";
    }
  },
  methods: {
    handleUpdate(copy) {
      this.updatedCopy = copy;
    },
    handleToggle() {
      if (!this.editMode) {
        this.editMode = true;
      } else {
        if (this.updatedCopy.length) {
          this.submittedCopy.push(this.updatedCopy)
          this.initialCopy = this.updatedCopy
        } 
        this.editMode = false
      }
    },
    handleCancel() {
      this.updatedCopy = "";
      this.editMode = false;
    }
  },
  created() {
    this.initialCopy = copy;
  }
};
</script>
