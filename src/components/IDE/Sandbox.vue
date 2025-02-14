<script setup lang="ts">
import { Splitpanes, Pane } from 'splitpanes'
import { store } from '~/store'

const initialScriptContent = store.scriptContent.length > 0
  ? store.scriptContent
  : `
import { useMouse } from '@vueuse/core'

const { x, y } = useMouse()
`.trim()

const initialTemplateContent = store.templateContent.length > 0
  ? store.templateContent
  : `
<div class="h-screen grid place-items-center place-content-center grid-flow-col gap-4 font-mono">
  <div class="dark:bg-dark-500 bg-light-500 flex flex-col text-center p-2 rounded">
    <span class="text-4xl">{{ x }}</span>
    <span class="text-sm dark:text-light-900  dark:text-opacity-50 mt-2">Mouse X</span>
  </div>
  <div class="dark:bg-dark-500 bg-light-500 flex flex-col text-center p-2 rounded">
    <span class="text-4xl">{{ y }}</span>
    <span class="text-sm dark:text-light-900 dark:text-opacity-50 mt-2">Mouse Y</span>
  </div>
</div>
`.trim()

const onContentChanged = (source: string, content: string) => {
  if (source === 'script')
    store.scriptContent = content
  else if (source === 'template')
    store.templateContent = content
}
</script>

<template>
  <Splitpanes class="default-theme">
    <Pane>
      <div class="h-full">
        <div class="bg-dark-100 dark:bg-dark-800 text-light-200 rounded-t-md border-b dark:border-dark-900 flex flex-row items-center pr-2">
          <Tab :name="file.filename" v-for="file in store.files" :key="file.filename">
            {{ file.filename }}
          </Tab>
          <span class="flex-1" />
          <!-- <carbon-add class="text-lg dark:text-light-900" /> -->
        </div>
        <Splitpanes class="h-full default-theme" horizontal>
          <Pane>
            <Container title="Script Setup" no-overflow no-rounding>
              <Editor
                language="typescript"
                :value="initialScriptContent"
                @change="content => onContentChanged('script', content)"
              />
            </Container>
          </Pane>
          <Pane>
            <Container title="Template" no-overflow>
              <Editor
                language="html"
                :value="initialTemplateContent"
                @change="content => onContentChanged('template', content)"
              />
            </Container>
          </Pane>
        </Splitpanes>
      </div>
    </Pane>
    <Pane>
      <Splitpanes horizontal>
        <Pane>
          <Container title="Output">
            <Preview />
          </Container>
        </Pane>
        <Pane size="25">
          <Container title="Console">
            <Console />
          </Container>
        </Pane>
      </Splitpanes>
    </Pane>
  </Splitpanes>
</template>

<style>
.splitpanes.default-theme .splitpanes__pane {
  @apply bg-transparent;
}
.splitpanes.default-theme .splitpanes__splitter {
  @apply bg-transparent border-transparent min-w-4 min-h-4;
}

.splitpanes.default-theme .splitpanes__splitter::before,
.splitpanes.default-theme .splitpanes__splitter::after {
  @apply bg-dark-100 bg-opacity-50;
}

.splitpanes.default-theme .splitpanes__splitter:hover::before,
.splitpanes.default-theme .splitpanes__splitter:hover::after {
  @apply bg-light-100 bg-opacity-50;
}

.dark .splitpanes.default-theme .splitpanes__splitter::before,
.dark .splitpanes.default-theme .splitpanes__splitter::after {
  @apply bg-dark-100;
}

.dark .splitpanes.default-theme .splitpanes__splitter:hover::before,
.dark .splitpanes.default-theme .splitpanes__splitter:hover::after {
  @apply bg-dark-100;
}
</style>
