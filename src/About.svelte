<script>
console.log($$props)
export let location;
console.log(location)
import {useLocation} from 'svelte-navigator'

const loc = useLocation();
console.log(loc)
console.log($loc)
</script>

<h1>
    about
</h1>