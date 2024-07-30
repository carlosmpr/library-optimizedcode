---
  title: 'SvelteTestFile.svelte'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos Polanco'
  ---
  
  
  
  # SvelteTestFile.svelte
  ## Code Explanation

### Libraries/Dependencies
- The code uses the `svelte` framework for creating web applications.
- The code relies on the `createEventDispatcher` function from the `svelte` library to dispatch events.

### Variables
- `allowedExtensions`: An array containing a list of file extensions that are allowed to be uploaded.
- `maxFiles`: Maximum number of files that can be uploaded.
- `selectedFiles`: An array to store the files that have been selected for upload.
- `dispatch`: A function created using `createEventDispatcher` to dispatch events.

### Functions
1. `handleDragOver(event)`: Prevents the default behavior when a file is dragged over an element.
2. `handleDrop(event)`: Handles the drop event when files are dropped onto the designated area. It extracts the files and validates them.
3. `extractFiles(items)`: Extracts files from the dropped items and validates them against allowed extensions.
4. `traverseFileTree(item, filesArray)`: Recursively traverses a file tree to extract files from directories.
5. `isValidFile(file)`: Checks if a file has a valid extension based on the `allowedExtensions` array.
6. `handleFileSelect(event)`: Handles the file selection event when files are selected using the file input.
7. `validateAndSetFiles(files)`: Validates the selected files against allowed extensions and maximum file limit.

### Event Handlers
- `on:dragover|preventDefault={handleDragOver}`: Calls `handleDragOver` function when a dragover event occurs and prevents the default behavior.
- `on:drop|preventDefault={handleDrop}`: Calls `handleDrop` function when a drop event occurs and prevents the default behavior.
- `on:change={handleFileSelect}`: Calls `handleFileSelect` function when files are selected using the file input.

### Example Usage
```html
<div>
  <!-- File upload area -->
</div>
```

This code snippet provides functionality for drag-and-drop file upload with validation for allowed file types and maximum file count. It also allows selecting files using a file input. The selected files are displayed with their names, and error messages are dispatched if validation fails.
  
  ## Component Code
  ```jsx
  <script>
  import { createEventDispatcher } from 'svelte';

  const allowedExtensions = [
    ".js", ".jsx", ".ts", ".tsx", ".py", ".java", ".rb", ".php",
    ".html", ".css", ".cpp", ".c", ".go", ".rs", ".swift", ".kt",
    ".m", ".h", ".cs", ".json", ".xml", ".sh", ".yml", ".yaml",
    ".vue", ".svelte", ".qwik"
  ];
  const maxFiles = 4;

  let selectedFiles = [];
  const dispatch = createEventDispatcher();

  function handleDragOver(event) {
    event.preventDefault();
  }

  async function handleDrop(event) {
    event.preventDefault();
    const items = event.dataTransfer.items;
    const filesArray = await extractFiles(items);
    validateAndSetFiles(filesArray);
  }

  async function extractFiles(items) {
    const filesArray = [];
    for (let i = 0; i < items.length; i++) {
      const item = items[i].webkitGetAsEntry();
      if (item.isFile) {
        const file = await new Promise(resolve => item.file(resolve));
        if (isValidFile(file)) {
          filesArray.push(file);
        } else {
          dispatch('setError', `Invalid file type. Only ${allowedExtensions.join(", ")} files are allowed.`);
        }
      } else if (item.isDirectory) {
        await traverseFileTree(item, filesArray);
      }
    }
    return filesArray;
  }

  function traverseFileTree(item, filesArray) {
    return new Promise(resolve => {
      if (item.isFile) {
        item.file(file => {
          if (isValidFile(file)) {
            filesArray.push(file);
          }
          resolve();
        });
      } else if (item.isDirectory) {
        const dirReader = item.createReader();
        dirReader.readEntries(async entries => {
          for (const entry of entries) {
            await traverseFileTree(entry, filesArray);
          }
          resolve();
        });
      }
    });
  }

  function isValidFile(file) {
    return allowedExtensions.some(ext => file.name.endsWith(ext));
  }

  function handleFileSelect(event) {
    const files = Array.from(event.target.files);
    validateAndSetFiles(files);
  }

  function validateAndSetFiles(files) {
    const filteredFiles = files.filter(file => isValidFile(file));
    if (filteredFiles.length + selectedFiles.length > maxFiles) {
      dispatch('setError', `You can only upload a maximum of ${maxFiles} files.`);
    } else {
      selectedFiles = [...selectedFiles, ...filteredFiles].slice(0, maxFiles);
      dispatch('setError', "");
    }
  }
</script>

<div
  on:dragover|preventDefault={handleDragOver}
  on:drop|preventDefault={handleDrop}
  class="p-4 border-2 border-dashed border-gray-300 rounded-md text-center cursor-pointer mb-4 h-96 w-96 flex overflow-y-scroll items-center justify-center"
>
  <input
    type="file"
    on:change={handleFileSelect}
    style="display: none;"
    accept={allowedExtensions.join(",")}
    id="fileUpload"
    multiple
  />
  <label for="fileUpload" class="cursor-pointer">
    {#if selectedFiles.length > 0}
      <div class="flex flex-wrap gap-10">
        {#each selectedFiles as file}
          <div>
            <DocumentTextIcon class="mx-auto w-8" />
            <p>{file.name}</p>
          </div>
        {/each}
      </div>
    {:else}
      <div>
        <ArrowUpTrayIcon class="mx-auto w-8" />
        <p>Drag & drop files or folders here, or click to select files</p>
      </div>
    {/if}
  </label>
</div>

<style>
/* Add any component-specific styles here */
</style>
  ```
  