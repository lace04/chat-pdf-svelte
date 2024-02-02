<script>
  import {Alert} from 'flowbite-svelte'
  import {APP_STATUS, appStatus} from '@/store'
  import StepUpload from './StepUpload.svelte';
  import StepLoading from './StepLoading.svelte';
  import StepChat from './StepChat.svelte';
</script>

{ #if $appStatus === APP_STATUS.INIT }
  <StepUpload />
{ :else if $appStatus === APP_STATUS.LOADING }
  <StepLoading />
{ :else if $appStatus === APP_STATUS.ERROR }
<Alert type="success" dismissible>
  <strong>¡Algo malo ha pasado!</strong> ¡Por favor, inténtalo de nuevo!
</Alert>
{ :else if $appStatus === APP_STATUS.CHAT_MODE }
  <StepChat />
{ :else }
<Alert type="success" dismissible>
  <strong>¡Algo malo ha pasado!</strong> Estado de la aplicación desconocido.
</Alert>

{ /if}
