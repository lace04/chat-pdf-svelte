<script>
  import {Alert} from 'flowbite-svelte'
  import {appStatus, setAppStatusLoading} from '@/store'

  import Dropzone from "svelte-file-dropzone";

  let files = {
    accepted: [],
    rejected: []
  };

  function handleFilesSelect(e) {
    const { acceptedFiles, fileRejections } = e.detail;
    files.accepted = [...files.accepted, ...acceptedFiles];
    files.rejected = [...files.rejected, ...fileRejections];

    if(acceptedFiles.length > 0) {
      setAppStatusLoading();
    }
  }
</script>

<Dropzone on:drop={handleFilesSelect} multiple={false} accept={"application/pdf"}>
    <p class="text-black/70 text-pretty">Arrastra y suelta tus archivos PDF aquí</p>
</Dropzone>
<ol>
  {#each files.accepted as item}
    <li>{item.name}</li>
  {/each}
</ol>