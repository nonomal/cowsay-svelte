<script lang="ts">
  import { DropdownMenu } from "bits-ui";

  import { say } from "cowsay2";
  import { COW_FILES, COW_TEXT_WRAP } from "../data";
  import type { Cow } from "../data";
  import { onMount } from "svelte";
  import { Copy, Image, ChevronDown, Text, Code } from "lucide-svelte";
  import { flyAndScale } from "../utils";

  export let canvas: HTMLCanvasElement, text: string, cow: Cow;

  let copyImageAvailable: boolean = false;

  const copyText = async (markdown: boolean = false) => {
    let cowSay = say(text, { W: COW_TEXT_WRAP, cow: COW_FILES[cow] });
    try {
      await navigator.clipboard.writeText(
        markdown ? "```\n" + cowSay + "```\n" : cowSay,
      );
    } catch (exception) {
      console.log("Could not copy! You must be using an old browser. Sorry!");
    }
  };

  const copyImage = () => {
    canvas.toBlob(async (blob) => {
      try {
        await navigator.clipboard.write([
          new ClipboardItem({ "image/png": blob as Blob }),
        ]);
      } catch (exception) {
        console.log(exception);
        console.log("Could not copy! You must be using an old browser. Sorry!");
      }
    });
  };

  onMount(() => {
    copyImageAvailable = !!window.ClipboardItem;
  });
</script>

<DropdownMenu.Root>
  <DropdownMenu.Trigger
    class="text-white px-4 py-2 gap-2 rounded-md inline-flex items-center text-sm font-medium shadow-md bg-black opacity-50 hover:opacity-60 focus:outline focus:outline-1 focus:outline-red-500"
  >
    <Copy class="size-4" />
    <span>Copy</span>
    <ChevronDown class="size-4" />
  </DropdownMenu.Trigger>

  <DropdownMenu.Content
    transition={flyAndScale}
    class="w-56 mt-1 origin-top-right bg-white divide-y divide-gray-100 p-1 rounded-md shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
  >
    <DropdownMenu.Item
      class="hover:bg-rose-500 hover:text-white text-gray-900 group flex rounded-md items-center w-full px-2 py-2 text-sm"
      on:click={copyImage}
    >
      <Image
        class="size-5 mr-2 group-hover:bg-rose-500 group-hover:text-white text-gray-900"
      />
      <span>Image</span>
    </DropdownMenu.Item>

    <DropdownMenu.Item
      class="hover:bg-rose-500 hover:text-white text-gray-900 group flex rounded-md items-center w-full px-2 py-2 text-sm"
      on:click={() => copyText()}
    >
      <Text
        class="size-5 mr-2 group-hover:bg-rose-500 group-hover:text-white text-gray-900"
      />
      <span>Text</span>
    </DropdownMenu.Item>

    <DropdownMenu.Item
      class="hover:bg-rose-500 hover:text-white text-gray-900 group flex rounded-md items-center w-full px-2 py-2 text-sm"
      on:click={() => copyText(true)}
    >
      <Code
        class="size-5 mr-2 group-hover:bg-rose-500 group-hover:text-white text-gray-900"
      />
      <span>Markdown</span>
    </DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu.Root>

<!-- <Menu class="relative inline-block text-left">
  <div>
    <MenuButton
      class="text-white px-4 py-2 rounded-md inline-flex items-center text-sm font-medium shadow-md bg-black opacity-50 hover:opacity-60 focus:outline focus:outline-1 focus:outline-red-500"
    >
      <span>Copy</span>
    </MenuButton>
  </div>
  <Transition
    as="div"
    enter="transition ease-out duration-100"
    enterFrom="transform opacity-0 scale-95"
    enterTo="transform opacity-100 scale-100"
    leave="transition ease-in duration-75"
    leaveFrom="transform opacity-100 scale-100"
    leaveTo="transform opacity-0 scale-95"
  >
    <MenuItems
      class="absolute right-0 w-56 mt-1 origin-top-right bg-white divide-y divide-gray-100 p-1 rounded-md shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
    >
      {#if copyImageAvailable}
        <MenuItem let:active>
          <button
            class={`${
              active ? "bg-rose-500 text-white" : "text-gray-900"
            } group flex rounded-md items-center w-full px-2 py-2 text-sm`}
            on:click={copyImage}
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class={`h-5 w-5 mr-2 ${
                active ? "text-rose-100" : "text-rose-500"
              }`}
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
              />
            </svg>
            Image
          </button>
        </MenuItem>
      {/if}
      <MenuItem let:active>
        <button
          class={`${
            active ? "bg-rose-500 text-white" : "text-gray-900"
          } group flex rounded-md items-center w-full px-2 py-2 text-sm`}
          on:click={() => copyText(false)}
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class={`h-5 w-5 mr-2 ${active ? "text-rose-100" : "text-rose-500"}`}
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"
            />
          </svg>
          Text
        </button>
      </MenuItem>

      <MenuItem let:active>
        <button
          class={`${
            active ? "bg-rose-500 text-white" : "text-gray-900"
          } group flex rounded-md items-center w-full px-2 py-2 text-sm`}
          on:click={() => copyText(true)}
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class={`h-5 w-5 mr-2 ${active ? "text-rose-100" : "text-rose-500"}`}
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"
            />
          </svg>
          Markdown text
        </button>
      </MenuItem>
    </MenuItems>
  </Transition>
</Menu> -->
