<script>

import { fade } from 'svelte/transition';

export let tocando;
export let parar;
export let tocar;
export let tiempo;
export let duracion;
export let progreso;
export let audio;
export let cargando;

</script>

<style>

    .AudioReproductor {
        border: 1px solid #000;
        position: relative;
        min-height: 6rem;
    }

    input[type="range"] {
        width: 100%;
    }

    .AudioReproductor > .Cargando {
        width: 100%;
        height: 6rem;
        position: absolute;
        top: 0;
        left: 0;
        display: flex;
        align-items: center;
        justify-content: center;
    }


</style>

<article class="AudioReproductor">

    {#if ! cargando }
        <header transition:fade>
            {#if tocando}
                <button on:click={()=>parar()}>Parar</button>
            {:else}
                <button on:click={()=>tocar()}>Tocar</button>
            {/if}

            <div class="Tiempo">
                <span class="Posicion">{tiempo}</span>
                <span class="Duracion">{duracion}</span>
                <input type="range" class="Progreso" value={progreso}/>
            </div>
        </header>
        <section class="Informacion">
            <span>
                { audio.nombre }
            </span>
        </section>
    {:else}
        <div class="Cargando" transition:fade>
            Cargando...
        </div>
    {/if}

    
</article>