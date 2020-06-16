<template>
  <div class="content-editor">
    <h1 v-if="sectionTitle" v-text="sectionTitle" />
    <label v-if="label" class="bx--label" v-text="label" />
    <div v-if="showControlBar" class="control-bar">
      <editor-menu-bar v-slot="{ commands, isActive }" :editor="editor">
        <!-- <div class="menubar">
          <div class="toolbar">
            <span>
              <button class="menubar__button" @click="commands.undo">
                v-icon
              </button>
              <button class="menubar__button" @click="commands.redo">
                <Redo16 />
              </button>
            </span>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.bold() }"
              @click="commands.bold"
            >
              <TextBold16 />
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.italic() }"
              @click="commands.italic"
            >
              <TextItalic16 />
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.underline() }"
              @click="commands.underline"
            >
              <TextUnderline16 />
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.link() }"
              @click="commands.link"
            >
              <Link16 />
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.paragraph() }"
              @click="commands.paragraph"
            >
              <Paragraph16 />
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.heading({ level: 3 }) }"
              @click="commands.heading({ level: 3 })"
            >
              <h4><strong>H</strong></h4>
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.bullet_list() }"
              @click="commands.bullet_list"
            >
              <ListBulleted16 />
            </button>
            <button
              class="menubar__button"
              :class="{ 'is-active': isActive.ordered_list() }"
              @click="commands.ordered_list"
            >
              <ListNumbered16 />
            </button>
          </div>
        </div> -->
        <v-toolbar dense flat outlined>

          <v-btn icon>
            <v-icon>mdi-undo</v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon>mdi-redo</v-icon>
          </v-btn>

          <v-btn icon>
            <v-icon>mdi-format-bold</v-icon>
          </v-btn>

          <v-btn icon>
            <v-icon>mdi-format-italic</v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon>mdi-format-underline</v-icon>
          </v-btn>
        </v-toolbar>
      </editor-menu-bar>
    </div>
    <editor-content :editor="editor" />
  </div>
</template>
<script>
import { Editor, EditorContent, EditorMenuBar } from "tiptap";
import {
  Heading,
  OrderedList,
  BulletList,
  ListItem,
  Bold,
  Italic,
  Link,
  Underline,
  History
} from "tiptap-extensions";

export default {
  name: "content-editor",
  components: {
    EditorContent,
    EditorMenuBar
  },
  props: {
    showControlBar: {
      type: Boolean,
      required: false
    },
    content: {
      type: String,
      required: true
    },
    label: {
      type: String,
      required: false
    },
    placeholder: {
      type: String,
      default: "Start typing...",
      required: false
    }
  },
  data() {
    return {
      updatedContent: "",
      editor: new Editor({
        content: this.content || this.placeholder,
        extensions: [
          new BulletList(),
          new Heading({ levels: [3] }),
          new ListItem(),
          new OrderedList(),
          new Link(),
          new Bold(),
          new Italic(),
          new Underline(),
          new History()
        ],
        onUpdate: ({ getHTML }) => {
          this.updatedContent = getHTML();
          /**
           * @todo need to debounce
           */
          this.$emit("content:updated", this.updatedContent);
        }
      })
    };
  },
  mounted() {
    this.$emit("editor:mounted");
  },
  beforeDestroy() {
    // Always destroy your editor instance when it's no longer needed
    this.editor.destroy();
  },
  destroyed() {
    this.$emit("editor:destroyed");
  }
};
</script>
<style lang="scss">
.content-editor {
  .ProseMirror {
    padding: 1rem;
  }
  .toolbar {
    display: flex;
    justify-content: space-between;
    width: 100%;

    button {
      border: 0;
      cursor: pointer;
      flex-basis: auto;
      font-size: 1rem;
      margin: 0;

      &:hover {
        // background-color: $ui-02;
      }
      h3 {
        line-height: 1;
        margin: 0;
        padding: 0;
        u {
          text-decoration: none;
        }
      }
      svg {
        vertical-align: middle;
      }
    }
  }
}
</style>
