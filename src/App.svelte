<script>
    import TimeSegment from './TimeSegment.svelte'
    const intervalDuration = 10
    let ms = 0;
    let isRunning = false;
    let interval = null
    $: msDisplay = (parseInt(ms % 1000) / 10).toString().padStart(2, '0')
    $: seconds = parseInt((ms / 1000) % 60)
    $: minutes = parseInt((ms / 60000) % 60)
    $: hours = parseInt((ms / 3600000) % 24)

    function handleStartStop() {
        if (!isRunning) {
            startTimer()
        } else {
            stopTimer()
        }
    }

    function startTimer() {
        isRunning = true
        interval = setInterval(() => {
            ms += intervalDuration
        }, intervalDuration)
    }

    function stopTimer() {
        isRunning = false
        clearInterval(interval)
    }

    function handleReset() {
        stopTimer()
        ms = 0;
    }
</script>

<main>
    <time>
        {#if hours > 0}
            <TimeSegment value={hours} label="h" />
        {/if}
        {#if minutes > 0 || hours > 0}
            <TimeSegment value={minutes} label="m" />
        {/if}
        <TimeSegment value={seconds} label="s" /> <span class="ms-block">{msDisplay}</span>
    </time>
    <div class="actions">
        <button on:click={handleStartStop} class="start-stop">{isRunning ? 'Stop' : 'Start'}</button>
        <button on:click={handleReset} class="reset">Reset</button>
    </div>
</main>

<style>
	main {
		text-align: center;
		padding: 1rem;
		margin: 2rem auto 0;
    }

    time {
        font-size: 4rem;
        font-variant-numeric: tabular-nums;
    }

    .actions {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 3rem;
    }
    .actions > button {
        width: 5rem;
        font-size: .825rem;
        padding: .5rem 0;
        border-radius: 2px;
        text-transform: uppercase;
        font-weight: 700;
        cursor: pointer;
        margin: 0 .5rem;
    }
    .actions .start-stop {
        background-color: #2563EB;
        color: #fff;
        border: 1px solid #1D4ED8;
    }
    .actions .start-stop:hover {
        background-color: #3B82F6;
    }
    .actions .reset {
        background: #fff;
        color: #1D4ED8;
        border: 1px solid #D1D5DB;
    }
    .actions .reset:hover {
        background: #F3F4F6
    }
    .ms-block {
        font-size: .56em;
    }
</style>
