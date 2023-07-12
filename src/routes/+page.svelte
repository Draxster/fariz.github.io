<script lang="ts">
	import Body from "$lib/Body.svelte";
	import Footer from "$lib/Footer.svelte";
    import Comic from "$lib/Comic.svelte";

    import moment from 'moment';
    import { onMount } from 'svelte';

const email: string = "f.rakhmatov@innopolis.university";
onMount(() => {
    if (typeof(document) !== "undefined") {
        async function getXkcdId(): Promise<number> {
        const params: URLSearchParams = new URLSearchParams();
        if (email) {
            params.append('email', email);
        }
        const response = await fetch('https://fwd.innopolis.university/api/hw2?email=' + params.toString());
        return response.json();
}
    }
})

onMount(() => {
      if (typeof document !== 'undefined') { 
        async function getXkcdComic(id: number): Promise<Comic> {
            const url: string = 'https://fwd.innopolis.university/api/comic?id=' + id.toString();
            const response = await fetch(url);
            const data = await response.json();
            return data;
}
      }
  });


async function displayComic(): Promise<void> {
  const id: number = await getXkcdId();
  const comic: Comic = await getXkcdComic(id);

  const container: HTMLElement | null = document.getElementById('comic-container');

  if (container) {
    const title: HTMLTitleElement = document.createElement('title');
    title.textContent = comic.safe_title;
    container.appendChild(title);

    const image: HTMLImageElement = document.createElement('img');
    image.src = comic.img;
    image.alt = comic.alt;

    const date: HTMLParagraphElement = document.createElement('p');
    date.textContent = moment(`${comic.year}-${comic.month}-${comic.day}`).fromNow();
    container.appendChild(date);
  }
}

interface Comic {
    "safe_title": string;
    "img": string;
    "alt": string;
    "year": number;
    "month": number;
    "day": number;
}


	function getXkcdId(): number | PromiseLike<number> {
		throw new Error("Function not implemented.");
	}


	function getXkcdComic(id: number): Comic | PromiseLike<Comic> {
		throw new Error("Function not implemented.");
	}
</script>

<body>
   <Body/>
   <Footer/>
</body>

<footer>
    
</footer>