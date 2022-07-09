<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  import Quill from "quill";
// 	import { v4 } from 'uuid';

	export let classN;
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
		
		quill.on('text-change', (delta, oldDelta, source) => {
			console.log("Text Changed");
		});
		
  });

  const dispatch = createEventDispatcher();

  let handleInput = (e) => {
    dispatch("contentChange", e.target.innerHTML);
  };
	
	let handleAnyChange = (e) => {
    dispatch(
      "contentChange",
      document.querySelector(`.${classN} .ql-editor`).innerHTML
    );
  };
</script>

<div on:click={handleAnyChange}>
  <div class="q-cont">
    <div
      class={`${classN} q-inner`}
      bind:this={editor}
      on:input={handleAnyChange}
    />
  </div>
</div>

<style>
  @import "https://cdn.quilljs.com/1.3.6/quill.snow.css";
	
  .q-cont {
    /* border-top-left-radius: 0.75rem !important;
    border-top-right-radius: 0.75rem !important; */
    overflow: hidden !important;
    border-radius: 0.75rem !important;
  }

  .q-inner {
    min-height: 7.5rem !important;
    /* margin-bottom: 2rem !important; */
    /* border-bottom-left-radius: 0.75rem !important; */
    /* border-bottom-right-radius: 0.75rem !important; */
    overflow: hidden !important;
  }
</style>
