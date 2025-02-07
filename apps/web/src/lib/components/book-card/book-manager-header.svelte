<script lang="ts">
  import { faBug, faCog, faTimes, faTrash, faExpand } from '@fortawesome/free-solid-svg-icons';
  import { createEventDispatcher } from 'svelte';
  import { quintOut } from 'svelte/easing';
  import { scale } from 'svelte/transition';
  import Fa from 'svelte-fa';
  import {
    nTranslateXHeaderFa,
    opacityHeaderIcon,
    pHeaderFa,
    pxScreen,
    translateXHeaderFa
  } from '$lib/css-classes';
  import { inputAllowDirectory } from '$lib/functions/file-dom/input-allow-directory';
  import { inputFile } from '$lib/functions/file-dom/input-file';
  import { fullscreenManager } from '$lib/data/fullscreen-manager';


  export let hasBookOpened: boolean;
  export let selectMode: boolean;
  export let selectedCount: number;
  export let isImporting: boolean;

  const dispatch = createEventDispatcher<{
    selectAllClick: void;
    removeClick: void;
    bugReportClick: void;
    backToBookClick: void;
    fullscreenClick: void;
    filesChange: FileList;
  }>();

  const pHeaderMat = 'p-3 xl:p-2.5';
  const nTranslateXHeaderMat = '-translate-x-3 xl:-translate-x-2.5';

  function dispatchFilesChange(fileList: FileList) {
    dispatch('filesChange', fileList);
  }

  const fullscreenEnabled = fullscreenManager.fullscreenEnabled

  const inAnimationParams = {
    delay: 150,
    duration: 150,
    easing: quintOut
  };

  const outAnimationParams = {
    duration: 150,
    easing: quintOut
  };
</script>

