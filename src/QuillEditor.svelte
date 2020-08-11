<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  import Quill from "quill";

  export let initValue;
  export let toolbarOptions = [
    [{ header: 1 }, { header: 2 }, "blockquote", "link", "image"],
    ["bold", "italic", "underline", "strike"],
    [{ list: "ordered" }, { list: "ordered" }],
    [{ align: [] }],
    ["clean"],
  ];

  let quill = null;
  let editor;

  onMount(() => {
    quill = new Quill(editor, {
      modules: {
        toolbar: toolbarOptions,
      },
      theme: "snow",
      placeholder: "Write your story...",
    });

    if (initValue) {
      const delta = quill.clipboard.convert(initValue);
      quill.setContents(delta, "silent");
    }
  });

  const dispatch = createEventDispatcher();

  let handleInput = (e) => {
    dispatch("contentChange", e.target.innerHTML);
  };
</script>

<style>
  @import "https://cdn.quilljs.com/1.3.6/quill.snow.css";
</style>

<div>
  <div bind:this={editor} on:input={handleInput} />
</div>
