<script>
  import { Label, Input, Spinner  } from 'flowbite-svelte';
  import {appStatusInfo} from '@/store'
  const {id, url, pages} = $appStatusInfo;

  let answer = '';
  let loading = false;

  const numOfImagesToShow = Math.min(pages, 2);
  const images = Array.from({length: numOfImagesToShow}, (_, i) => {
    const page = i + 1;
    url
    .replace('/upload/', `/upload/w_400,h_540,c_fill,pg_${page}/`)
    .replace('.pdf', '.png')
  });

  const handleSubmit = async(event) => {
    event.preventDefault();
    loading = true;

    const question = event.target.question.value;

    const res = await fetch('/api/ask', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        id,
        question,
      })
    })

  if (!res.ok) {
    loading = false;
    console.error('Error al enviar la pregunta')
    return
  }

  const {answer: apiAnswer} = await res.json();
  answer = apiAnswer;
  loading = false;
}

</script>

<div class="grid grid-cols-4 gap-2">
  {#each images as image}
  <img class="rounded w-full h-auto aspect-[400/540]" src={image} alt="PDF page" />
{/each}

</div>
  

<form class="mt-8" on:submit={handleSubmit}>
    <Label for="question" class="block mb-2">Deja aqui tu pregunta</Label>
    <Input id="question" type="text" required placeholder="¿De que trata este documento?" />
</form>

{#if loading}
  <div class="flex justify-center items-center flex-col gap-y-2">
    <Spinner class="mt-4" />
    <p class="text-black/70 text-pretty">Esperando respuesta...</p> 
  </div>
{/if}

{#if answer}
  <div class="mt-8">
    <p class="font-medium">Respuesta:</p>
    <p>{answer}</p>
  </div>
{/if}