<div class="relative h-12 bg-gray-700 text-white xl:h-10">
  <div class="flex h-full justify-between {pxScreen}">
    {#if selectedCount === 0}
      <div class="transform-gpu {nTranslateXHeaderMat}">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          on:click={() => (selectMode = !selectMode)}
          in:scale={inAnimationParams}
          out:scale={outAnimationParams}
          class:opacity-100={selectMode}
          class:opacity-60={!selectMode}
          class="h-12 w-12 xl:h-10 xl:w-10 {pHeaderMat} cursor-pointer transition-opacity hover:opacity-100"
          viewBox="0 0 24 24"
        >
          <path
            class="fill-current"
            d="M20,4v12H8V4H20 M20,2H8C6.9,2,6,2.9,6,4v12c0,1.1,0.9,2,2,2h12c1.1,0,2-0.9,2-2V4C22,2.9,21.1,2,20,2L20,2z M12.47,14 L9,10.5l1.4-1.41l2.07,2.08L17.6,6L19,7.41L12.47,14z M4,6H2v14c0,1.1,0.9,2,2,2h14v-2H4V6z"
          />
        </svg>
      </div>
    {:else}
      <div class="flex items-center h-full transform-gpu {nTranslateXHeaderFa} text-xl font-medium">
        <div
          class="flex items-center h-full text-2xl xl:text-xl {pHeaderFa} cursor-pointer"
          in:scale={inAnimationParams}
          out:scale={outAnimationParams}
          on:click={() => (selectMode = !selectMode)}
        >
          <Fa icon={faTimes} />
        </div>
        <span
          class="transform-gpu translate-x-2"
          in:scale={inAnimationParams}
          out:scale={outAnimationParams}>{selectedCount}</span
        >
      </div>
    {/if}

    <div class="absolute left-1/2 h-full -translate-x-1/2 transform-gpu">
      {#if !selectMode}
        {#if hasBookOpened}
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            in:scale={inAnimationParams}
            out:scale={outAnimationParams}
            on:click={() => dispatch('backToBookClick')}
            class="h-12 w-12 xl:h-10 xl:w-10 {pHeaderMat} {opacityHeaderIcon} cursor-pointer"
          >
            <path
              class="fill-current"
              d="M21 5c-1.11-.35-2.33-.5-3.5-.5-1.95 0-4.05.4-5.5 1.5-1.45-1.1-3.55-1.5-5.5-1.5S2.45 4.9 1 6v14.65c0 .25.25.5.5.5.1 0 .15-.05.25-.05C3.1 20.45 5.05 20 6.5 20c1.95 0 4.05.4 5.5 1.5 1.35-.85 3.8-1.5 5.5-1.5 1.65 0 3.35.3 4.75 1.05.1.05.15.05.25.05.25 0 .5-.25.5-.5V6c-.6-.45-1.25-.75-2-1zm0 13.5c-1.1-.35-2.3-.5-3.5-.5-1.7 0-4.15.65-5.5 1.5V8c1.35-.85 3.8-1.5 5.5-1.5 1.2 0 2.4.15 3.5.5v11.5zm-3.5-8c.88 0 1.73.09 2.5.26V9.24c-.79-.15-1.64-.24-2.5-.24-1.7 0-3.24.29-4.5.83v1.66c1.13-.64 2.7-.99 4.5-.99zM13 12.49v1.66c1.13-.64 2.7-.99 4.5-.99.88 0 1.73.09 2.5.26V11.9c-.79-.15-1.64-.24-2.5-.24-1.7 0-3.24.3-4.5.83zm4.5 1.84c-1.7 0-3.24.29-4.5.83v1.66c1.13-.64 2.7-.99 4.5-.99.88 0 1.73.09 2.5.26v-1.52c-.79-.16-1.64-.24-2.5-.24z"
            />
          </svg>
        {/if}
      {:else}
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          in:scale={inAnimationParams}
          out:scale={outAnimationParams}
          on:click={() => dispatch('selectAllClick')}
          class="w-12 xl:w-10 h-12 xl:h-10 {pHeaderMat} {opacityHeaderIcon} cursor-pointer"
        >
          <path
            class="fill-current"
            d="M18 7l-1.41-1.41-6.34 6.34 1.41 1.41L18 7zm4.24-1.41L11.66 16.17 7.48 12l-1.41 1.41L11.66 19l12-12-1.42-1.41zM.41 13.41L6 19l1.41-1.41L1.83 12 .41 13.41z"
          />
        </svg>
      {/if}
    </div>

    <div class="flex transform-gpu {translateXHeaderFa}">
      {#if !selectMode}
        {#if !isImporting}
          <label
            class="xl:mr-1 {opacityHeaderIcon} cursor-pointer"
            in:scale={inAnimationParams}
            out:scale={outAnimationParams}
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              class="h-12 w-12 xl:h-10 xl:w-10 {pHeaderMat}"
            >
              <path
                class="fill-current"
                d="M14 2H6c-1.1 0-1.99.9-1.99 2L4 20c0 1.1.89 2 1.99 2H18c1.1 0 2-.9 2-2V8l-6-6zm4 18H6V4h7v5h5v11zM8 15.01l1.41 1.41L11 14.84V19h2v-4.16l1.59 1.59L16 15.01 12.01 11 8 15.01z"
              />
            </svg>
            <input
              type="file"
              accept=".htmlz,.epub"
              use:inputFile={dispatchFilesChange}
              disabled={isImporting}
              multiple
              hidden
            />
          </label>
          <label
            class="xl:mr-1 {opacityHeaderIcon} cursor-pointer"
            in:scale={inAnimationParams}
            out:scale={outAnimationParams}
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              class="h-12 w-12 xl:h-10 xl:w-10 {pHeaderMat}"
            >
              <path
                class="fill-current"
                d="M20 6h-8l-2-2H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V8c0-1.1-.9-2-2-2zm0 12H4V6h5.17l2 2H20v10zM9.41 14.42L11 12.84V17h2v-4.16l1.59 1.59L16 13.01 12.01 9 8 13.01l1.41 1.41z"
              />
            </svg>
            <input
              type="file"
              use:inputFile={dispatchFilesChange}
              use:inputAllowDirectory
              multiple
              disabled={isImporting}
              hidden
            />
          </label>
        {/if}
        
        {#if {fullscreenEnabled}}
        <div
          role="button"
          on:click={() => dispatch('fullscreenClick')}
          class="flex h-full items-center text-xl xl:text-lg {pHeaderFa} {opacityHeaderIcon} cursor-pointer"
        >
          <Fa icon={faExpand} />
        </div>
      {/if}

        <a href="/settings">
          <span
            class="flex h-full items-center text-xl xl:text-lg {pHeaderFa} {opacityHeaderIcon} cursor-pointer"
            in:scale={inAnimationParams}
            out:scale={outAnimationParams}
          >
            <Fa icon={faCog} />
          </span>
        </a>
      {/if}

      {#if selectedCount > 0}
        <div
          class="flex h-full items-center text-xl xl:text-lg {pHeaderFa} {opacityHeaderIcon} cursor-pointer"
          in:scale={inAnimationParams}
          out:scale={outAnimationParams}
          on:click={() => dispatch('removeClick')}
        >
          <Fa icon={faTrash} />
        </div>
      {/if}

      <div
        class="flex h-full items-center text-xl xl:text-lg {pHeaderFa} {opacityHeaderIcon} cursor-pointer"
        in:scale={inAnimationParams}
        out:scale={outAnimationParams}
        on:click={() => dispatch('bugReportClick')}
      >
        <Fa icon={faBug} />
      </div>
    </div>
  </div>
</div>
