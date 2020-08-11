<script>
  import QuillEditor from "./QuillEditor.svelte";

  let initValue = `<p>This is the initial content</p>`;

  let handleContentChange = (e) => {
    console.log(e.detail);
  };
</script>

<div>
  <QuillEditor {initValue} on:contentChange={handleContentChange} />
</div>
