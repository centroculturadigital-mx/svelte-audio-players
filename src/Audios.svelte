<script>
	import AudioReproductor from './AudioReproductor.svelte';

    // import audio from "./data/ambiente-interior.ogg"
    const audios = [
        {
            nombre: "Qui adipisicing veniam anim labore culpa fugiat officia eu deserunt adipisicing consectetur duis.",
            archivo: "/data/ambiente-interior.ogg"
        },
        {
            nombre: "Deserunt reprehenderit ullamco quis aliqua nisi occaecat aute dolor ipsum excepteur tempor sint.",
            archivo: "/data/ambiente-interior.ogg"
        },
        {
            nombre: "Ex ut amet veniam laborum anim id.",
            archivo: "/data/ambiente-interior.ogg"
        },
        {
            nombre: "Laborum ut nisi eu dolore exercitation veniam ipsum.",
            archivo: "/data/ambiente-interior.ogg"
        },
        {
            nombre: "Exercitation sunt enim consectetur irure aliquip velit veniam duis.",
            archivo: "/data/ambiente-interior.ogg"
        },
        {
            nombre: "Esse voluptate culpa dolor non do qui adipisicing Lorem qui voluptate ex sit nulla.",
            archivo: "/data/ambiente-interior.ogg"
        },
        {
            nombre: "Lorem velit nisi anim exercitation elit officia.",
            archivo: "/data/ambiente-interior.ogg"
        },
    ]



</script>


{#each audios as audio ('audio_'+Math.random()) }
    
    <AudioReproductor audio={audio}/>

{/each}
