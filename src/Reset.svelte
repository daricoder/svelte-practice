<script>
    import {count} from './stores'

    const handleClick = () => {
        $count = 0;
    }
</script>

<button on:click={handleClick}>RESET</button>