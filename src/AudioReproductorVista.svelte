<script>
    export let tocando;
    export let parar;
    export let tocar;
    export let tiempo;
    export let duracion;
    export let progreso;
    export let audio;
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
    
</article>