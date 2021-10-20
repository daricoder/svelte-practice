<script>
import {Router,Route,Link} from 'svelte-navigator'
import About from './About.svelte'


let saludo = "Hola mundo"

</script>


<Router>

    <Link to="/about">
    about
    </Link>

    <Route path="/about" let:location>
        <About {location}></About>
    </Route>

    <Route path="/">
        <h1>hello world</h1>
    </Route>

    
</Router>
