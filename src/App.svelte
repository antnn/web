<script lang="ts">
  import { MaterialApp,AppBar} from "svelte-materialify";
  import PrivacyCenter from "./components/PrivacyCenter.svelte";
	let theme:'light' | 'dark' = 'light';
	function toggleTheme() {
		if (theme === 'light') theme = 'dark';
		else theme = 'light';
	}
  function getParam(p){
    var match = RegExp('[?&]' + p + '=([^&]*)').exec(window.location.search);
    return match && decodeURIComponent(match[1].replace(/\+/g, ' '));
}
</script>

<MaterialApp {theme}>
<div class="App">
  <header class="App-header" />
  <div>
    <button on:click={() => console.log(1)} />
  </div>
  <PrivacyCenter/>
</div>
</MaterialApp>

<style lang="scss">
  $font-stack: Helvetica, sans-serif;
  $primary-color: rgb(0, 0, 0);

  button {
    width: 100px;
    height: 100px;
  }
</style>
