<script>
  import { count } from "./stores";

  const handleClick = () => {
    $count = $count + 1;
  };
</script>

<button on:click={handleClick}>INCREMENT</button>
