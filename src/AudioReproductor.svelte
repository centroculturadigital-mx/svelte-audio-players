<script>

	import { onMount, onDestroy } from 'svelte';

    import AudioReproductorVista from "./AudioReproductorVista.svelte"

    export let audio;

    let audioHTML
    
    let cargando = true;

    let actualizacion;
    let tiempo;
    let progreso 


    $: tocando = !! actualizacion
    $: tiempo = !! audioHTML ? formatearDuracion(audioHTML.currentTime) : 0
    $: duracion = !! audioHTML ? formatearDuracion(audioHTML.duration) : 0
    $: progreso = !! audioHTML ? calcularProgreso(audioHTML.currentTime,audioHTML.duration) : 0
    


    const calcularProgreso = (tiempo,duracion) => (tiempo / duracion)*100

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

    

    onMount(() => {
    
        
        audioHTML.addEventListener('canplay',()=>{
          actualizar()
          setTimeout(()=>{
            // suavizar cargado
            cargando = false      
          }, 500)
        });
        
    
    })

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
            tocar,
            cargando
        }
    }
/>
