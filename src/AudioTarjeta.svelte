<script>
	import { onMount, onDestroy } from 'svelte';

    // import audio from "./data/ambiente-interior.ogg"
    const audio = "/data/ambiente-interior.ogg"

    let audioHtml=[];
    let tocando=[false,false];
    let tiempo = [0,0]
    let duracion = [0,0]
    let progreso = [0,0]

    let actualizacion


    const formatearDuracion = (duracion) => {
        let date = new Date(null);
        date.setSeconds(duracion); // specify value for SECONDS here
        return date.toISOString().substr(14, 5);
    }



    const actualizar = ()=>{
            
        tiempo[0]=formatearDuracion(audioHtml[0].currentTime)
        
        duracion[0]= formatearDuracion(audioHtml[0].duration)

        progreso[0]=( audioHtml[0].currentTime/audioHtml[0].duration ) * 100
         
         
        tiempo[1]=formatearDuracion(audioHtml[1].currentTime)
        
        duracion[1]= formatearDuracion(audioHtml[1].duration)

        progreso[1]=( audioHtml[1].currentTime/audioHtml[1].duration ) * 100

    }



    onMount(() => {

        actualizacion = setInterval(actualizar,1000) 
        
    })


    onDestroy(() => {
    
        clearInterval(actualizacion)
    
    })

    const tocar = (i) => {
        
        if( !! audioHtml[i] ) {
            
            audioHtml[i].play()
            tocando[i] = true;
            
        }
    }
    const parar = (i) => {
        if( !! audioHtml[i] ) {

            audioHtml[i].pause()
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

<article class="AudioTarjeta">

    <header>
        {#if tocando[0]}
            <button on:click={()=>parar(0)}>Parar</button>
        {:else}
            <button on:click={()=>tocar(0)}>Tocar</button>
        {/if}
        {#if tocando[1]}
            <button on:click={()=>parar(1)}>Parar</button>
        {:else}
            <button on:click={()=>tocar(1)}>Tocar</button>
        {/if}

        <div class="Tiempo">
            <span class="Posicion">{tiempo[0]}</span>
            <span class="Duracion">{duracion[0]}</span>
            <input type="range" class="Progreso" value={progreso[0]}/>
        </div>

        <div class="Tiempo">
            <span class="Posicion">{tiempo[1]}</span>
            <span class="Duracion">{duracion[1]}</span>
            <input type="range" class="Progreso" value={progreso[1]}/>
        </div>
    </header>

    <audio bind:this={audioHtml[0]}>
        <source src={audio}/>
    </audio>

    <audio bind:this={audioHtml[1]}>
        <source src={audio}/>
    </audio>


    <section class="Informacion">
        Informacion
    </section>
    
</article>