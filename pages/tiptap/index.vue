<template>
  <div>
    <div class="editor">
      <editor-menu-bar :editor="editor" v-slot="{ commands, isActive }">
        <div class="menubar">
          <button
            :class="{ 'is-active': isActive.bold() }"
            @click="commands.bold"
            class="menubar__button"
          >
            bold
          </button>

          <button
            :class="{ 'is-active': isActive.italic() }"
            @click="commands.italic"
            class="menubar__button"
          >
            italic
          </button>

          <button
            :class="{ 'is-active': isActive.strike() }"
            @click="commands.strike"
            class="menubar__button"
          >
            strike
          </button>

          <button
            :class="{ 'is-active': isActive.underline() }"
            @click="commands.underline"
            class="menubar__button"
          >
            underline
          </button>

          <button
            :class="{ 'is-active': isActive.code() }"
            @click="commands.code"
            class="menubar__button"
          >
            code
          </button>

          <button
            :class="{ 'is-active': isActive.paragraph() }"
            @click="commands.paragraph"
            class="menubar__button"
          >
            paragraph
          </button>

          <button
            :class="{ 'is-active': isActive.heading({ level: 1 }) }"
            @click="commands.heading({ level: 1 })"
            class="menubar__button"
          >
            H1
          </button>

          <button
            :class="{ 'is-active': isActive.heading({ level: 2 }) }"
            @click="commands.heading({ level: 2 })"
            class="menubar__button"
          >
            H2
          </button>

          <button
            :class="{ 'is-active': isActive.heading({ level: 3 }) }"
            @click="commands.heading({ level: 3 })"
            class="menubar__button"
          >
            H3
          </button>

          <button
            :class="{ 'is-active': isActive.bullet_list() }"
            @click="commands.bullet_list"
            class="menubar__button"
          >
            bullet_list
          </button>

          <button
            :class="{ 'is-active': isActive.ordered_list() }"
            @click="commands.ordered_list"
            class="menubar__button"
          >
            ordered_list
          </button>

          <button
            :class="{ 'is-active': isActive.blockquote() }"
            @click="commands.blockquote"
            class="menubar__button"
          >
            blockquote
          </button>

          <button
            :class="{ 'is-active': isActive.code_block() }"
            @click="commands.code_block"
            class="menubar__button"
          >
            code_block
          </button>

          <button
            @click="commands.horizontal_rule"
            class="menubar__button"
          >
            horizontal_rule
          </button>

          <button
            @click="commands.undo"
            class="menubar__button"
          >
            undo
          </button>

          <button
            @click="commands.redo"
            class="menubar__button"
          >
            redo
          </button>
        </div>
      </editor-menu-bar>

      <editor-content :editor="editor" class="editor__content" />
    </div>

    <div class="actions">
      <button @click="clearContent" class="button">
        Clear Content
      </button>
      <button @click="setContent" class="button">
        Set Content
      </button>
    </div>

    <div class="export">
      <h3>JSON</h3>
      <pre><code v-html="json" /></pre>

      <h3>HTML</h3>
      <pre><code>{{ html }}</code></pre>
    </div>
  </div>
</template>

<script>
import { Editor, EditorContent, EditorMenuBar } from 'tiptap'
import {
  Blockquote,
  CodeBlock,
  HardBreak,
  Heading,
  HorizontalRule,
  OrderedList,
  BulletList,
  ListItem,
  TodoItem,
  TodoList,
  Bold,
  Code,
  Italic,
  Link,
  Strike,
  Underline,
  History
} from 'tiptap-extensions'

export default {
  components: {
    EditorContent,
    EditorMenuBar
  },
  data () {
    return {
      editor: null,
      json: 'Update content to see changes',
      html: 'Update content to see changes'
    }
  },
  mounted () {
    this.editor = new Editor({
      extensions: [
        new Blockquote(),
        new BulletList(),
        new CodeBlock(),
        new HardBreak(),
        new Heading({ levels: [1, 2, 3] }),
        new HorizontalRule(),
        new ListItem(),
        new OrderedList(),
        new TodoItem(),
        new TodoList(),
        new Link(),
        new Bold(),
        new Code(),
        new Italic(),
        new Strike(),
        new Underline(),
        new History()
      ],
      content: `
        <h2>
          Export HTML or JSON
        </h2>
        <p>
          You are able to export your data as <code>HTML</code> or <code>JSON</code>.
        </p>
        <button>btn</button>
      `,
      onUpdate: ({ getJSON, getHTML }) => {
        this.json = getJSON()
        this.html = getHTML()
      }
    })
  },
  methods: {
    clearContent () {
      this.editor.clearContent(true)
      this.editor.focus()
    },
    setContent () {
      // you can pass a json document
      this.editor.setContent({
        type: 'doc',
        content: [{
          type: 'paragraph',
          content: [
            {
              type: 'text',
              text: 'This is some inserted text. 👋'
            }
          ]
        }]
      }, true)
      // HTML string is also supported
      // this.editor.setContent('<p>This is some inserted text. 👋</p>')
      this.editor.focus()
    }
  }
}
</script>

<style lang="scss" scoped>
.actions {
  max-width: 30rem;
  margin: 0 auto 2rem auto;
}
.export {
  max-width: 30rem;
  margin: 0 auto 2rem auto;
  pre {
    padding: 1rem;
    border-radius: 5px;
    font-size: 0.8rem;
    font-weight: bold;
    background: rgba(black, 0.05);
    color: rgba(black, 0.8);
  }
  code {
    display: block;
    white-space: pre-wrap;
  }
}
</style>
