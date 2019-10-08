<script>

	import { onMount, onDestroy } from 'svelte';

    import AudioReproductorVista from "./AudioReproductorVista.svelte"

    export let audio;

    let audioHTML
    
    let actualizacion;
    let tiempo;
    
    export let informacion;

    $: tocando = !! actualizacion
    $: tiempo = !! audioHTML ? formatearDuracion(audioHTML.currentTime) : 0
    $: duracion = !! audioHTML ? formatearDuracion(audioHTML.currentTime / audioHTML.duration) : 0
    $: progreso = (tiempo / duracion)*100;



    const actualizar = ()=>{
        // reasignar para detonar asignacion reactiva con '$'
        audioHTML = audioHTML
    }



    const tocar = () => {

        if( ! actualizacion ) {
            actualizacion = setInterval(actualizar,1000) 
        }
    
        audioHTML.play()
    }
    
    const parar = () => {
    
        audioHTML.pause()
        clearInterval(actualizacion)
        actualizacion = false
        
    }

    

    onDestroy(() => {
    
        clearInterval(actualizacion)
    
    })

    


    const formatearDuracion = (duracion) => {
        let date = new Date(null);
        
        if(Number.isNaN(duracion)) {
            duracion = 0
        }

        date.setSeconds(duracion); // specify value for SECONDS here
        
        return date.toISOString().substr(14, 5);
    }


</script>

<audio bind:this={audioHTML}>
    <source src={audio.archivo}/>
</audio>

<AudioReproductorVista
    {
        ...{
            tocando,
            tiempo,
            duracion,
            progreso,
            audio,
            parar,
            tocar
        }
    }
/>
