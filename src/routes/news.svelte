<script>
  let newsObject;

function dropdown(e) {
let arrow;
let content;

if (e.target.classList.contains("arrow")) {
  arrow = e.target;
  content = e.target.parentElement.parentElement.nextElementSibling;
} else if (!e.target.classList.contains("arrow")) {
  arrow = e.target.children[0].children[0];
  content = e.target.nextElementSibling;
}

if (!content.classList.contains("flex")) {
  content.className = " flex flex-col pb-3 px-2";
  arrow.className = "rotate-180 arrow";
} else if (content.classList.contains("flex")) {
  content.className = "hidden";
  arrow.className = "rotate-360 arrow";
}
}

async function fetchData() {
const response = await fetch(
  "https://newsapi.org/v2/everything?q=tesla&from=2023-05-14&sortBy=publishedAt&apiKey=acd0b902a33d442c8dc12604aa9b8eb6"
);
newsObject = await response.json();
newsObject = await newsObject;
}


fetchData();
</script>

<main class="flex justify-center items-center w-[1000px] mx-auto overflow-x-hidden my-6 ">
<div class="flex flex-col gap-y-3 w-[100%] items-center">
{#if !newsObject}
  <h1 class="text-center">Loading...</h1>
{:else}
  {#each newsObject.articles
     as news}
    <div class="newsDropdownable w-[100%]">
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <h1
        on:click={dropdown}
        class="relative font-sans h-14  flex items-center px-2 hover:bg-gray-300"
      >
        {news.title}<span class="absolute right-2 h-8 w-8"
          ><img
            src="/public/images/arrow.png"
            class="arrow"
            alt="arrow"
          /></span
        >
      </h1>
      <div class="hidden">
        <p class="text-xs/6 font-bold">by {news.creator}</p>
        <p class="text-justify">{news.content}</p>
      </div>
    </div>
  {/each}
{/if}
</div>
</main>

<style lang="postcss">
.newsDropdownable {
@apply w-full rounded-md bg-gray-100;
}
</style>