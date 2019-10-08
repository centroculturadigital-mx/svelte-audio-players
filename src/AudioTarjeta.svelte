<script>
	import { onMount, onDestroy } from 'svelte';

    // import audio from "./data/ambiente-interior.ogg"
    const archivosAudio = [
        "/data/ambiente-interior.ogg",
        "/data/ambiente-interior.ogg",
        "/data/ambiente-interior.ogg",
        "/data/ambiente-interior.ogg",
        "/data/ambiente-interior.ogg",
        "/data/ambiente-interior.ogg",
        "/data/ambiente-interior.ogg",
    ]

    let numAudios = archivosAudio.length
    
    let audios = []

    let tocando=new Array(numAudios).fill(false)
    let tiempo = new Array(numAudios).fill(0)
    let duracion = new Array(numAudios).fill(0)
    let progreso = new Array(numAudios).fill(0)

    let actualizacion


    const formatearDuracion = (duracion) => {
        let date = new Date(null);
        
        if(Number.isNaN(duracion)) {
            duracion = 0
        }

        date.setSeconds(duracion); // specify value for SECONDS here
        return date.toISOString().substr(14, 5);
    }



    const actualizar = ()=>{
            

        for( let indice in audios ) {
            let audio = audios[indice]

            // console.log(audio, indice);
            
            tiempo[indice]=formatearDuracion(audio.currentTime)
            
            duracion[indice]= formatearDuracion(audio.duration)
    
            progreso[indice]=( audio.currentTime/audio.duration ) * 100

        }
         

    }



    onMount(() => {

        actualizacion = setInterval(actualizar,1000) 
        
    })


    onDestroy(() => {
    
        clearInterval(actualizacion)
    
    })

    const tocar = (i) => {
        
        if( !! audios[i] ) {
            
            audios[i].play()
            tocando[i] = true;
            
        }
    }
    const parar = (i) => {
        if( !! audios[i] ) {

            audios[i].pause()
            tocando[i] = false;
        
        }
    }


</script>

<style>
.AudioTarjeta {
    border: 1px solid #000;
}

input[type="range"] {
    width: 100%;
}
</style>

{#each archivosAudio as archivoAudio, indice ('audio_'+indice) }
    <article class="AudioTarjeta">

        <header>
        {#if tocando[indice]}
            <button on:click={()=>parar(indice)}>Parar</button>
        {:else}
            <button on:click={()=>tocar(indice)}>Tocar</button>
        {/if}

        <div class="Tiempo">
            <span class="Posicion">{tiempo[indice]}</span>
            <span class="Duracion">{duracion[indice]}</span>
            <input type="range" class="Progreso" value={progreso[indice]}/>
        </div>
        </header>

        <audio bind:this={audios[indice]}>
            <source src={archivoAudio}/>
        </audio>


        <section class="Informacion">
            Informacion
        </section>
        
    </article>
{/each}
